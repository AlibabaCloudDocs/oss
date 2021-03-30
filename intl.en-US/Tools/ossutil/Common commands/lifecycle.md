# lifecycle

After you configure lifecycle rules, OSS converts the storage class of objects to Infrequent Access \(IA\), Archive, or Cold Archive, or deletes expired objects and parts to save storage costs on a regular basis. This topic describes how to add, modify, query, and delete lifecycle rule configuration by using the lifecycle command.

**Note:** For more information, see [Lifecycle rules](/intl.en-US/Developer Guide/Objects/Object lifecycle/Lifecycle rules.md).

## Add or modify lifecycle rules

The following steps show how to add or modify lifecycle rules:

1.  Create a local file and configure lifecycle rules in the file in the XML format.
2.  ossutil reads the lifecycle configurations from the local file, and then adds the read lifecycle configurations for the specified bucket.

-   Command syntax

    ```
    ./ossutil64 lifecycle --method put oss://bucket\_name local\_xml\_file
    ```

    The following table describes the parameters you can configure in the command.

    |Parameter|Description|
    |---------|-----------|
    |bucket\_name|The name of the bucket for which you want to add or modify lifecycle rules.|
    |local\_xml\_file|The local file name of the lifecycle rule. Example: `localfile.txt`.|

-   Examples

    **Note:** You can add multiple lifecycle rules for a bucket, and the rule ID is the unique identifier of the lifecycle configurations. If the lifecycle rule ID already exists, HTTP status code 409 is returned.

    1.  Create a file named `localfile.txt` on the local computer and write different lifecycle rules to the file.

        Common lifecycle rules:

        -   Example 1

            Specify that the lifecycle rule applies to all objects in the examplebucket destination bucket \(Prefix is null\), and OSS deletes the object 365 days after the object is last modified. Additionally, Prefix is specified as test/, which indicates that the storage class of the object that matches the test/ prefix is converted to Archive 30 days after the object is last modified.

            ```
            <?xml version="1.0" encoding="UTF-8"?>
            <LifecycleConfiguration>
              <Rule>
                <ID>test-rule1</ID>
                <Prefix></Prefix>
                <Status>Enabled</Status>
                <Expiration>
                  <Days>365</Days>
                </Expiration>
              </Rule>
              <Rule>
                <ID>test-rule2</ID>
                <Prefix>test/</Prefix>
                <Status>Enabled</Status>
                <Transition>
                  <Days>30</Days>
                  <StorageClass>Archive</StorageClass>
                </Transition>
              </Rule>
            </LifecycleConfiguration>
            ```

        -   Example 2

            Specify that the lifecycle rule applies to all objects in the examplebucket destination bucket \(Prefix is null\). Objects that are modification before December 30, 2019 expire.

            ```
            <?xml version="1.0" encoding="UTF-8"?>
            <LifecycleConfiguration>
              <Rule>
                <ID>test-rule0</ID>
                <Prefix></Prefix>
                <Status>Enabled</Status>
                <Expiration>
                  <CreatedBeforeDate>2019-12-30T00:00:00.000Z</CreatedBeforeDate>
                </Expiration>
              </Rule>
            </LifecycleConfiguration>
            ```

        -   Example 3

            When versioning is enabled, the storage class of objects in the examplebucket destination bucket is converted to IA 10 days after objects are last modified. Objects are converted to Archive 60 days after they become a previous version. Objects are deleted 90 days after they become a previous version.

            ```
            <?xml version="1.0" encoding="UTF-8"?>
            <LifecycleConfiguration>
              <Rule>
                <ID>test-rule3</ID>
                <Prefix></Prefix>
                <Status>Enabled</Status>
                <Transition>
                  <Days>10</Days>
                  <StorageClass>IA</StorageClass>
                </Transition>
                <NoncurrentVersionTransition>
                  <NoncurrentDays>60</NoncurrentDays>
                  <StorageClass>Archive</StorageClass>
                </NoncurrentVersionTransition>
                <NoncurrentVersionExpiration>
                  <NoncurrentDays>90</NoncurrentDays>
                </NoncurrentVersionExpiration>
              </Rule>
            </LifecycleConfiguration>
            ```

    2.  Add inventories for examplebucket.

        ```
        ./ossutil64 lifecycle --method put oss://examplebucket localfile.txt
        ```

        The following result indicates that the lifecycle rule is added:

        ```
        0.299514(s) elapsed
        ```


## Query lifecycle configurations

-   Command syntax

    ```
    ./ossutil64 lifecycle --method get oss://bucket\_name [local\_xml\_file]
    ```

    The following table describes the parameters you can configure in the command.

    |Parameter|Description|
    |---------|-----------|
    |bucket\_name|Queries the name of the bucket for which lifecycle rules are configured.|
    |local\_xml\_file|The name of the local file used to store lifecycle rule configurations. Example: `localfile.txt`. If this parameter is not specified, the lifecycle rule configurations are displayed on the screen.|

-   Examples

    Query the lifecycle rule of examplebucket.

    ```
    ./ossutil64 lifecycle --method get oss://examplebucket localfile.txt
    ```

    The following result indicates that the lifecycle rule configurations are queried and written to the localfile.txt local file:

    ```
    0.212407(s) elapsed
    ```


## Delete lifecycle rule configurations

-   Command syntax

    ```
    ./ossuitl64 lifecycle --method delete oss://bucket\_name
    ```

-   Examples

    Delete the lifecycle rule configurations of examplebucket.

    ```
    ./ossuitl64 lifecycle --method delete oss://examplebucket
    ```

    The following result indicates that the lifecycle rule configurations of examplebucket are deleted:

    ```
    0.530750(s) elapsed
    ```


## Common options

To use command-line tool ossutil to manage buckets that reside in different regions, you can use the -e option to switch to the endpoint of the specified bucket. To use command-line tool ossutil to manage buckets within multiple Alibaba Cloud accounts, you can use the -i option to switch to the AccessKey ID of the specified account and use the -k option to switch to the AccessKey secret of the specified account.

To configure lifecycle rules for the examplebucket bucket in the China \(Hangzhou\) region within another Alibaba Cloud account, run the following command:

```
./ossutil64 lifecycle --method put oss://examplebucket localfile.txt -e oss-cn-hangzhou.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

For more information about other common options, see [Common options](/intl.en-US/Tools/ossutil/View options.md).

