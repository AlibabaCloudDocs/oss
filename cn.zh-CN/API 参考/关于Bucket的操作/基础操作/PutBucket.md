# PutBucket

PutBucket接口用于创建存储空间（Bucket）。

## 注意事项

-   要创建存储空间，您必须有PutBucket权限。
-   同一阿里云账号在同一地域（Region）内最多可创建100个Bucket。
-   每个地域都有对应的访问域名（Endpoint）。地域与访问域名对应关系的更多信息，请参见[访问域名和数据中心](/cn.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md)。

## 请求语法

```
PUT / HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
x-oss-acl: Permission
Authorization: SignatureValue
<?xml version="1.0" encoding="UTF-8"?>
<CreateBucketConfiguration>
    <StorageClass>Standard</StorageClass>
</CreateBucketConfiguration>
```

## 请求头

|名称|类型|是否必选|示例值|描述|
|:-|:-|:---|---|:-|
|x-oss-acl|字符串|否|private|指定Bucket的访问权限ACL。包括如下选择： -   public-read-write：公共读写
-   public-read：公共读
-   private：私有（默认值）

有关Bucket访问权限ACL的更多信息，请参见[设置存储空间访问权限ACL](/cn.zh-CN/开发指南/存储空间（Bucket）/设置存储空间读写权限（ACL）.md)。 |

此接口还需要包含公共请求头。关于公共请求头的更多信息，请参见[公共请求头（Common Request Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 请求元素

|名称|类型|是否必选|示例值|描述|
|--|--|:---|---|--|
|StorageClass|字符串|否|Standard|指定Bucket的存储类型。 有效值：

-   Standard（默认值）：标准存储
-   IA：低频访问
-   Archive：归档存储
-   ColdArchive：冷归档存储 |
|DataRedundancyType|字符串|否|LRS|指定Bucket的数据容灾类型。

有效值：

-   LRS（默认值）

本地冗余LRS，将您的数据冗余存储在同一个可用区的不同存储设备上，可支持两个存储设备并发损坏时，仍维持数据不丢失，可正常访问。

-   ZRS

同城冗余ZRS采用多可用区（AZ）机制，将您的数据冗余存储在同一地域（Region）的3个可用区。可支持单个可用区（机房）整体故障时（例如断电、火灾等），仍然能够保障数据的正常访问。 |

关于Bucket的存储类型和数据容灾类型的更多信息，请参见[存储类型介绍](/cn.zh-CN/开发指南/存储类型/存储类型介绍.md)。

## 响应头

此接口仅包含公共响应头。关于公共响应头的更多信息，请参见[公共响应头（Common Response Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 示例

请求示例

```
PUT / HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2017 03:15:40 GMT
x-oss-acl: private
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:77Dvh5wQgIjWjwO/KyRt8dOP****
<?xml version="1.0" encoding="UTF-8"?>
<CreateBucketConfiguration>
    <StorageClass>Standard</StorageClass>
</CreateBucketConfiguration>
```

返回示例

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906****
Date: Fri, 24 Feb 2017 03:15:40 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

## SDK

此接口所对应的各语言SDK如下：

-   [Java](/cn.zh-CN/SDK 示例/Java/存储空间/创建存储空间.md)
-   [Python](/cn.zh-CN/SDK 示例/Python/存储空间/创建存储空间.md)
-   [PHP](/cn.zh-CN/SDK 示例/PHP/存储空间/创建存储空间.md)
-   [Go](/cn.zh-CN/SDK 示例/Go/存储空间/创建存储空间.md)
-   [C](/cn.zh-CN/SDK 示例/C/存储空间/创建存储空间.md)
-   [.NET](/cn.zh-CN/SDK 示例/.NET/存储空间/创建存储空间.md)
-   [Android](/cn.zh-CN/SDK 示例/Android/存储空间/创建存储空间.md)
-   [iOS](/cn.zh-CN/SDK 示例/iOS/存储空间.md)
-   [Node.js](/cn.zh-CN/SDK 示例/Node.js/存储空间/创建存储空间.md)
-   [Ruby](/cn.zh-CN/SDK 示例/Ruby/存储空间/创建存储空间.md)

## 错误码

|错误码|HTTP状态码|描述|
|:--|:------|:-|
|InvalidBucketName|400|创建Bucket时，定义的Bucket名称不符合命名规范。|
|AccessDenied|403|返回该错误的可能原因如下：-   发起PutBucket请求时没有传入用户验证信息。
-   没有操作权限。 |
|TooManyBuckets|400|创建的Bucket数量超过上限。同一阿里云账号在同一地域（Region）内最多可创建100个Bucket。|
|BucketAlreadyExists|409|该Bucket已存在或被其他用户占用。请尝试使用其他符合命名规范的Bucket名称新建Bucket。 |

