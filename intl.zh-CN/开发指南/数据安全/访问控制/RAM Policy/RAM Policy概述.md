# RAM Policy概述

RAM Policy是基于用户的授权策略。您可以使用RAM Policy控制用户可以访问您名下哪些资源的权限。

## 背景信息

-   RAM Policy语法和结构

    RAM Policy包含版本号（Version）、授权语句（Statement）、每条授权语句包括授权效力（Effect）、操作（Action）、资源（Resource）以及限制条件（Condition，可选项）。有关权限策略的语法和结构的详情，请参见[权限策略语法和结构](/intl.zh-CN/权限策略管理/权限策略语言/权限策略语法和结构.md)。

    其中OSS中Version、Statement以及Effect的应用规则与RAM相同。OSS的Action、Resource以及Condition说明请参见：

    -   [OSS Action分类](#section_x3c_nsm_2gb)
    -   [OSS Resource说明](#section_an0_sb1_5sh)
    -   [OSS Condition说明](#section_zhg_wsm_2gb)
-   OSS常用的权限策略
    -   AliyunOSSFullAccess：为RAM用户授予OSS的完全管理权限。
    -   AliyunOSSReadOnlyAccess：为RAM用户授予OSS的只读访问权限。
-   OSS访问控制方式

    有关OSS包含的访问控制方式的更多信息，请参见[访问控制概述](/intl.zh-CN/开发指南/数据安全/访问控制/访问控制概述.md)。


## OSS Action分类

Action分为Service级别操作、Bucket级别操作以及Object级别的操作。

-   Service级别

    Service级别操作对应的是GetService \(ListBuckets\)接口，用来列举所有属于该用户的Bucket列表。

    |API|Action|
    |:--|:-----|
    |GetService \(ListBuckets\)|oss:ListBuckets|

-   Bucket级别

    操作的对象均为Bucket，其Action名称和相应的接口名称一一对应。

    |API|Action|
    |:--|:-----|
    |PutBucket|oss:PutBucket|
    |GetBucket \(ListObjects\)|oss:ListObjects|
    |GetBucketVersions \(ListObjectVersions\)|oss:ListObjectVersions|
    |PutBucketVersioning|oss:PutBucketVersioning|
    |GetBucketVersioning|oss:GetBucketVersioning|
    |PutBucketAcl|oss:PutBucketAcl|
    |GetBucketAcl|oss:GetBucketAcl|
    |DeleteBucket|oss:DeleteBucket|
    |GetBucketLocation|oss:GetBucketLocation|
    |GetBucketInfo|oss:GetBucketInfo|
    |GetBucketLogging|oss:GetBucketLogging|
    |PutBucketLogging|oss:PutBucketLogging|
    |DeleteBucketLogging|oss:DeleteBucketLogging|
    |GetBucketWebsite|oss:GetBucketWebsite|
    |PutBucketWebsite|oss:PutBucketWebsite|
    |DeleteBucketWebsite|oss:DeleteBucketWebsite|
    |GetBucketReferer|oss:GetBucketReferer|
    |PutBucketReferer|oss:PutBucketReferer|
    |GetBucketLifecycle|oss:GetBucketLifecycle|
    |PutBucketLifecycle|oss:PutBucketLifecycle|
    |DeleteBucketLifecycle|oss:DeleteBucketLifecycle|
    |ListMultipartUploads|oss:ListMultipartUploads|
    |ListParts|oss:ListParts|
    |PutBucketCors|oss:PutBucketCors|
    |GetBucketCors|oss:GetBucketCors|
    |DeleteBucketCors|oss:DeleteBucketCors|
    |PutBucketVersioning|oss:PutBucketVersioning|
    |GetBucketVersions\(ListObjectVersions\)|oss::ListObjectVersions|
    |PutBucketPolicy|oss:PutBucketPolicy|
    |GetBucketPolicy|oss:GetBucketPolicy|
    |DeleteBucketPolicy|oss:DeleteBucketPolicy|
    |PutBucketTags|oss:PutBucketTagging|
    |GetBucketTags|oss:GetBucketTagging|
    |DeleteBucketTags|oss:DeleteBucketTagging|
    |PutBucketEncryption|oss:PutBucketEncryption|
    |GetBucketEncryption|oss:GetBucketEncryption|
    |DeleteBucketEncryption|oss:DeleteBucketEncryption|
    |PutBucketRequestPayment|oss:PutBucketRequestPayment|
    |GetBucketRequestPayment|oss:GetBucketRequestPayment|
    |PutBucketReplication|oss:PutBucketReplication|
    |GetBucketReplication|oss:GetBucketReplication|
    |DeleteBucketReplication|oss:DeleteBucketReplication|
    |GetBucketReplicationLocation|oss:GetBucketReplicationLocation|
    |GetBucketReplicationProgress|oss:GetBucketReplicationProgress|

-   Object级别

    操作对象均为Object，各Action名称与接口的对应关系如下所示。

    |API|Action|
    |:--|:-----|
    |PutObject|oss:PutObject|
    |PostObject|
    |InitiateMultipartUpload|
    |UploadPart|
    |CompleteMultipart|
    |AppendObject|
    |CompleteMultipartUpload|
    |PutSymlink|
    |GetObject|oss:GetObject|
    |HeadObject|
    |GetObjectMeta|
    |SelectObject|
    |GetSymlink|
    |DeleteObject|oss:DeleteObject|
    |DeleteMultipleObjects|
    |CopyObject|oss:GetObject,oss:PutObject|
    |UploadPartCopy|
    |GetObjectAcl|oss:GetObjectAcl|
    |PutObjectAcl|oss:PutObjectAcl|
    |RestoreObject|oss:RestoreObject|
    |PutObjectTagging|oss:PutObjectTagging|
    |GetObjectTagging|oss:GetObjectTagging|
    |DeleteObjectTagging|oss:DeleteObjectTagging|
    |GetObject（请求参数中指定versionId）|oss:GetObjectVersion|
    |PutObjectACL（请求参数中指定versionId）|oss:PutObjectVersionAcl|
    |GetObjectAcl（请求参数中指定versionId）|oss:GetObjectVersionAcl|
    |RestoreObject（请求参数中指定versionId）|oss:RestoreObjectVersion|
    |DeleteObject（请求参数中指定versionId）|oss:DeleteObjectVersion|
    |PutObjectTagging（请求参数中指定versionId）|oss:PutObjectVersionTagging|
    |GetObjectTagging（请求参数中指定versionId）|oss:GetObjectVersionTagging|
    |DeleteObjectTagging（请求参数中指定versionId）|oss:DeleteObjectVersionTagging|
    |PutLiveChannel|oss:PutLiveChannel|
    |ListLiveChannel|oss:ListLiveChannel|
    |DeleteLiveChannel|oss:DeleteLiveChannel|
    |PutLiveChannelStatus|oss:PutLiveChannelStatus|
    |GetLiveChannelInfo|oss:GetLiveChannel|
    |GetLiveChannelStat|oss:GetLiveChannelStat|
    |GetLiveChannelHistory|oss:GetLiveChannelHistory|
    |PostVodPlaylist|oss:PostVodPlaylist|
    |GetVodPlaylist|oss:GetVodPlaylist|
    |ProcessImm|oss:ProcessImm|
    |ImgSaveAs|oss:PostProcessTask|
    |AbortMultipartUpload|oss:AbortMultipartUpload|


## OSS Resource说明

在OSS中，Resource指代某个具体资源或者某些资源，支持通配符星号（\*）。单个RAM Policy允许包含多个Resource。

Resource规则：`acs:oss:{region}:{bucket_owner}:{bucket_name}/{object_name}`

针对Bucket级别的Resource设置，不需要在`{bucket_name}`之后添加正斜线（/）以及`{object_name}`，即`acs:oss:{region}:{bucket_owner}:{bucket_name}`。region字段当前仅支持设置为通配符星号（\*）。

## OSS Condition说明

Condition代表Policy授权的条件。OSS支持的Condition如下：

|Condition|说明|
|:--------|:-|
|acs:SourceIp|指定普通IP网段，支持通配符星号（\*）。|
|acs:UserAgent|指定HTTP User-Agent头。类型：字符串 |
|acs:CurrentTime|请求到达OSS服务端的时间。格式：ISO8601 |
|acs:SecureTransport|请求的协议类型。如果请求是HTTP协议，则为HTTP，如果请求是HTTPS协议，则为HTTPS。|
|oss:Prefix|用于ListObjects请求时，列举指定前缀的Object。|
|oss:Delimiter|用于ListObjects请求时，对Object名字进行分组的字符。|
|acs:AccessId|请求中携带的AccessId。|
|oss:BucketTag|存储空间标签（BucketTag）。单个BucketTag可以作为一个Condition。当设置多个BucketTag时，需在每个BucketTag前加上`oss:BucketTag/`，组成多个Condition。 |
|acs:MFAPresent|是否启用了多因素认证MFA（Multi-factor authentication）。取值：

-   true：已启用多因素认证。
-   false：未启用多因素认证。 |
|oss:ExistingObjectTag|请求的Object已存在标签。单个ObjectTag可以作为一个Condition。当设置多个ObjectTag时，需在每个ObjectTag前加上`oss:ExistingObjectTag/`，组成多个Condition。

主要针对GetObject、HeadObject等读取文件接口以及PutObjectTagging、GetObjectTagging等ObjectTagging接口。 |
|oss:RequestObjectTag|请求中携带的对象标签。单个ObjectTag可以作为一个Condition。当设置多个ObjectTag时，需在每个ObjectTag前加上`oss:RequestObjectTag/`，组成多个Condition。

主要针对PutObject、PostObject等写入文件接口以及PutObjectTagging、GetObjectTagging等ObjectTagging接口。 |

## 常见示例

您可以使用RAM Policy实现不同场景下的用户权限策略。更多信息，请参见[RAM Policy常见示例](/intl.zh-CN/开发指南/数据安全/访问控制/RAM Policy/RAM Policy常见示例.md)。

