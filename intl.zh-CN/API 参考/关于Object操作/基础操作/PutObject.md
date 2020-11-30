# PutObject

PutObject接口用于上传文件（Object）。

## 注意事项

调用PutObject接口时，有如下注意事项：

-   添加的Object大小不能超过5 GB。
-   默认情况下，如果已经存在同名Object且对该Object有访问权限，则新添加的Object将覆盖原有的Object，并成功返回200 OK。
-   OSS没有文件夹的概念，所有元素都是以文件来存储，但您可以通过创建一个以正斜线（/）结尾，大小为0的Object来创建模拟文件夹。
-   为确保数据完整性，OSS提供多种方式对数据的MD5值进行校验。 如需通过Content-MD5进行MD5验证，可将其加入请求头中。在请求中指定Content-MD5头后，OSS会计算所发送数据的MD5值，并与请求中Conent-MD5的值进行比较。如二者不一致，则操作失败并返回400（Bad Request）错误。

## 版本控制

在已开启版本控制的Bucket中，OSS会为新添加的Object自动生成唯一的版本ID，并在响应Header中通过x-oss-version-id形式返回。在暂停了版本控制的Bucket中，新添加的Object的版本ID为null。OSS保证同一个Object仅有一个null的版本ID。

## 请求语法

```
PUT /ObjectName HTTP/1.1
Content-Length：ContentLength
Content-Type: ContentType
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 请求头

OSS支持HTTP协议规定的5个请求头：Cache-Control、Expires、Content-Encoding、Content-Disposition、Content-Type。如果上传Object时设置了这些请求头，则该Object被下载时，相应的请求头值会被自动设置成上传时的值。

|名称|类型|是否必选|描述|
|:-|:-|:---|:-|
|Authorization|字符串|否|表示请求本身已被授权。更多信息，请参见[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 通常情况下Authorization是必选请求头，但如果采用了URL包含签名，则不用携带该请求头。更多信息，请参见[在URL中包含签名](/intl.zh-CN/API 参考/访问控制/在URL中包含签名.md)。

默认值：无 |
|Cache-Control|字符串|否|指定该Object被下载时网页的缓存行为。更多信息，请参见[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 默认值：无 |
|Content-Disposition|字符串|否|指定该Object被下载时的名称。更多信息，请参见[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 默认值：无 |
|Content-Encoding|字符串|否|指定该Object被下载时的内容编码格式。更多信息，请参见[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 默认值：无 |
|Content-MD5|字符串|否|用于检查消息内容是否与发送时一致。Content-MD5是由MD5算法生成的值。上传了Content-MD5请求头后，OSS会计算消息体的Content-MD5并检查一致性。更多信息，请参见[Content-MD5的计算方法](/intl.zh-CN/API 参考/访问控制/在Header中包含签名.md)。 默认值：无 |
|Content-Length|字符串|否|用于描述HTTP消息体的传输大小。 如果请求头中的Content-Length值小于实际请求体中传输的数据大小，OSS仍将成功创建Object，但Object的大小只能等于Content-Length中定义的大小，其他数据将被丢弃。 |
|ETag|字符串|否|Object生成时会创建相应的ETag ，ETag用于标识一个Object的内容。 -   对于PutObject请求创建的Object，ETag值是其内容的MD5值。
-   对于其他方式创建的Object，ETag值是其内容的UUID。

**说明：** ETag值可以用于检查Object内容是否发生变化。不建议使用ETag作为Object内容的MD5来校验数据完整性。

默认值：无 |
|Expires|字符串|否|过期时间。更多信息，请参见[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 默认值：无 |
|x-oss-forbid-overwrite|字符串|否|指定PutObject操作时是否覆盖同名Object。 当目标Bucket处于已开启或已暂停的版本控制状态时，x-oss-forbid-overwrite请求Header设置无效，即允许覆盖同名Object。-   不指定x-oss-forbid-overwrite时，默认覆盖同名Object。
-   指定x-oss-forbid-overwrite为true时，表示禁止覆盖同名Object；指定x-oss-forbid-overwrite为false时，表示允许覆盖同名Object。

设置x-oss-forbid-overwrite请求Header会导致QPS处理性能下降，如果您有大量的操作需要使用x-oss-forbid-overwrite请求Header（QPS \> 1000），请联系技术支持，避免影响您的业务。 |
|x-oss-server-side-encryption|字符串|否|创建Object时，指定服务器端加密方式。 取值：AES256、KMS

指定此参数后，在响应头中会返回此参数，OSS会对上传的Object进行加密编码存储。当下载该Object时，响应头中会包含x-oss-server-side-encryption，且该值会被设置成此Object的加密算法。 |
|x-oss-server-side-encryption-key-id|字符串|否|KMS托管的用户主密钥。 此参数仅在x-oss-server-side-encryption为KMS时有效。 |
|x-oss-object-acl|字符串|否|指定OSS创建Object时的访问权限。 合法值：public-read、private、public-read-write |
|x-oss-storage-class|字符串|否|指定Object的存储类型。 对于任意存储类型的Bucket，若上传Object时指定此参数，则此次上传的Object将存储为指定的类型。例如，在IA类型的Bucket中上传Object时，若指定x-oss-storage-class为Standard，则该Object直接存储为Standard。

取值：Standard、IA、Archive和ColdArchive

支持的接口：PutObject、InitMultipartUpload、AppendObject、 PutObjectSymlink、CopyObject。 |
|x-oss-meta-\*|字符串|否|使用PutObject接口时，如果配置以x-oss-meta-\*为前缀的参数，则该参数视为元数据，例如`x-oss-meta-location`。一个Object可以有多个类似的参数，但所有的元数据总大小不能超过8 KB。 元数据支持短划线（-）、数字、英文字母（a~z）。英文字符的大写字母会被转成小写字母，不支持下划线（\_）在内的其他字符。 |
|x-oss-tagging|字符串|否|指定Object的标签，可同时设置多个标签，例如：TagA=A&TagB=B。 **说明：** Key和Value需要先进行URL编码，如果某项没有”=“，则看作Value为空字符串。 |

## 响应头

此接口仅包含公共响应头。更多信息，请参见[公共响应头（Common Response Headers）](/intl.zh-CN/API 参考/公共HTTP头定义.md)。

## 示例

-   简单上传的请求示例

    ```
    PUT /test.txt HTTP/1.1
    Host: test.oss-cn-zhangjiakou.aliyuncs.com
    User-Agent: aliyun-sdk-python/2.6.0(Windows/7/AMD64;3.7.0)
    Accept: */*
    Connection: keep-alive
    Content-Type: text/plain
    date: Tue, 04 Dec 2018 15:56:37 GMT
    authorization: OSS qn6qrrqxo2oawuk53otfjbyc:kZoYNv66bsmc10+dcGKw5x2P****
    Transfer-Encoding: chunked
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    Server: AliyunOSS
    Date: Tue, 04 Dec 2018 15:56:38 GMT
    Content-Length: 0
    Connection: keep-alive
    x-oss-request-id: 5C06A3B67B8B5A3DA422299D
    ETag: "D41D8CD98F00B204E9800998ECF8****"
    x-oss-hash-crc64ecma: 0
    Content-MD5: 1B2M2Y8AsgTpgAmY7PhCfg==
    x-oss-server-time: 7
    ```

-   带有归档存储类型的请求示例

    ```
    PUT /oss.jpg HTTP/1.1 
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com Cache-control: no-cache 
    Expires: Fri, 28 Feb 2012 05:38:42 GMT 
    Content-Encoding: utf-8
    Content-Disposition: attachment;filename=oss_download.jpg 
    Date: Fri, 24 Feb 2012 06:03:28 GMT 
    Content-Type: image/jpg 
    Content-Length: 344606 
    x-oss-storage-class: Archive
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:kZoYNv66bsmc10+dcGKw5x2P**** 
    [344606 bytes of object data]
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    Server: AliyunOSS
    Date: Sat, 21 Nov 2015 18:52:34 GMT
    Content-Type: image/jpg
    Content-Length: 0
    Connection: keep-alive
    x-oss-request-id: 5650BD72207FB30443962F9A
    x-oss-bucket-version: 1418321259
    ETag: "A797938C31D59EDD08D86188F6D5B872"
    ```

-   版本控制的请求示例

    ```
    PUT /test HTTP/1.1
    Content-Length：362149
    Content-Type: text/html
    Host: versioning-put.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 02:53:24 GMT
    Authorization: OSS lkojgn8y1exic6e:6yYhX+BuuEqzI1tAMW0wgIyl****
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    Server: AliyunOSS
    Date: Tue, 09 Apr 2019 02:53:24 GMT
    Content-Length: 0
    Connection: keep-alive
    x-oss-request-id: 5CAC0A3DB7AEADE01700****
    x-oss-version-id: CAEQNhiBgMDJgZCA0BYiIDc4MGZjZGI2OTBjOTRmNTE5NmU5NmFhZjhjYmY0****
    ETag: "4F345B1F066DB1444775AA97D5D2****"
    ```


## SDK

PutObject接口对应的各语言SDK如下：

-   [Java](/intl.zh-CN/SDK 示例/Java/上传文件/简单上传.md)
-   [Python](/intl.zh-CN/SDK 示例/Python/上传文件/简单上传.md)
-   [Go](/intl.zh-CN/SDK 示例/Go/上传文件/简单上传.md)
-   [C++](/intl.zh-CN/SDK 示例/C++/上传文件/简单上传.md)
-   [PHP](/intl.zh-CN/SDK 示例/PHP/上传文件/简单上传.md)
-   [C](/intl.zh-CN/SDK 示例/C/上传文件/简单上传.md)
-   [.NET](/intl.zh-CN/SDK 示例/.NET/上传文件/简单上传.md)
-   [Android](/intl.zh-CN/SDK 示例/Android/上传文件/简单上传.md)
-   [iOS](/intl.zh-CN/SDK 示例/iOS/上传文件/概述.md)
-   [Node.js](/intl.zh-CN/SDK 示例/Node.js/上传文件/概述.md)
-   [Browser.js](/intl.zh-CN/SDK 示例/Browser.js/上传文件.md)
-   [Ruby](/intl.zh-CN/SDK 示例/Ruby/上传文件.md)

## 错误码

|错误码|HTTP状态码|描述|
|:--|:------|:-|
|MissingContentLength|411|请求头没有采用[chunked encoding](https://tools.ietf.org/html/rfc2616#section-3.6.1)编码方式，或没有设置Content-Length参数。|
|InvalidEncryptionAlgorithmError|400|指定x-oss-server-side-encryption的值无效。 有效值：AES256、KMS。 |
|AccessDenied|403|对试图添加Object的Bucket没有访问权限。|
|NoSuchBucket|404|试图添加Object的Bucket不存在。|
|InvalidObjectName|400|传入的Object key长度大于1023字节。|
|InvalidArgument|400|-   添加的Object大小超过5 GB。
-   x-oss-storage-class等参数的值无效。 |
|RequestTimeout|400|指定了Content-Length，但没有发送消息体，或者发送的消息体小于指定的大小。此种情况下服务器会一直等待，直至请求超时。|
|KmsServiceNotEnabled|403|将x-oss-server-side-encryption指定为KMS，但没有预先购买KMS套件。|
|FileAlreadyExists|409|当请求的Header中携带`x-oss-forbid-overwrite=true`时，表示禁止覆盖同名文件。如果文件已存在，则返回该错误。|
|FileImmutable|409|Bucket内的数据处于被保护状态时，若您尝试删除或修改这些数据，将返回此错误码。|

