# GetObject

GetObject接口用于获取某个文件（Object）。此操作需要对此Object具有读权限。

## 注意事项

-   GetObject接口默认可通过HTTP和HTTPS两种方式访问。如果要设置仅允许通过HTTPS方式访问，请使用Bucket Policy的授权访问方式。具体操作，请参见[通过Bucket Policy授权用户访问指定资源](/cn.zh-CN/控制台用户指南/文件管理/添加Bucket授权策略（Bucket Policy）.md)。
-   如果Object类型为归档类型，需要先完成解冻文件（RestoreObject）请求，且该请求不能超时。

## 版本控制

默认情况下，调用GetObject接口仅返回Object的当前版本。

如果在查询参数中指定Object的versionId，则返回指定的Object版本。当versionId指定为null时，则返回versionId为null的Object版本。

## 请求语法

```
GET /ObjectName HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
Range: bytes=ByteRange(可选)
```

下载OSS中的大文件（超过100 MB）时，如果传输过程中受到网络环境影响，则会传输失败。您可以通过HTTP Range请求获取大文件的部分内容。

OSS不支持多Range参数，即不支持指定多个范围。ByteRange指请求资源的范围，单位为Byte（字节），ByteRange有效区间在0至`object size - 1`的范围内。具体示例如下：

-   `Range: bytes=0-499`表示第0~499字节范围的内容。
-   `Range: bytes=500-999`表示第500~999字节范围的内容。
-   `Range: bytes=-500`表示最后500字节的内容。
-   `Range: bytes=500-`表示从第500字节开始到文件结束部分的内容。
-   `Range: bytes=0-`表示从第一个字节到最后一个字节，即完整的文件内容。

**说明：** 有关Rangeget兼容性的更多信息，请参见[兼容行为说明](https://help.aliyun.com/knowledge_detail/39571.html)。

## 请求头

OSS支持在GET请求中通过请求头来自定义响应头，但只有请求成功（即返回码为200 OK）才会将响应头的值设置成GET请求头中指定的值。

**说明：** OSS不支持在匿名访问的GET请求中自定义响应头。

|名称|类型|是否必选|描述|
|:-|:-|:---|:-|
|response-content-type|字符串|否|指定OSS返回请求的content-type头。 默认值：无 |
|response-content-language|字符串|否|指定OSS返回请求的content-language头。 默认值：无 |
|response-expires|字符串|否|指定OSS返回请求的expires头。 默认值：无 |
|response-cache-control|字符串|否|指定OSS返回请求的cache-control头。 默认值：无 |
|response-content-disposition|字符串|否|指定OSS返回请求的content-disposition头。 默认值：无 |
|response-content-encoding|字符串|否|指定OSS返回请求的content-encoding头。 默认值：无 |
|Range|字符串|否|指定文件传输的范围。 -   如果指定的范围符合规范，返回消息中会包含整个Object的大小和此次返回Object的范围。例如：Content-Range: bytes 0~9/44，表示整个Object大小为44，此次返回的范围为0~9。
-   如果指定的范围不符合规范，则传送整个Object，并且结果中不包含Content-Range。

默认值：无 |
|If-Modified-Since|字符串|否|如果指定的时间早于实际修改时间或指定的时间不符合规范，则直接返回Object，并返回200 OK；如果指定的时间等于或者晚于实际修改时间，则返回304 Not Modified。 时间格式：GMT，例如`Fri, 13 Nov 2015 14:47:53 GMT`

默认值：无 |
|If-Unmodified-Since|字符串|否|如果指定的时间等于或者晚于Object实际修改时间，则正常传输Object，并返回200 OK；如果指定的时间早于实际修改时间，则返回412 Precondition Failed。 时间格式：GMT，例如`Fri, 13 Nov 2015 14:47:53 GMT`

If-Modified-Since和If-Unmodified-Since可以同时使用。

默认值：无 |
|If-Match|字符串|否|如果传入的ETag和Object的ETag匹配，则正常传输Object，并返回200 OK；如果传入的ETag和Object的ETag不匹配，则返回412 Precondition Failed。 Object的ETag值用于验证数据是否发生了更改，您可以基于ETag值验证数据完整性。

默认值：无 |
|If-None-Match|字符串|否|如果传入的ETag值和Object的ETag不匹配，则正常传输Object，并返回200 OK；如果传入的ETag和Object的ETag匹配，则返回304 Not Modified。 If-Match和If-None-Match可以同时使用。

默认值：无 |
|Accept-Encoding|字符串|否|指定客户端的编码类型。 如果要对返回内容进行Gzip压缩传输，您需要在请求头中以显示方式加入Accept-Encoding:gzip。OSS会根据Object的Content-Type和Object大小（不小于1 KB）判断是否返回经过Gzip压缩的数据。

**说明：**

-   如果采用了Gzip压缩，则不会附带ETag信息。
-   目前OSS支持Gzip压缩的Content-Type为text/cache-manifest、 text/xml、text/plain、text/css、application/javascript、application/x-javascript、application/rss+xml、application/json和text/json。

默认值：无 |

## 响应头

如果Object类型为软链接，则返回目标Object的内容。响应头中`Content-Length`、`ETag`、`Content-Md5`为目标Object的元数据；`Last-Modified`取目标Object和软链接对应的最大值（即在两者中取更新较晚的时间）；其他均为软链接的元数据。

|名称|类型|描述|
|--|--|--|
|x-oss-server-side-encryption|字符串|若Object在服务器端采用熵编码加密存储，使用GET请求时，系统会自动解密返回给用户，并且在响应头中返回x-oss-server-side-encryption，表明该Object的服务器端加密算法。|
|x‑oss‑tagging‑count|字符串|对象关联的标签的个数。仅当有读取标签权限时返回。|

## 示例

-   简单的GET请求

    请求示例

    ```
    GET /oss.jpg HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 24 Feb 2012 06:38:30 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:UNQDb7GapEgJkcde6OhZ9J*****
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 3a8f-2e2d-7965-3ff9-51c875b*****
    x-oss-object-type: Normal
    Date: Fri, 24 Feb 2012 06:38:30 GMT
    Last-Modified: Fri, 24 Feb 2012 06:07:48 GMT
    ETag: "5B3C1A2E0563E1B002CC607C*****"
    Content-Type: image/jpg
    Content-Length: 344606
    Server: AliyunOSS
    [344606 bytes of object data]
    ```

-   带有Range参数

    请求示例

    ```
    GET /oss.jpg HTTP/1.1
    Host:oss-example. oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 28 Feb 2012 05:38:42 GMT
    Range: bytes=100-900
    Authorization: OSS qn6qrrqxo2oawuk5jbyc:qZzjF3DUtd+yK16BdhGtFcC*****
    ```

    返回示例

    ```
    HTTP/1.1 206 Partial Content
    x-oss-request-id: 28f6-15ea-8224-234e-c0ce407*****
    x-oss-object-type: Normal
    Date: Fri, 28 Feb 2012 05:38:42 GMT
    Last-Modified: Fri, 24 Feb 2012 06:07:48 GMT
    ETag: "5B3C1A2E05E1B002CC607C*****"
    Accept-Ranges: bytes
    Content-Range: bytes 100-900/344606
    Content-Type: image/jpg
    Content-Length: 801
    Server: AliyunOSS
    [801 bytes of object data]
    ```

-   带自定义返回消息头

    请求示例

    ```
    GET /oss.jpg?response-expires=Thu%2C%2001%20Feb%202012%2017%3A00%3A00%20GMT& response-content-type=text&response-cache-control=No-cache&response-content-disposition=attachment%253B%2520filename%253Dtesting.txt&response-content-encoding=utf-8&response-content-language=%E4%B8%AD%E6%96%87 HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com:
    Date: Fri, 24 Feb 2012 06:09:48 GMT
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 559CC9BDC75A644*****
    x-oss-object-type: Normal
    Date: Fri, 24 Feb 2012 06:09:48 GMT 
    Last-Modified: Fri, 24 Feb 2012 06:07:48 GMT
    ETag: "5B3C1A2E053D1B002CC607*****"
    Content-Length: 344606
    Connection: keep-alive
    Content-disposition: attachment; filename:testing.txt
    Content-language: 中文
    Content-encoding: utf-8
    Content-type: text
    Cache-control: no-cache
    Expires: Fri, 24 Feb 2012 17:00:00 GMT
    Server: AliyunOSS
    [344606 bytes of object data]
    ```

-   Object类型为软链接

    请求示例

    ```
    GET /link-to-oss.jpg HTTP/1.1
    Accept-Encoding: identity
    Date: Tue, 08 Nov 2016 03:17:58 GMT
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Authorization: OSS qn6qrrqxok53otfjbyc:qZzjF3DUtd+yK16BdhGtFc*****
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    Server: AliyunOSS
    Date: Tue, 08 Nov 2016 03:17:58 GMT
    Content-Type: application/octet-stream
    Content-Length: 20
    Connection: keep-alive
    x-oss-request-id: 582143E6A212AD*****
    Accept-Ranges: bytes
    ETag: "8086265EFC021F9A2F09BF4****"
    Last-Modified: Tue, 08 Nov 2016 03:17:58 GMT
    x-oss-object-type: Symlink
    Content-MD5: gIYmXvwCEe0fmi8Jv0Y****
    ```

-   Restore操作已完成

    请求示例

    ```
    GET /oss.jpg HTTP/1.1
    Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
    Date: Sat, 15 Apr 2017 09:38:30 GMT
    Authorization: OSS qn6qrrqxo2o***k53otfjbyc:zUglwRPGkbByZxm1+y4eyu+*****
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 58F723829F29F18D7F00*****
    x-oss-object-type: Normal
    x-oss-restore: ongoing-request="false", expiry-date="Sun, 16 Apr 2017 08:12:33 GMT"
    Date: Sat, 15 Apr 2017 09:38:30 GMT
    Last-Modified: Sat, 15 Apr 2017 06:07:48 GMT
    ETag: "5B3C1A2E0763E1B002CC607C*****"
    Content-Type: image/jpg
    Content-Length: 344606
    Server: AliyunOSS
    [354606 bytes of object data]
    ```

-   指定versionId访问指定版本Object

    请求示例

    ```
    GET /example?versionId=CAEQNhiBgMDJgZCA0BYiIDc4MGZjZGI2OTBjOTRmNTE5NmU5NmFhZjhjYmY0**** HTTP/1.1
    Host: versioning-get.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 02:58:06 GMT
    Authorization: OSS lkojgxic6e:8wcOrEDt4iSxpBPfQW9OJNw*****
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 5CAC0A3EDE0170*****
    x-oss-version-id: CAEQNhiBgM0BYiIDc4MGZjZGI2OTBjOTRmNTE5NmU5NmFhZjhjYmY*****
    x-oss-object-type: Normal
    Date: Tue, 09 Apr 2019 02:58:06 GMT
    Last-Modified: Fri, 22 Mar 2018 08:07:50 GMT
    ETag: "5B3C1A2E053D7002CC607C5A*****"
    Content-Type: text/html
    Content-Length: 362149
    Server: AliyunOSS
    [362149 bytes of object data]
    ```

-   未指定versionId且Object的当前版本为删除标记

    请求示例

    ```
    GET /example HTTP/1.1
    Host: versioning-get.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 03:22:33 GMT
    Authorization: OSS duagpvtn35:taVlDvAJMhEumrR+oLMWtQp*****
    ```

    返回示例

    ```
    HTTP/1.1 404 Not Found
    x-oss-request-id: 5CAC0FEADE0170*****
    x-oss-delete-marker: true
    x-oss-version-id: CAEQNxiBgyA0BYiIDc4ZDdmNTA2MGViZTRiNjE5NzZlZWM4OWM5OT*****
    Date: Tue, 09 Apr 2019 03:22:33 GMT
    Content-Type: application/xml
    Connection: keep-alive
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <Error>
      <Code>NoSuchKey</Code>
      <Message>The specified key does not exist.</Message>
      <RequestId>5CAC0FEADE0170*****</RequestId>
      <HostId>versioning-get.oss-cn-hangzhou.aliyun*****</HostId>
      <Key>example</Key>
    </Error>
    ```

-   指定versionId访问删除标记

    请求示例

    ```
    GET /example?versionId=CAEQMxiBgMCfqaWA0BYiIDliMWI4MGQ0MTVmMjQ3MmE5MDNlMmY4YmFkYTk3**** HTTP/1.1
    Host: versioning-get.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 03:09:44 GMT
    Authorization: OSS tvqm50uz4y:51UaP+wQt5k1RQang/U6Eeq*****
    ```

    返回示例

    ```
    HTTP/1.1 405 Method Not Allowed
    x-oss-request-id: 5CAC0CF8DE01700*****
    x-oss-delete-marker: true
    x-oss-version-id: CAEQMxiBgMCfqaWADliMWI4MGQ0MTVmMjQ3MmE5MDNlMmY4YmFkYTk*****
    Allow: DELETE
    Date: Tue, 09 Apr 2019 03:09:44 GMT
    Content-Type: application/xml
    Content-Length: 318
    Connection: keep-alive
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <Error>
      <Code>MethodNotAllowed</Code>
      <Message>The specified method is not allowed against this resource.</Message>
      <RequestId>5CAC0CF8DE0170*****</RequestId>
      <HostId>versioning-get.oss-cn-hangzhou.aliyunc*****</HostId>
      <Method>GET</Method>
      <ResourceType>DeleteMarker</ResourceType>
    </Error>
    ```


## SDK

GetObject接口所对应的各语言SDK如下：

-   [Java](/cn.zh-CN/SDK 示例/Java/上传文件/概述.md)
-   [Python](/cn.zh-CN/SDK 示例/Python/上传文件/概述.md)
-   [PHP](/cn.zh-CN/SDK 示例/PHP/上传文件/概述 .md)
-   [Go](/cn.zh-CN/SDK 示例/Go/上传文件/概述.md)
-   [C](/cn.zh-CN/SDK 示例/C/上传文件/概述.md)
-   [.NET](/cn.zh-CN/SDK 示例/.NET/上传文件/概述.md)
-   [Android](/cn.zh-CN/SDK 示例/Android/上传文件/概述.md)
-   [Node.js](/cn.zh-CN/SDK 示例/Node.js/上传文件/概述.md)
-   [Browser.js](/cn.zh-CN/SDK 示例/Browser.js/上传文件.md)
-   [Ruby](/cn.zh-CN/SDK 示例/Ruby/上传文件.md)

## 错误码

|错误码|HTTP状态码|说明|
|---|-------|--|
|NoSuchKey|404|目标Object不存在。|
|SymlinkTargetNotExist|404|Object类型为软链接，且目标Object不存在。|
|InvalidTargetType|400|Object类型为软链接，且目标Object类型仍为软链接。|
|InvalidObjectState|403|下载归档类型的Object时： -   没有提交RestoreObject请求或者上一次提交RestoreObject已经超时。
-   已经提交RestoreObject请求，但数据的RestoreObject操作还没有完成。 |
|Not Modified|304|返回该错误的可能原因如下：-   指定了If-Modified-Since请求头，但请求的Object在指定的时间后没被修改过。
-   指定了If-None-Match请求头，且请求的Object的ETag值和您提供的ETag相等。 |
|Precondition Failed|412|返回该错误的可能原因如下：-   指定了If-Unmodified-Since，但指定的时间早于Object实际修改时间 。
-   指定了If-Match，但请求的Object的ETag值和您提供的ETag不相等。 |
|Not Found|404|在请求中未指定Object的versionId，且Object的当前版本是删除标记（Delete Marker）时，返回该错误。|
|Method Not Allowed|405|在请求中指定了Object的versionId，且versionid对应的是删除标记时，返回该错误。|

