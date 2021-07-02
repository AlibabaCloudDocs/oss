# lifecycle \(manage lifecycle rules\)

After you configure lifecycle rules for a bucket, OSS \(Object Storage Service\) converts the storage class of objects in the bucket to Infrequent Access \(IA\), Archive, Cold Archive, or deletes expired objects and parts to save storage costs on a regular basis. This topic describes how to use the lifecycle command to add, modify, query, and delete lifecycle rules.

**Note:**

-   Sample command lines in this topic are based on the 64-bit Linux system. For other systems, replace ./ossutil64 in the commands with the corresponding binary name. For more information, see [ossutil](/intl.en-US/Quick Start/ossutil.md).
-   For more information about lifecycle rules, see [Lifecycle rules](/intl.en-US/Developer Guide/Buckets/Lifecycle/Lifecycle rules.md).

## Add or modify lifecycle rules

You can perform the following steps to add or modify a lifecycle rule:

1.  Create a local file and configure lifecycle rules in the XML format in the file.
2.  Use ossutil to read the lifecycle configurations from the local file, and then add the configurations to the specified bucket.

-   Command syntax

    ```
    ./ossutil64 lifecycle --method put oss://bucketname local\_xml\_file
    ```

    The following table describes the parameters that you can configure when you run this command.

    |Parameter|Description|
    |---------|-----------|
    |bucketname|The name of the bucket for which you want to add or modify lifecycle rules.|
    |local\_xml\_file|The name of the local file in which the lifecycle rule is configured. Example: `localfile.txt`.|

-   Examples

    **Note:** You can add multiple lifecycle rules for a bucket. Each rule is identified by their unique IDs. If the rule you want to add is already an existing lifecycle rule for a bucket, HTTP status code 409 is returned.

    1.  Create a file named `localfile.txt` on the local device and configure different lifecycle rules to the file.

        The following examples show how to configure common lifecycle rules:

        -   Example 1

            Specify that objects in a bucket named examplebucket are deleted 365 days after they are last modified, and convert objects whose names contain the text/ prefix to Archive objects 30 days after they are last modified.

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

            Specify that objects last modified before December 30, 2019 in a bucket named examplebucket as expired.

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

            Specify that objects in a versioned bucket named examplebucket are first converted to IA objects 10 days after their last modified date, converted to Archive objects after 60 days as IA objects, and then deleted after 90 days as Archive objects.

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

    2.  Run the following command to add the lifecycle rule to the examplebucket:

        ```
        ./ossutil64 lifecycle --method put oss://examplebucket localfile.txt
        ```

        If a similar output is displayed, the lifecycle rule is added to the bucket.

        ```
        0.299514(s) elapsed
        ```


## Query lifecycle rules

-   Command syntax

    ```
    ./ossutil64 lifecycle --method get oss://bucketname [local\_xml\_file]
    ```

    The following table describes the parameters that you can configure when you run this command.

    |Parameter|Description|
    |---------|-----------|
    |bucketname|The name of the bucket whose lifecycle rules you want to query.|
    |local\_xml\_file|The name of the local file used to store lifecycle rule configurations. Example: `localfile.txt`. If this parameter is not specified, the lifecycle rule configurations are displayed on the screen.|

-   Examples

    You can run the following command to query the lifecycle rules of examplebucket:

    ```
    ./ossutil64 lifecycle --method get oss://examplebucket localfile.txt
    ```

    If a similar output is displayed, the lifecycle rules of examplebucket are obtained and stored in localfile.txt.

    ```
    0.212407(s) elapsed
    ```


## Delete lifecycle rules

-   Command syntax

    ```
    ./ossutil64 lifecycle --method delete oss://bucketname
    ```

-   Examples

    You can run the following command to delete the lifecycle rules of examplebucket:

    ```
    ./ossutil64 lifecycle --method delete oss://examplebucket
    ```

    If a similar output is displayed, the lifecycle rules of examplebucket are deleted.

    ```
    0.530750(s) elapsed
    ```


## Common options

To use ossutil to manage buckets that are located in different regions, you can use the -e option to use the endpoint of the specified bucket. To use ossutil to manage buckets that are owned by different Alibaba Cloud accounts, you can use the -i option to use the AccessKey ID of the specified account, and use the -k option to use the AccessKey secret of the specified account.

For example, you can run the following command to configure lifecycle rules for a bucket named examplebucket, which is located in the China \(Hangzhou\) region and is owned by another Alibaba Cloud account.

```
./ossutil64 lifecycle --method put oss://examplebucket localfile.txt -e oss-cn-hangzhou.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

For more information about other common options that apply to the lifecycle command, see [Common options](/intl.en-US/Tools/ossutil/View options.md).

