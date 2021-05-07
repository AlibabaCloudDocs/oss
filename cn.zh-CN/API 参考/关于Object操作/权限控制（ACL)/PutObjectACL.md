# PutObjectACL

调用PutObjectACL接口修改文件（Object）的访问权限（ACL）。此操作只有Bucket Owner有权限执行，且需对Object有读写权限。

## 版本控制

调用PutObjectACL接口时，默认只能设置Object当前版本的ACL。您可以通过指定versionId参数来设置指定Object版本的ACL。如果Object的对应版本为删除标记（Delete Marker），则OSS将返回404 Not Found。

## ACL说明

PutObjectACL接口通过Put请求中的`x-oss-object-acl`头来设置Object ACL。目前Object包括如下四种访问权限。

|名称|描述|
|:-|:-|
|private|Object是私有资源。只有该Object的Owner拥有该Object的读写权限，其他用户没有权限操作该Object。|
|public-read|Object是公共读资源。Object Owner拥有该Object的读写权限。非Object Owner只有该Object的读权限。|
|public-read-write|Object是公共读写资源。所有用户拥有对该Object的读写权限。|
|default|Object遵循其所在Bucket的读写权限，即Bucket是什么权限，Object就是什么权限。|

**说明：**

-   Object ACL优先级高于Bucket ACL。例如Bucket ACL是private的，而Object ACL是public-read-write的，则所有用户都拥有该Object的访问权限，即使该Bucket是私有Bucket。如果某个Object未设置过ACL，则访问权限遵循Bucket ACL。
-   Object的读操作包括GetObject、HeadObject、CopyObject和UploadPartCopy中的对原Object的读；Object的写操作包括PutObject、PostObject、AppendObject、DeleteObject、DeleteMultipleObjects、CompleteMultipartUpload以及CopyObject对新Object的写。
-   您还可以在进行Object的写操作时，在请求头中带上x-oss-object-acl来设置Object ACL，效果与PutObjectACL等同。例如PutObject时在请求头中带上x-oss-object-acl可以在上传一个Object的同时设置此Object的ACL。

## 请求语法

```
PUT /ObjectName?acl HTTP/1.1
x-oss-object-acl: Permission
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 请求头

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|x-oss-object-acl|字符串|否|public-read|指定OSS创建Object时的访问权限。 取值：

-   default（默认）：Object遵循所在存储空间的访问权限。
-   private：Object是私有资源。只有Object的拥有者和授权用户有该Object的读写权限，其他用户没有权限操作该Object。
-   public-read：Object是公共读资源。只有Object的拥有者和授权用户有该Object的读写权限，其他用户只有该Object的读权限。请谨慎使用该权限。
-   public-read-write：Object是公共读写资源。所有用户都有该Object的读写权限。请谨慎使用该权限。

关于访问权限的更多信息，请参见[读写权限ACL](/cn.zh-CN/开发指南/数据安全/访问控制/读写权限ACL.md)。 |

此接口还需要包含Host、Date等公共请求头。更多信息，请参见[公共请求头（Common Request Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 示例

-   未开启版本控制

    请求示例

    ```
    PUT /test-object?acl HTTP/1.1
    x-oss-object-acl: public-read
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Wed, 29 Apr 2015 05:21:12 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:KU5h8YMUC78M30dXqf3JxrTZ****
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 559CC9BDC755F95A64485981
    Date: Wed, 29 Apr 2015 05:21:12 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   已启用版本控制

    请求示例

    ```
    PUT /example?acl&versionId=CAEQMhiBgIC3rpSD0BYiIDBjYTk5MmIzN2JlNjQxZTFiNGIzM2E3OTliODA0**** HTTP/1.1
    x-oss-object-acl: public-read
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 06:30:11 GMT
    Authorization: OSS qctg2ns3l8u51iu:UTsv3F7L34v+ECq52vURdCSv****
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-version-id: CAEQMhiBgIC3rpSD0BYiIDBjYTk5MmIzN2JlNjQxZTFiNGIzM2E3OTliODA0****
    x-oss-request-id: 5CAC3BF3B7AEADE017000624
    Date: Tue, 09 Apr 2019 06:30:11 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```


## SDK

此接口所对应的各语言SDK如下：

-   [Java](/cn.zh-CN/SDK 示例/Java/管理文件/管理文件访问权限.md)
-   [Python](/cn.zh-CN/SDK 示例/Python/管理文件/管理文件访问权限.md)
-   [PHP](/cn.zh-CN/SDK 示例/PHP/管理文件/管理文件访问权限.md)
-   [Go](/cn.zh-CN/SDK 示例/Go/管理文件/管理文件访问权限.md)
-   [C](/cn.zh-CN/SDK 示例/C/管理文件/管理文件访问权限.md)
-   [.NET](/cn.zh-CN/SDK 示例/.NET/管理文件/管理文件访问权限.md)
-   [Node.js](/cn.zh-CN/SDK 示例/Node.js/管理文件/管理文件访问权限.md)
-   [Browser.js](/cn.zh-CN/SDK 示例/Browser.js/访问权限.md)

## 错误码

|错误码|HTTP状态码|描述|
|:--|:------|:-|
|AccessDenied|403|接口使用者不是Bucket Owner或者Bucket Owner对Object没有读写权限。|
|InvalidArgument|400|指定的x-oss-object-acl值无效。|

