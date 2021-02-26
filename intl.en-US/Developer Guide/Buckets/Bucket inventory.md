# Bucket inventory

You can use the bucket inventory feature to list specific objects within a bucket and obtain the information such as the number, sizes, storage classes, and encryption status of the objects. Compared with the ListObject operation, we recommend that you use the inventory feature to list a large number of objects.

## Feature description

After an inventory is configured, OSS generates inventory lists at the specified frequency. The following figure shows the tree structure of the folders for inventory list storage:

```
dest_bucket
 └──destination-prefix/
     └──src_bucket/
         └──inventory_id/
             ├──YYYY-MM-DDTHH-MMZ/
             │   ├──manifest.json
             │   └──manifest.checksum
             └──data/
                 └──745a29e3-bfaa-490d-9109-47086afcc8f2.csv.gz
```

|Directory|Description|
|---------|-----------|
|destination-prefix/|This folder is generated based on the prefix specified for inventory lists. If no prefix is specified for inventory lists, this folder is omitted.|
|src\_bucket/|This folder is generated based on the name of the source bucket for which inventory lists are generated.|
|inventory\_id/|The folder is generated based on the name of the inventory.|
|YYY-MM-DDTHH-MMZ/|This folder indicates the start time when the bucket is scanned. The name of this folder is a timestamp in GMT in the standard format. Example: 2020-05-17T16-00Z. The manifest.json and manifest.checksum objects are stored in this folder.|
|data/|This folder contains the inventory lists.|

After an inventory is created for a bucket, the following objects are generated based on the inventory:

-   Inventory lists

    Inventory lists contain the exported object information and are stored in the data folder. You can query the fileSchema field in manifest.json to obtain the field columns included in the inventory lists. The following figure shows an example of an inventory list:

    ![Inventory list](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0295688951/p104943.png)

    The following table describes the fields contained in the inventory list in sequence:

    |Field|Description|
    |-----|-----------|
    |Bucket|The name of the bucket for which the inventory is created.|
    |Key|The name of the object in the bucket.The object name is URL-encoded. You must decode the object name before you can view it. |
    |VersionId|The version ID of the object.This field exists only when versioning is enabled for the bucket and the inventory specifies that all versions of data are exported. |
    |IsLatest|This field indicates whether the version is the current version. If the version is the current version, the value is True. Othewise, the value is False.This field exists only when versioning is enabled for the bucket and the inventory specifies that all versions of data are exported. |
    |IsDeleteMarker|This field indicates whether the version is a delete marker. If the version is a delete marker, the value is True. Otherwise, the value is False.This field exists only when versioning is enabled for the bucket and the inventory specifies that all versions of data are exported. |
    |Size|The size of the object.|
    |LastModifiedDate|The time when the object is last modified.|
    |ETag|The ETag of the object.An ETag is generated when an object is created. ETags are used to identify the content of the objects.

    -   If an object is created by using [PutObject](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.md), the ETag of the object is the MD5 hash value of the object content.
    -   If an object is created by using other methods, the ETag of the object is the UUID of the object content. |
    |StorageClass|The storage class of the object.|
    |IsMultipartUploaded|This field indicates whether the object is created by using multipart upload. If the object is created by using multipart upload, the value is True. Otherwise, the value is False.|
    |EncryptionStatus|This field indicates whether the object is encrypted. If the object is encrypted, the value is True. Otherwise, the value is False.|

-   manifest objects

    manifest objects include manifest.json and manifest.checksum.

    -   manifest.json: stores the metadata of inventory lists and related information, including the following fields:

        |Field|Description|
        |-----|-----------|
        |creationTimestamp|The start time when the source bucket is scanned. The value of this field is a timestamp in the UNIX time format.|
        |destinationBucket|The destination bucket where the inventory lists are stored.|
        |fileFormat|The format of the inventory lists.|
        |fileSchema|The fields contained in each inventory list.|
        |files|This field contains the name, size, and MD5 hash value of each inventory list.|
        |sourceBucket|The source bucket for which the inventory lists are generated.|
        |version|The version of the inventory list.|

    -   manifest.checksum: stores the MD5 hash value of the manifest.json object.

## Usage notes

-   Billing
    -   You are charged if you use the bucket inventory feature. However, only storage fees for inventory lists and API calling fees are charged during public preview.
    -   OSS generates inventory lists based on the inventory until you delete the inventory. Fees are incurred for the storage of the inventory lists. To avoid unnecessary costs, delete inventory lists that are no longer needed.
-   Permissions

    To use a RAM user to configure inventories, you must have the following permissions:

    -   The RAM user must have permissions to call the following operations: PutBucketInventory, GetBucketInventory, ListBucketInventory, and DeleteBucketInventory.
    -   Before you configure inventories, you must create a RAM role to obtain the permissions to read all objects in the source bucket and write objects to the destination bucket. If your Alibaba Cloud account does not have this role, the RAM user must have the CreateRole and GetRole permissions. If your Alibaba Cloud account has this role, the RAM user must have the GetRole permission.

        For more information about how to configure a role, see [Create a RAM role for a trusted Alibaba Cloud service](/intl.en-US/RAM Role Management/Create a RAM role/Create a RAM role for a trusted Alibaba Cloud service.md).

-   Limits
    -   The source bucket for which an inventory is configured and the destination bucket where the inventory lists are stored do not have to be the same bucket, but they must belong to the same account and reside within the same region.
    -   You can configure up to 1,000 inventories for a bucket. A maximum of 10 inventories can be configured in the OSS console.
-   Recommended configurations

    We recommend that you configure inventories based on the number of objects in the source bucket:

    -   If the number of objects is smaller than 1 billion, OSS generates inventory lists in days.
    -   If the number of objects is between 1 billion to 10 billion, OSS generates inventory lists in weeks.
    -   If the number of objects is greater than 10 billion, we recommend that you configure different inventories based on different object prefixes in weeks, and ensure that OSS generates inventory lists for up to 10 billion objects based on each inventory.
-   Exceptions
    -   If no objects are stored in the bucket for which the inventory is configured or no objects can apply to the prefix specified in the inventory, inventory lists are not generated.
    -   When you export the inventory lists, the final lists may not contain all objects due to operations such as creation, deletion, or overwriting. If the last modified time of an object is earlier than the time specified by the createTimeStamp field in the manifest.json object, the inventory list contains information about the object. Otherwise, the inventory list may not contain information about the object. We recommend that you check the object attributes by using HeadObject before you export the information about an object. For more information, see [HeadObject](/intl.en-US/API Reference/Object operations/Basic operations/HeadObject.md).

## Implementation methods

|Implementation method|Description|
|---------------------|-----------|
|[Console](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure bucket inventory.md)|A user-friendly and intuitive web application|
|[ossutil](/intl.en-US/Tools/ossutil/Common commands/inventory.md)|A high-performance command-line tool|
|[Java SDK](/intl.en-US/SDK Reference/Java/Buckets/Bucket inventory.md)|SDK demos for various programming languages|
|[Python SDK](/intl.en-US/SDK Reference/Python/Buckets/Bucket inventory.md)|
|[Go SDK](/intl.en-US/SDK Reference/Go/Buckets/Manage inventory rules.md)|
|[C++ SDK](/intl.en-US/SDK Reference/C++/Buckets/Bucket inventory.md)|

## FAQ

How do I know whether an inventory list is generated?

It may take a while to generate an inventory list for a large number of objects. If you want to be informed when the inventory list is generated for objects, we recommend that you configure an event notification for the destination bucket in which the inventory list is stored and set the event to PutObject. When an inventory list is generated, you can receive a notification from OSS. For more information about how to configure event notifications, see [Configure event notification](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure event notification.md).

