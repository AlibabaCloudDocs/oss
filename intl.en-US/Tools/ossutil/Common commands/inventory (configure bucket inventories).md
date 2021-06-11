# inventory \(configure bucket inventories\)

You can use the bucket inventory feature to export the information about specific objects in a bucket, such as the number, sizes, storage classes, and encryption status of the objects. Compared with the GetBucket \(ListObjects\) operation, we recommend that you use the bucket inventory feature to list a large number of objects. This topic describes how to run the inventory command to add, query, list, or delete bucket inventories.

**Note:**

-   Sample command lines in this topic are based on the 64-bit Linux system. For other systems, replace ./ossutil64 in the commands with the corresponding binary name. For more information, see [ossutil](/intl.en-US/Quick Start/ossutil.md).
-   For more information about the bucket inventory feature, see [Bucket inventory](/intl.en-US/Developer Guide/Buckets/Bucket inventory.md).

## Add inventories

You can perform the following steps to add an inventory for a bucket:

1.  Create a RAM role and authorize the role to read all objects in the source bucket and write objects in the destination bucket in which inventory lists are generated. For more information about how to create a RAM role, see [Create a RAM role for a trusted Alibaba Cloud service](/intl.en-US/RAM Role Management/Create a RAM role/Create a RAM role for a trusted Alibaba Cloud service.md).
2.  Create a local file and configure inventories in the XML format in the file.
3.  Use ossutil to read the inventories from the local file, and then add the inventories to the specified bucket.

-   Command syntax

    ```
    ./ossutil64 inventory --method put oss://bucketname local\_xml\_file
    ```

    The following table describes the parameters that you can configure when you run this command.

    |Parameter|Description|
    |---------|-----------|
    |bucketname|The name of the bucket for which you want to add inventories.|
    |local\_xml\_file|The name of the local file in which the inventory is configured. Example: `localfile.txt`.|

-   Examples
    1.  Create a file named localfile.txt on the local computer and write different inventories to the file.

        The following example shows an inventory named inventorytest. Based on this inventory, OSS exports the information about all objects in a bucket named destbucket, including the storage classes, last update date, and multipart upload status of the objects. Exported inventory lists are encrypted by using the AES256 algorithm.

        ```
        <?xml version="1.0" encoding="UTF-8"?>
          <InventoryConfiguration>
              <Id>inventorytest</Id>
              <IsEnabled>true</IsEnabled>
              <Filter></Filter>
              <Destination>
                  <OSSBucketDestination>
                      <Format>CSV</Format>
                      <AccountId>1746495857602745</AccountId>
                      <RoleArn>acs:ram::174649585760****:role/AliyunOSSRole</RoleArn>
                      <Bucket>acs:oss:::destbucket</Bucket>
                      <Encryption>
                          <SSE-OSS></SSE-OSS>
                      </Encryption>
                  </OSSBucketDestination>
              </Destination>
              <Schedule>
                  <Frequency>Weekly</Frequency>
              </Schedule>
              <IncludedObjectVersions>All</IncludedObjectVersions>
              <OptionalFields>
                  <Field>LastModifiedDate</Field>
                  <Field>StorageClass</Field>
                  <Field>IsMultipartUploaded</Field>
                  <Field>ETag</Field>
                  <Field>EncryptionStatus</Field>
                  <Field>Size</Field>
              </OptionalFields>
          </InventoryConfiguration>
        ```

        **Note:** You can add multiple inventories for a bucket. The inventories are uniquely identified by their IDs. If the ID of the inventory you want to add is the same as that of an existing inventory, HTTP status code 409 is returned.

    2.  Add the inventories for a bucket named examplebucket.

        ```
        ./ossutil64 inventory --method put oss://examplebucket localfile.txt
        ```

        If a similar output is displayed, the inventories are added for examplebucket.

        ```
        0.299514(s) elapsed
        ```


## Query a specified inventory

-   Command syntax

    ```
    ./ossutil64 inventory --method get oss://bucketname inventory\_id [--local\_xml\_file ]
    ```

    The following table describes the parameters that you can configure when you run this command.

    |Parameter|Description|
    |---------|-----------|
    |bucketname|The name of the bucket whose inventories you want to query.|
    |inventory\_id|The name of the inventory you want to query.|
    |local\_xml\_file|The name of the local file used to store the inventory obtained inventory. Example: `localfile.txt`. If this parameter is not specified, obtained inventories are displayed without being stored in a local file.|

-   Examples

    ```
    ./ossutil64 inventory --method get oss://examplebucket inventorytest localfile.txt
    ```

    If a similar output is displayed, an inventory named inventorytest is configured for the bucket named examplebucket, and the inventory is written to a local file named localfile.txt.

    ```
    0.212407(s) elapsed
    ```


## Query all inventory rules configured for a bucket

-   Command syntax

    ```
    ./ossutil64 inventory --method list oss://bucketname [--local\_xml\_file ] [--marker <value>]
    ```

    The following table describes the parameters that you can configure when you run this command.

    |Parameter|Description|
    |---------|-----------|
    |bucketname|The name of the bucket of which inventories you want to query.|
    |local\_xml\_file|The name of the local XML file used to store the obtained inventories. If this parameter is not specified, obtained inventories are displayed without being stored in a local file.|
    |marker|The filtering conditions for inventories. Inventory lists are generated for only objects whose names contain the specified prefix. If this parameter is not specified, inventory lists are generated for all objects in the bucket.|

-   Examples

    ```
    ./ossutil64 inventory --method list oss://examplebucket localfile.txt dest
    ```

    If a similar output is displayed, all inventories that are configured for a bucket named examplebucket and apply to objects whose names contained the dest prefix are obtained and written to a local file named localfile.txt.

    ```
    0.216897(s) elapsed
    ```


## Delete a specified inventory

-   Command syntax

    ```
    ./ossutil64 inventory --method delete oss://bucketname inventory\_id
    ```

    The following table describes the parameters that you can configure when you run this command.

    |Parameter|Description|
    |---------|-----------|
    |bucketname|The name of the bucket for which you want to delete the inventory.|
    |inventory\_id|The name of the inventory you want to delete.|

-   Examples

    ```
    ./ossutil64 inventory --method delete oss://examplebucket inventorytest
    ```

    If a similar output is displayed, an inventory named inventorytest is deleted for a bucket named examplebucket.

    ```
    0.212407(s) elapsed
    ```


## Common options

To use ossutil to manage buckets that are located in different regions, you can use the -e option to use the endpoint of the specified bucket. To use ossutil to manage buckets that are owned by different Alibaba Cloud accounts, you can use the -i option to use the AccessKey ID of the specified account, and use the -k option to use the AccessKey secret of the specified account.

For example, you can run the following command to configure inventories for a bucket named examplebucket, which is located in the China \(Hangzhou\) region and owned by another Alibaba cloud account:

```
./ossutil64 inventory --method put oss://examplebucket local_xml_file -e oss-cn-hangzhou.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

For more information about other common options that apply to the inventory command, see [Common options](/intl.en-US/Tools/ossutil/View options.md).

