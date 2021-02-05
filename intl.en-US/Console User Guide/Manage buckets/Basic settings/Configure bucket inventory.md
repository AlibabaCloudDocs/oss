# Configure bucket inventory

OSS provides the bucket inventory feature to regularly export information about objects in a bucket and store that information as an object in a specified bucket. This feature can help you understand the status of objects in your buckets and simplify and speed up workflows and big data tasks.

After you enable the bucket inventory feature, OSS scans objects in your bucket on a weekly basis, generates an inventory list in the CSV format, and stores the list as an object in the specified bucket. You can specify object metadata to be exported to the inventory list, such as object size and encryption status. For more information about the path where inventory lists are stored and the meaning of each column in an inventory list, see [Bucket inventory](/intl.en-US/Developer Guide/Buckets/Bucket inventory.md).

**Note:**

-   You can configure and display up to 10 inventories in the console.
-   You are charged if you use the bucket inventory feature. However, only storage fees for inventory lists and API calling fees are charged during public preview.

If you use a RAM user to configure bucket inventory, the following permissions are required:

-   The RAM user must have permissions to call the following operations: PutBucketInventory, GetBucketInventory, ListBucketInventory, and DeleteBucketInventory.
-   The RAM user must have the CreateRole permissions if the Alibaba Cloud account does not have the AliyunOSSRole RAM role.
-   The RAM user must have the GetRole permissions if the Alibaba Cloud account has the AliyunOSSRole RAM role.

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  Choose **Basic Settings** \> **Bucket Inventory**. In the Bucket Inventory section, click **Configure**.

4.  Click **Create Inventory**. In the Create Inventory dialog box that appears, configure the following parameters to create an inventory.

    |Parameter|Description|
    |---------|-----------|
    |**Status**|Configure the status of the inventory. You can set the status of the inventory to **Enabled** or **Disabled**.|
    |**Rule Name**|Configure the name of the inventory. The name can contain only lowercase letters, digits, and hyphens \(-\) and cannot start or end with a hyphen \(-\).|
    |**Destination Bucket**|Configure the bucket that stores inventory lists. You can select only the bucket within the same region as the bucket for which the inventory is configured.|
    |**Destination Prefix**|Configure the prefix contained in the names of inventory lists. You can specify the folder where inventory lists are stored by configuring this parameter. If you set the prefix to test, OSS stores generated inventory lists in the test folder in the destination bucket.|
    |**Frequency**|Specify the frequency at which inventory lists are generated. Select **Weekly**. OSS generates inventory lists at the specified frequency. **Note:**

    -   Currently, **Daily** is not available. It will be available in the future.
    -   If the number of objects in the source bucket is more than 10 billion, we recommend that you configure different inventories for objects whose names contain different prefixes and set the frequency to weekly. Ensure that the number of objects to which an inventory applies is at most 10 billion.
    -   We recommend that you delete inventory lists that are no longer needed in a timely manner to save storage space. |
    |**Encryption Method**|Specify the method used to encrypt inventory lists.     -   **None**: Inventory lists are not encrypted.
    -   **AES-256**: Inventory lists are encrypted by using AES-256.
    -   **KMS**: Inventory lists are encrypted by using a customer master key \(CMK\) managed by KMS. To use a CMK to encrypt inventory lists, you must create a CMK in KMS in the same region as the destination bucket. For more information, see [Manage CMKs]().

**Note:** You are charged for calling API operations when you use CMKs to encrypt or decrypt data. |
    |**Object Versions**|Select the object version to which the inventory is applied.     -   **Current Version**: exports the information about the current version of the specified objects to inventory lists.
    -   **All Versions**: exports the information about all versions of the specified objects to inventory lists.
 For more information about versioning, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md). |
    |**Source Prefix**|Specify the prefix contained in the names of the objects to which the inventory is applied. Only objects whose names contain the specified prefix are scanned to generate inventory lists. If you do not specify the prefix, all objects in the bucket are scanned to generate inventory lists. **Note:** If objects whose names contain the specified prefix do not exist in the bucket, inventory lists are not generated. |
    |**Optional Fields**|Select the object information that you want to export to inventory lists. You can select the following fields: **Object Size**, **Storage Class**, **Last Update Time**, **ETag**, **Multipart Upload**, and **Encryption Status**.|

5.  Read and select **I understand the terms and agree to authorize Alibaba Cloud OSS to access the resources in my buckets**. Click **OK**.


