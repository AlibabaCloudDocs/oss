# inventory

You can use the inventory feature to export the information of specified objects in a bucket, such as the number, size, storage class, and encryption status. Compared with the GetBucket \(ListObjects\) operation, we recommend that you use the inventory feature to list a large number of objects. This topic describes how to add, query, list, or delete bucket inventory rules by using the inventory command.

**Note:** For more information about the inventory feature, see [Bucket inventory](/intl.en-US/Developer Guide/Buckets/Bucket inventory.md).

## Add inventory rules

The following steps show how to add inventory rules:

1.  You must create a RAM role to obtain the permissions to read all objects in the source bucket and write objects to the destination bucket. For more information about how to create a RAM role, see [Create a RAM role for a trusted Alibaba Cloud service](/intl.en-US/RAM Role Management/Create a RAM role/Create a RAM role for a trusted Alibaba Cloud service.md).
2.  Create a local file and configure inventory rules in the file in the XML format.
3.  ossutil reads the inventory configurations from the local file, and then adds the read inventory configurations to the specified bucket.

-   Command syntax

    ```
    ./ossutil64 inventory --method put oss://bucket\_name local\_xml\_file
    ```

    The following table describes the parameters you can configure in the command.

    |Parameter|Description|
    |---------|-----------|
    |bucket\_name|The name of the bucket to which inventories are added.|
    |local\_xml\_file|The name of the local file in which the inventory is configured. Example: `localfile.txt`.|

-   Examples
    1.  Create a file named localfile.txt on the local computer and write different inventories to the file.

        For example, set the name of the inventory to inventorytest, and export the inventory report to the destination bucket on a weekly basis. The inventory includes the storage class, last update time, and part upload status of objects in the destination bucket named destbucket. And then use the AES-256 encryption algorithm to encrypt the inventory list.

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

        **Note:** You can add multiple inventories for a bucket, and the ID is the unique identifier of the inventory configuration. If the inventory ID already exists, HTTP status code 409 is returned.

    2.  Add inventories for examplebucket.

        ```
        ./ossutil64 inventory --method put oss://examplebucket localfile.txt
        ```

        The following result indicates that the inventory is added:

        ```
        0.299514(s) elapsed
        ```


## Query a specified inventory

-   Command syntax

    ```
    ./ossutil64 inventory --method get oss://bucket\_name inventory\_id [--local\_xml\_file ]
    ```

    The following table describes the parameters you can configure in the command.

    |Parameter|Description|
    |---------|-----------|
    |bucket\_name|Queries the name of the bucket for which the inventory feature is configured.|
    |inventory\_id|The name of the inventory.|
    |local\_xml\_file|The name of the local file used to store the inventory configurations. Example: `localfile.txt`. If this parameter is not specified, the inventory configurations are displayed on the screen.|

-   Examples

    ```
    ./ossutil64 inventory --method get oss://examplebucket inventorytest localfile.txt
    ```

    The following result indicates that the inventory configurations whose inventory ID is inventorytest is queried for examplebucket. The inventory results are written to the local file named localfile.txt:

    ```
    0.212407(s) elapsed
    ```


## Query inventory rules

-   Command syntax

    ```
    ./ossutil64 inventory --method list oss://bucket\_name [--local\_xml\_file ] [--marker <value>]
    ```

    The following table describes the parameters you can configure in the command.

    |Parameter|Description|
    |---------|-----------|
    |bucket\_name|Queries the name of the bucket for which the inventory feature is configured.|
    |local\_xml\_file|The name of the local file in the XML format used to store the inventory configurations. If this parameter is not specified, the inventory configurations are displayed on the screen.|
    |marker|The inventory filter condition generates inventory lists for objects that match only the specified prefix. If this parameter is null, inventory lists are generated for all objects in the bucket.|

-   Examples

    ```
    ./ossutil64 inventory --method list oss://examplebucket localfile.txt dest
    ```

    The following result indicates that inventories in the file that matches the dest prefix in examplebucket are queried. The inventory results are written to the local localfile.txt file:

    ```
    0.216897(s) elapsed
    ```


## Delete a specified inventory

-   Command syntax

    ```
    ./ossutil64 inventory --method delete oss://bucket\_name inventory\_id
    ```

    The following table describes the parameters you can configure in the command.

    |Parameter|Description|
    |---------|-----------|
    |bucket\_name|The name of the bucket for which the inventory configurations is deleted.|
    |inventory\_id|The name of the inventory.|

-   Examples

    ```
    ./ossutil64 inventory --method delete oss://examplebucket inventorytest
    ```

    The following result indicates that the inventory content whose configuration rule ID is inventorytest in examplebucket is deleted:

    ```
    0.212407(s) elapsed
    ```


## Common options

To use command-line tool ossutil to manage buckets that reside in different regions, you can use the -e option to switch to the endpoint of the specified bucket. To use command-line tool ossutil to manage buckets that belong to multiple Alibaba Cloud accounts, you can use the -i option to switch to the AccessKey ID of the specified account, and use the -k option to switch to the AccessKey secret of the specified account.

For example, you must configure inventory rules for the examplebucket bucket in the China \(Hangzhou\) region within another Alibaba Cloud account. Command:

```
./ossutil64 inventory --method put oss://examplebucket local_xml_file -e oss-cn-hangzhou.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

For more information about other common options of this command, see [Common options](/intl.en-US/Tools/ossutil/View options.md).

