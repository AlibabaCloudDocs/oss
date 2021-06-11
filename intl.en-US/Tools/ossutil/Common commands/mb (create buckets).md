# mb \(create buckets\)

A bucket is a container for objects stored in OSS. You must create a bucket before you upload an object to OSS. This topic describes how to run the mb command to create buckets.

**Note:** Sample command lines in this topic are based on the 64-bit Linux system. For other systems, replace ./ossutil64 in the commands with the corresponding binary name. For more information, see [ossutil](/intl.en-US/Quick Start/ossutil.md).

## Command syntax

```
./ossutil64 mb oss://bucketname [--acl <value>][--storage-class <value>][--redundancy-type <value>]
```

The following table describes the parameters that you can configure when you run this command.

|Parameter|Description|
|---------|-----------|
|bucketname|The name of the bucket that you want to create. A bucket name must be globally unique within Object Storage Service \(OSS\). The name of a bucket cannot be changed after the bucket is created.|
|--acl|The access control list \(ACL\) of the bucket. Default value: private. Valid values:-   private: Only the bucket owner can perform read and write operations on objects in the bucket. Other users cannot access the objects in the bucket.
-   public-read: Only the bucket owner can perform write operations on objects in the bucket. Other users, including anonymous users, can perform only read operations on the objects in the bucket. If you set the bucket ACL to this value, data leakage may occur and you may be charged additional fees. Exercise caution when you set the bucket ACL to this value.
-   public-read-write: All users, including anonymous users, can perform read and write operations on the objects in the bucket. If you set the bucket ACL to this value, data leakage may occur and you may be charged additional fees. If a user writes illegal information to your objects, your legitimate interests and rights may be infringed. We recommend that you do not set the bucket ACL to this value except in special scenarios. |
|--storage-class|The storage class of the bucket. Default value: Standard. Valid values:-   Standard: This storage class can handle frequent data access.
-   IA: This storage class is suitable for long-term storage of data that is infrequently accessed \(once or twice each month\). Objects of the IA storage class have a minimum storage period of 30 days and a minimum billable size of 64 KB. You can access objects of the IA storage class in real time. You are charged for data retrieval fees when you access IA objects.
-   Archive: This storage class applies to scenarios that store data for a long period of time. Objects of the Archive storage class have a minimum storage period of 60 days and a minimum billable size of 64 KB. You must restore an Archive object before you can access it. The restoration takes about one minute, and you are charged for data retrieval fees.
-   ColdArchive: This storage class is suitable for long-term storage of data that is barely accessed. Objects of the Cold Archive storage class have a minimum storage period of 180 days and a minimum billable size of 64 KB. You must restore an object of the Cold Archive storage class before you can access it. The time required to restore a Cold Archive object depends on the object size and the restoration mode. You are charged for data retrieval fees when you restore Cold Archive objects.

For more information about storage classes, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md). |
|--redundancy-type|The redundancy type of the bucket. Valid values:-   LRS: If you set the redundancy type of a bucket to locally redundant storage \(LRS\), OSS stores the copies of each object across different devices within the same zone. This way, OSS ensures data reliability and availability when hardware failures occur.
-   ZRS: If you set the redundancy type of a bucket to zone-redundant storage \(ZRS\), OSS uses the multi-zone mechanism to distribute user data across three zones within the same region. This way, the data can be accessed even if one zone becomes unavailable due to failures such as power outages and fires.

**Note:** ZRS is supported in the following regions: China \(Shenzhen\), China \(Beijing\), China \(Hangzhou\), China \(Shanghai\), China \(Hong Kong\), and Singapore. |

## Examples

-   You can run the following command to create a bucket named examplebucket:

    ```
    ./ossutil64 mb oss://examplebucket
    ```

    If you do not specify the region in which you want to create the bucket, the bucket is created in the region specified by the endpoint in the ossutil configuration file. For example, if the endpoint specified in the configuration file is `https://oss-cn-hangzhou.aliyuncs.com`, the bucket is created in the China \(Hangzhou\) region.

-   You can run the following command to create a bucket named examplebucket and set the bucket ACL to private, storage class to IA, and redundancy type to ZRS.

    ```
    ./ossutil64 mb oss://examplebucket --acl private --storage-class IA --redundancy-type ZRS
    ```

-   If a similar output is displayed, the specified bucket is created.

    ```
    0.335189(s) elapsed
    ```


## Common options

To use ossutil to manage buckets that are located in different regions, you can use the -e option to use the endpoint of the specified bucket. To use ossutil to manage buckets that are owned by different Alibaba Cloud accounts, you can use the -i option to use the AccessKey ID of the specified account, and use the -k option to use the AccessKey secret of the specified account.

For example, you can run the following command to create a bucket named examplebucket, which is located in the China \(Shanghai\) region and owned by another Alibaba Cloud account.

```
./ossutil64 mb oss://examplebucket -e oss-cn-shanghai.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

For more information the mb command, see [Common options](/intl.en-US/Tools/ossutil/View options.md).

