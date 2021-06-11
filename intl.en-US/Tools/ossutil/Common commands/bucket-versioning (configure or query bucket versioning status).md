# bucket-versioning \(configure or query bucket versioning status\)

OSS allows you to configure versioning for a bucket to protect objects stored in the bucket. After you enable versioning for a bucket, objects that are overwritten or deleted in the bucket are saved as previous versions. Versioning allows you to recover a previous version of an object to protect the object from being accidentally overwritten or deleted. This topic describes how to use the bucket-versioning command to configure or query the versioning status of a bucket.

**Note:** The commands described in this topic are for 64-bit Linux systems. To use the commands in the examples on 64-bit Windows systems, replace ./ossutil64 in the commands with ossutil64.exe.

## Configure the versioning status of a bucket

-   Command syntax

    ```
    ./ossutil64 bucket-versioning --method put oss://bucketname versioning
    ```

    The following table describes the parameters that you can configure when you run this command.

    |Parameter|Description|
    |---------|-----------|
    |bucketname|The name of the bucket whose versioning status you want to configure.|
    |versioning|The versioning status of the bucket that you want to configure. Valid values:    -   enabled: Enable versioning for the bucket. When an object is uploaded to a bucket that has versioning enabled, OSS generates a random string as the globally unique version ID of the object. For more information about how to manage objects in a versioned bucket, see [Manage objects in a versioning-enabled bucket](/intl.en-US/Developer Guide/Data security/Versioning/Manage objects in a versioning-enabled bucket.md).
    -   suspended: Suspend versioning for the bucket. When an object is uploaded to a bucket for which versioning is suspended, OSS generates a string null as the version ID of the object. For more information about how to manage objects in a bucket for which versioning is suspended, see [Manage objects in a versioning-suspended bucket](/intl.en-US/Developer Guide/Data security/Versioning/Manage objects in a versioning-suspended bucket.md).
**Note:** By default, the versioning status of a bucket is disabled. After versioning is enabled for a bucket, the versioning status of the bucket cannot be set back to disabled. However, you can suspend versioning for a versioned bucket. |

-   Examples

    You can run the following command to enable versioning for a bucket named examplebucket:

    ```
    ./ossutil64 bucket-versioning --method put oss://examplebucket enabled
    ```

    You can run the following command to suspend versioning for a bucket named examplebucket:

    ```
    ./ossutil64 bucket-versioning --method put oss://examplebucket suspended
    ```

    If a similar output is displayed, the versioning status of the bucket named examplebucket is configured.

    ```
    0.261209(s) elapsed
    ```


## Query the versioning status of a bucket

-   Command syntax

    ```
    ./ossutil64 bucket-versioning --method get oss://bucketname
    ```

-   Examples

    You can run the following command to query the versioning status of a bucket named examplebucket:

    ```
    ./ossutil64 bucket-versioning --method get oss://examplebucket
    ```

    If a similar output is displayed, versioning is enabled for the bucket.

    ```
    bucket versioning status:Enabled
    
    0.218001(s) elapsed
    ```

    If a similar output is displayed, versioning is suspended for the bucket named examplebucket.

    ```
    bucket versioning status:Suspended
    
    0.168791(s) elapsed
    ```

    If a similar output is displayed, versioning is disabled for the bucket named examplebucket.

    ```
    bucket versioning status:Null
    
    0.158691(s) elapsed
    ```


## Related operations

-   OSS manages an object that is uploaded to a versioned bucket and an unversioned bucket the same way. However, OSS generates a globally unique version ID for an object that is uploaded to a versioned bucket. For more information, see [Upload objects](/intl.en-US/Tools/ossutil/Common commands/cp/Upload objects.md).
-   After you enable versioning for a bucket, objects that are overwritten or deleted in the bucket are saved as previous versions. You can specify a version ID to download the specified version of an object. For more information, see [Download objects](/intl.en-US/Tools/ossutil/Common commands/cp/Download objects.md). You can specify a version ID to recover the specified previous version of an object. For more information, see [Copy objects](/intl.en-US/Tools/ossutil/Common commands/cp/Copy objects.md).

## Common options

To use ossutil to manage buckets that are located in different regions, you can use the -e option to use the endpoint of the specified bucket. To use ossutil to manage buckets that are owned by different Alibaba Cloud accounts, you can use the -i option to use the AccessKey ID of the specified account, and use the -k option to use the AccessKey secret of the specified account.

For example, you can run the following command to enable versioning for a bucket named examplebucket, which is located in the China \(Hangzhou\) region and owned by another Alibaba Cloud account:

```
./ossutil64 bucket-versioning--method put oss://examplebucket enabled -e oss-cn-hangzhou.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

For more information about other common options that apply to the bucket-versioning command, see [Common options](/intl.en-US/Tools/ossutil/View options.md).

