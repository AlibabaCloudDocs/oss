# GetObjectMeta

GetObjectMeta接口用于获取一个文件（Object）的元数据信息，包括该Object的ETag、Size、LastModified信息，并且不返回该Object的内容。

**说明：** 如果Object类型为软链接，则会返回软链接自身信息。

## 版本控制

GetObjectMeta操作默认获取Object当前版本的元数据信息。如果Object的当前版本为删除标记，则返回404 Not Found。请求参数中指定versionId则返回指定版本Object的元数据信息。

## 请求语法

```
HEAD /ObjectName?objectMeta HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 响应头

|响应头|类型|描述|
|:--|--|:-|
|Content-Length|字符串|Object的文件大小。 |
|ETag|字符串|Object生成时会创建ETag \(entity tag\)，ETag用于标识一个Object的内容。

对于PutObject请求创建的Object，ETag值是其内容的MD5值；对于其他方式创建的Object，ETag值是其内容的UUID。ETag值可以用于检查Object内容是否发生变化。不建议用户使用ETag来作为Object内容的MD5校验数据完整性。

默认值：无 |

## 示例

-   未启用版本控制

    请求示例

    ```
    HEAD /oss.jpg?objectMeta HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Wed, 29 Apr 2015 05:21:12 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:CTkuxpLAi4XZ+WwIfNm0Fmgb****
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 559CC9BDC755F95A6448****
    Date: Wed, 29 Apr 2015 05:21:12 GMT
    ETag: "5B3C1A2E053D763E1B002CC607C5****"
    Last-Modified: Fri, 24 Feb 2012 06:07:48 GMT
    Content-Length: 344606
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   已启用版本控制

    请求示例

    ```
    GET /example?objectMeta&versionId=CAEQNRiBgIDMh4mD0BYiIDUzNDA4OGNmZjBjYTQ0YmI4Y2I4ZmVlYzJlNGVk**** HTTP/1.1
    Host: versioning-test.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 06:24:00 GMT
    Authorization: OSS 5n4nrhyqrcs6ngn:i/M/c36KzrOEA/bBSHLllIAt****
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-version-id: CAEQNRiBgIDMh4mD0BYiIDUzNDA4OGNmZjBjYTQ0YmI4Y2I4ZmVlYzJlNGVk****
    x-oss-request-id: 5CAC3A80B7AEADE0170005F6
    Date: Tue, 09 Apr 2019 06:24:00 GMT
    ETag: "1CF5A685959CA2ED8DE6E5F8ACC2****"
    Last-Modified: Tue, 09 Apr 2019 06:24:00 GMT
    Content-Length: 119914
    Connection: keep-alive
    Server: AliyunOSS
    ```


## SDK

GetObjectMeta接口所对应的各语言SDK如下：

-   [Java](/cn.zh-CN/SDK 示例/Java/管理文件/管理文件元信息.md)
-   [Python](/cn.zh-CN/SDK 示例/Python/管理文件/管理文件元信息.md)
-   [PHP](/cn.zh-CN/SDK 示例/PHP/管理文件/管理文件元信息.md)
-   [Go](/cn.zh-CN/SDK 示例/Go/管理文件/管理文件访问权限.md)
-   [C](/cn.zh-CN/SDK 示例/C/管理文件/管理文件访问权限.md)
-   [.NET](/cn.zh-CN/SDK 示例/.NET/管理文件/管理文件元信息.md)
-   [Android](/cn.zh-CN/SDK 示例/Android/管理文件/获取文件元信息.md)
-   [iOS](/cn.zh-CN/SDK 示例/iOS/管理文件/概述.md)

## 错误码

|错误码|HTTP状态码|描述|
|:--|:------|:-|
|Not Found|404|目标Object不存在。|

