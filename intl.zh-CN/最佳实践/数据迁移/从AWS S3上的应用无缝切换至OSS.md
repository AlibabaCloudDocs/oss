# 从AWS S3上的应用无缝切换至OSS

OSS提供了S3 API的兼容性，可以将您的数据从AWS S3无缝迁移至阿里云OSS。

## 注意事项

-   使用限制

    由于OSS兼容S3协议，因此您可以通过S3 SDK进行创建Bucket、上传Object等相关操作。执行相关操作过程中其带宽、QPS等限制遵循OSS性能指标，详情请参见[使用限制](/intl.zh-CN/产品简介/使用限制.md)。

-   客户端配置

    从AWS S3迁移到OSS后，您仍然可以使用S3 API访问OSS，仅需要对S3的客户端应用进行如下改动：

    1.  获取阿里云账号或RAM用户的AccessKey ID和AccessKey Secret，并在您使用的客户端和SDK中配置您申请的AccessKey ID与AccessKey Secret。
    2.  设置客户端连接的Endpoint为OSS Endpoint。OSS Endpoint列表请参见[访问域名和数据中心](/intl.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md)。

## 迁移教程

您可以使用[阿里云在线迁移服务](https://mgw.console.aliyun.com/job?_k=r90z7u#/job?_k=xdgp8r)将AWS S3 数据轻松迁移至阿里云对象存储OSS。详情请参见[AWS S3迁移教程]()。

## 迁移后通过S3 API访问OSS

从S3迁移到OSS后，您使用S3 API访问OSS时，需要注意以下几点：

-   Path style和Virtual hosted style

    [Virtual hosted style](http://docs.aws.amazon.com/AmazonS3/latest/dev/VirtualHosting.html)是指将Bucket置于Host Header的访问方式。基于安全考虑，OSS仅支持virtual hosted访问方式。所以在S3迁移至OSS后，客户端应用需要进行相应设置。部分S3工具默认使用Path style，也需要进行相应配置，否则可能导致OSS报错，并禁止访问。

-   ACL权限定义

    OSS对ACL权限的定义与S3不完全一致，迁移后如有需要，可对权限进行相应调整。二者的主要区别可参考下表：

    |级别|AWS S3权限|AWS S3|阿里云OSS|
    |:-|:-------|:-----|:-----|
    |Bucket|READ|拥有Bucket的list权限|对于Bucket下的所有Object，如果某Object没有设置Object权限，则该Object可读。|
    |WRITE|Bucket下的Object可写入或覆盖|    -   对于Bucket下不存在的Object，可写入。
    -   对于Bucket下存在的Object，如果该Object没有设置Object权限，则该Object可被覆盖。
    -   可以初始化分片上传（InitiateMultipartUpload）。 |
    |READ\_ACP|读取Bucket ACL|读取Bucket ACL，仅Bucket owner和授权子账号拥有此权限。|
    |WRITE\_ACP|设置Bucket ACL|设置Bucket ACL，仅Bucket owner和授权子账号拥有此权限。|
    |Object|READ|Object可读|Object可读。|
    |WRITE|N/A|Object可以被覆盖。|
    |READ\_ACP|读取Object ACL|读取Object ACL，仅Bucket owner和授权RAM用户拥有此权限。|
    |WRITE\_ACP|设置Object ACL|设置Object ACL，仅Bucket owner和授权RAM用户拥有此权限。|

    **说明：** OSS仅支持S3中的私有、公共读和公共读写三种ACL模式。

-   存储类型

    OSS支持标准（Standard）、低频访问（IA）和归档存储（Archive）三种存储类型，分别对应AWS S3中的STANDARD、STANDARD\_IA和GLACIER。您可以根据需要转换OSS Object的存储类型。

    归档存储类型的Object在读取之前，要先使用Restore请求进行解冻操作。与S3不同，OSS会忽略S3 API中的解冻天数设置，解冻状态默认持续1天，用户可以延长到最多7天，之后，Object又回到初始时的冷冻状态。

-   ETag
    -   对于PUT方式上传的Object，OSS Object的ETag与AWS S3在大小写上有区别。OSS为大写，而S3为小写。如果客户端有关于ETag的校验，请忽略大小写。
    -   对于分片上传的Object，OSS的ETag计算方式与S3不同。

## 兼容的S3 API

以下为OSS对 S3 Bucket、Object以及Multipart操作兼容的API列表。

|操作类型|API|
|----|---|
|Bucket操作|-   PutBucket
-   DeleteBucket
-   GetBucket
-   GetBucketACL
-   GetBucketLifecycle
-   GetBucketLocation
-   GetBucketLogging
-   HeadBucket
-   PutBucketACL
-   PutBucketLifecycle
-   PutBucketLogging |
|Object操作|-   DeleteObject
-   DeleteObjects
-   GetObject
-   GetObjectACL
-   HeadObject
-   PostObject
-   PutObject
-   PutObjectCopy
-   PutObjectACL |
|Multipart操作|-   InitiateMultipartUpload
-   AbortMultipartUpload
-   CompleteMultipartUpload
-   ListParts
-   UploadPart
-   UploadPartCopy |

