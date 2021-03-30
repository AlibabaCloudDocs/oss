# mb

A bucket is a container for objects stored in OSS. You must create a bucket before you upload an object to OSS. This topic describes how to create a bucket by using the mb command.

## Usage notes

Each sample command line in this topic is based on the 64-bit Linux system. For other systems, replace ./ossutil64 of the command with the corresponding binary name. For example, for the 64-bit Windows system, replace ./ossutil64 with ossutil64.exe. The following table lists the binary names corresponding to each system.

|System|Binary name|
|------|-----------|
|64-bit Linux|./ossutil64|
|32-bit Linux|./ossutil32|
|64-bit Windows|ossutil64.exe|
|32-bit Windows|ossutil32.exe|
|64-bit macOS|./ossutilmac64|
|32-bit macOS|./ossutilmac32|
|64-bit ARM|./ossutilarm64|
|32-bit ARM|./ossutilarm32|

## Command syntax

```
./ossutil64 mb oss://bucket\_name [--acl <value>][--storage-class <value>][--redundancy-type <value>]
```

The following table describes the parameters you can configure in the command.

|Parameter|Description|
|---------|-----------|
|bucket\_name|The name of the bucket that you want to create. A bucket name must be globally unique within OSS. Bucket names cannot be changed after the buckets are created.|
|--acl|The access control list \(ACL\) for a bucket. Default value: private. Valid values:-   private: Only the bucket owner can perform read and write operations on objects in the bucket. Other users cannot access the objects in the bucket.
-   public-read: Only the bucket owner can perform write operations on objects in the bucket. Other users, including anonymous users, can perform only read operations on the objects in the bucket. If you set the ACL to this value, data leakage and additional fees may occur. If a user writes illegal information to your objects, your legitimate interests and rights may be infringed. We recommend that you do not set the ACL to public read/write except in special scenarios.
-   public-read-write: All users, including anonymous users, can perform read and write operations on the objects in the bucket. This may lead to data leakage and additional fees. Proceed with caution. |
|--storage-class|The bucket storage class. Default value: Standard. Valid values:-   Standard: handles frequent data access.
-   IA: is suitable for long-term storage of data that is infrequently accessed Data that is accessed once or twice per month on average falls into this category. Objects of the IA storage class have a minimum storage period of 30 days and a minimum billable size of 64 KB. You can access objects of the IA storage class in real time. You are charged for the data retrieval.
-   Archive: applies to business scenarios that store data for a long period of time. Objects of the Archive storage class have a minimum storage period of 60 days and a minimum billable size of 64 KB. You must restore an object of the Archive storage class before you can access it. The restoration takes about one minute, and you are charged for the data retrieval.
-   ColdArchive: is suitable for long-term storage of data that is infrequently accessed. Objects of the Cold Archive storage class have a minimum storage period of 180 days and a minimum billable size of 64 KB. You must restore an object of the Cold Archive storage class before you can access it. The time required to restore a Cold Archive object depends on the object size and the restore mode. You are charged for the data retrieval when you restore a Cold Archive object.

For more information about storage classes, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md). |
|--redundancy-type|The type of disaster recovery for a bucket. Default value: LRS. Valid values:-   LRS: If you specify locally redundant storage \(LRS\), OSS stores the copies of each object across different devices within the same zone. This way, OSS ensures data reliability and availability when hardware failures occur.
-   ZRS: Zone-redundant storage \(ZRS\) uses the multi-zone mechanism to distribute user data across three zones within the same region. Even if one zone becomes unavailable due to failures such as power outages and fires, the data is still accessible.

**Note:** ZRS is supported in the following regions: China \(Shenzhen\), China \(Beijing\), China \(Hangzhou\), China \(Shanghai\), China \(Hong Kong\), and Singapore. |

## Examples

-   Create a bucket named examplebucket.

    ```
    ./ossutil64 mb oss://examplebucket
    ```

    If the region where the bucket is to be located is not specified when you create a bucket, the bucket is created in the region to which the endpoint in the ossutil configuration file points . For example, if the endpoint in the configuration file is `https://oss-cn-hangzhou.aliyuncs.com`, a bucket is created in the China \(Hangzhou\) region.

-   Create a bucket named examplebucket, and specify that the ACL is private, the storage class is Infrequent Access \(IA\), and the data recovery type is ZRS.

    ```
    ./ossutil64 mb oss://examplebucket --acl private --storage-class IA --redundancy-type ZRS
    ```

-   The following result indicates that the bucket that meets the specified conditions is created:

    ```
    0.335189(s) elapsed
    ```


## Common options

To use command-line tool ossutil to manage buckets that reside in different regions, you can use the -e option to switch to the endpoint of the specified bucket. To use command-line tool ossutil to manage buckets that belong to multiple Alibaba Cloud accounts, you can use the -i option to switch to the AccessKey ID of the specified account, and use the -k option to switch to the AccessKey secret of the specified account.

To set the encryption method to AES256 for a bucket named examplebucket in the China \(Hangzhou\) region for a different Alibaba Cloud account, run the following command:

```
./ossutil64 mb oss://examplebucket -e oss-cn-shanghai.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

For more information about other common options, see [Common options](/intl.en-US/Tools/ossutil/View options.md).

