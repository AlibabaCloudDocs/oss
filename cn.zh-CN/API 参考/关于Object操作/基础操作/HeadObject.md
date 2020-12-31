# HeadObject

HeadObject接口用于获取某个文件（Object）的元信息。使用此接口不会返回文件内容。

## 版本控制

HeadObject操作默认获取Object当前版本的元信息。如果Object的当前版本为删除标记，则返回404 Not Found。请求参数中指定versionId则返回指定版本Object的元信息。

## 请求语法

```
HEAD /ObjectName HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 请求头

|名称|类型|是否必选|描述|
|:-|:-|:---|:-|
|If-Modified-Since|字符串|否|如果传入参数中的时间早于实际修改时间，则返回200 OK和Object Meta；否则返回304 not modified。 默认值：无 |
|If-Unmodified-Since|字符串|否|如果传入参数中的时间等于或者晚于文件实际修改时间，则返回200 OK和Object Meta；否则返回412 precondition failed。 默认值：无 |
|If-Match|字符串|否|如果传入期望的ETag和Object的 ETag匹配，则返回200 OK和Object Meta；否则返回412 precondition failed。 默认值：无 |
|If-None-Match|字符串|否|如果传入期望的ETag值和Object的ETag不匹配，则返回200 OK和Object Meta；否则返回304 Not Modified。 默认值：无 |

## 响应头

如果请求的文件类型为软链接，则各个响应头的信息说明如下：

-   Content-Length、ETag以及Content-Md5均为目标文件的元信息。
-   Last-Modified取软链接的最后修改时间及目标文件的最后修改时间两者中的较大值。
-   其他响应头均表示软链接的元信息。

|名称|类型|描述|
|:-|:-|:-|
|x-oss-meta-\*|字符串|以x-oss-meta-为前缀的参数作为用户自定义meta header。当用户在PutObject时设置了以x-oss-meta-为前缀的自定义meta，则响应中会包含这些自定义meta。|
|非x-oss-meta-开头的自定义header|字符串|当用户在PutObject时，自定义一些非x-oss-meta为前缀的Header，如x-oss-persistent-headers:key1:base64\_encode\(value1\),key2:base64\_encode\(value2\).... ，响应中会增加相应的自定义Header。 详情请参见[OSS如何添加非x-oss-meta-开头的自定义](https://yq.aliyun.com/articles/499171)。 |
|x-oss-server-side-encryption|字符串|若该Object为进行服务器端熵编码加密存储的，则在响应头头中会返回此参数，其值表明该Object的服务器端加密算法。|
|x-oss-server-side-encryption-key-id|字符串|如果用户在创建Object时使用了服务端加密，且加密方法为KMS，则响应中会包含此Header，表示加密所使用的用户KMS key ID。|
|x-oss-storage-class|字符串|表示Object的存储类型，分别为：标准存储类型（Standard）、低频访问存储类型（IA）、归档存储类型（Archive）和冷归档存储类型（ColdArchive）。 -   标准存储类型提供高可靠、高可用、高性能的对象存储服务，能够支持频繁的数据访问。
-   低频访问存储类型适合需要长期存储但不经常被访问的数据（平均每月访问频率1到2次）。
-   归档存储类型适合需要长期存储（建议半年以上）的归档数据，在存储周期内极少被访问，数据进入到可读取状态需要1分钟的解冻时间。
-   冷归档存储类型适用于需要长期保存且几乎不访问的数据。 |
|x-oss-object-type|字符串|表示Object的类型。 -   通过PutObject上传的Object类型为Normal。
-   通过AppendObject上传的Object类型为Appendable。
-   通过MultipartUpload上传的Object类型为Multipart。 |
|x-oss-next-append-position|字符串|对于Appendable类型的Object会返回此Header，指明下一次请求应当提供的position。|
|x-oss-hash-crc64ecma|字符串|表示该Object的64位CRC值。该64位CRC根据ECMA-182标准计算得出。对OSS支持CRC64校验前创建的Object，调用HeadObject接口时可能不会返回此响应头。 |
|x-oss-expiration|字符串|如果为该Object设置了生命周期规则（Lifecycle），响应中将包含x-oss-expiration header。其中expiry-date的值表示该Object的过期日期，rule-id的值表示相匹配的规则ID。|
|x-oss-restore|字符串|如果Bucket类型为Archive，且用户已经提交Restore请求，则响应头中会以x-oss-restore返回该Object的Restore状态，分如下几种情况： -   如果没有提交Restore或者Restore已经超时，则不返回该字段。
-   如果已经提交Restore，且Restore没有完成，则返回的x-oss-restore值为ongoing-request=”true”。
-   如果已经提交Restore，且Restore已经完成，则返回的x-oss-restore值为ongoing-request=”false”, expiry-date=”Sun, 16 Apr 2017 08:12:33 GMT”，其中expiry-date是Restore完成后Object进入可读状态的过期时间。 |
|x-oss-process-status|字符串|当用户通过MNS消息服务创建OSS事件通知后，在进行请求OSS相关操作时如果有匹配的事件通知规则，则响应中会携带这个Header，值为经过Base64编码JSON格式的事件通知结果。|
|x-oss-request-charged|字符串|当Object所属的Bucket被设置为请求者付费模式，且请求者不是Bucket的拥有者时，响应中将携带此Header，值为requester。|
|Content-Md5|字符串|-   对于Normal类型的Object，根据RFC 1864标准对消息内容（不包括Header）计算Md5值获得128比特位数字，对该数字进行Base64编码作为一个消息的Content-Md5值。
-   Multipart和Appendable类型的文件不会返回这个Header。 |
|Last-Modified|字符串|Object最后一次修改的日期，格式为HTTP 1.1协议中规定的GMT时间。|
|Access-Control-Allow-Origin|字符串|当Object所在的Bucket配置了CORS规则，且请求的Origin满足指定的CORS规则时会在响应中包含这个Origin。|
|Access-Control-Allow-Methods|字符串|当Object所在的Bucket配置了CORS规则，且请求的Access-Control-Request-Method满足指定的CORS规则时会在响应中包含允许的Methods。|
|Access-Control-Max-Age|字符串|当Object所在的Bucket配置了CORS规则，且请求满足Bucket配置的CORS规则时会在响应中包含MaxAgeSeconds。|
|Access-Control-Allow-Headers|字符串|当Object所在的Bucket配置了CORS规则，且请求满足指定的CORS规则时会在响应中包含这些Headers。|
|Access-Control-Expose-Headers|字符串|表示允许访问客户端JavaScript程序的headers列表。当Object所在的Bucket配置了CORS规则，且请求满足指定的CORS规则时会在响应中包含ExposeHeader。|
|x‑oss‑tagging‑count|字符串|对象关联的标签个数。仅当用户有读取标签权限时返回。|

## 示例

-   未启用版本控制的请求示例

    ```
    HEAD /oss.jpg HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 7 Aug 2020 07:32:52 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:JbzF2LxZUtanlJ5dLA092wpD****
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 559CC9BDC755F95A6448****
    x-oss-object-type: Normal
    x-oss-storage-class: Archive
    Date: Fri, 7 Aug 2020 07:32:52 GMT
    Last-Modified: Fri, 24 Feb 2012 06:07:48 GMT
    ETag: "fba9dede5f27731c9771645a3986****"
    Content-Length: 344606
    Content-Type: image/jpg
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   请求对象指定版本（启用版本控制）

    ```
    HEAD /example?versionId=CAEQNRiBgICb8o6D0BYiIDNlNzk5NGE2M2Y3ZjRhZTViYTAxZGE0ZTEyMWYy****
    Host: versioning-test.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 7 Aug 2020 06:27:12 GMT
    Authorization: OSS ryghu9rp3mqp1ya:RvyjGvKxaUhdF0ibyEwX5mOM****
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-versionId: CAEQNRiBgICb8o6D0BYiIDNlNzk5NGE2M2Y3ZjRhZTViYTAxZGE0ZTEyMWYy****
    x-oss-request-id: 5CAC3B40B7AEADE01700****
    x-oss-object-type: Normal
    x-oss-storage-class: Archive
    Date: Fri, 7 Aug 2020 06:27:12 GMT
    Last-Modified: Fri, 7 Aug 2020 06:27:12 GMT
    ETag: "A082B659EF78733A5A042FA253B1****"
    Content-Length: 481827
    Content-Type: text/html
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   请求对象最新版本（启用版本控制）

    ```
    HEAD /example HTTP/1.1    
    Host: versioning-test.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 7 Aug 2020 06:27:12 GMT
    Authorization: OSS ryghu9rp3mqp1ya:RvyjGvKxaUhdF0ibyEwX5mOM****
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-versionId: CAEQMxiBgMCZov2D0BYiIDY4MDllOTc2YmY5MjQxMzdiOGI3OTlhNTU0ODIx****
    x-oss-request-id: 5CAC3B40B7AEADE01700****
    x-oss-object-type: Normal
    x-oss-storage-class: Archive
    Date: Fri, 7 Aug 2020 06:27:12 GMT
    Last-Modified: Fri, 7 Aug 2020 06:27:12 GMT
    ETag: "3663F7B0B9D3153F884C821E7CF4****"
    Content-Length: 485859
    Content-Type: text/html
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   提交Restore请求但Restore没有完成时的请求示例

    ```
    HEAD /oss.jpg HTTP/1.1
    Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 7 Aug 2020 07:32:52 GMT
    Authorization: OSS e1Unnbm1rgdnpI:KKxkdNrUBu2t1kqlDh0MLbDb****
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 58F71A164529F18D7F00****
    x-oss-object-type: Normal
    x-oss-storage-class: Archive
    x-oss-restore: ongoing-request="true"
    Date: Fri, 7 Aug 2020 07:32:52 GMT
    Last-Modified: Fri, 7 Aug 2020 06:07:48 GMT
    ETag: "fba9dede5f27731c9771645a3986****"
    Content-Length: 344606
    Content-Type: image/jpg
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   提交Restore请求且Restore已经完成时的请求示例

    ```
    HEAD /oss.jpg HTTP/1.1
    Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 7 Aug 2020 09:35:51 GMT
    Authorization: OSS e1Unnbm1rgdnpI:21qtGJ+ykDVmdu6O6FMJnn+W****
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 58F725344529F18D7F00****
    x-oss-object-type: Normal
    x-oss-storage-class: Archive
    x-oss-restore: ongoing-request="false", expiry-date="Sun, 16 Apr 2017 08:12:33 GMT"
    Date: Fri, 7 Aug 2020 09:35:51 GMT
    Last-Modified: Fri, 7 Aug 2020 06:07:48 GMT
    ETag: "fba9dede5f27731c9771645a3986****"
    Content-Length: 344606
    ```

-   使用服务端加密SSE-OSS的请求示例

    ```
    HEAD /oss.jpg HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 7 Aug 2020 07:32:52 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:JbzF2LxZUtanlJ5dLA092wpD****
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 559CC9BDC755F95A6448****
    x-oss-object-type: Normal
    x-oss-storage-class: Archive
    x-oss-server-side-encryption: AES256
    Date: Fri, 7 Aug 2020 07:32:52 GMT
    Last-Modified: Fri, 7 Aug 2020 06:07:48 GMT
    ETag: "fba9dede5f27731c9771645a3986****"
    Content-Length: 344606
    Content-Type: image/jpg
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   使用服务器端加密SSE-KMS的请求示例

    ```
    HEAD /oss.jpg HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 7 Aug 2020 07:32:52 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:JbzF2LxZUtanlJ5dLA092wpD****
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 559CC9BDC755F95A64485981
    x-oss-object-type: Normal
    x-oss-storage-class: Archive
    x-oss-server-side-encryption: KMS
    x-oss-server-side-encryption-key-id: 9468da86-3509-4f8d-a61e-6eab1eac****
    Date: Fri, 7 Aug 2020 07:32:52 GMT
    Last-Modified: Fri, 7 Aug 2020 06:07:48 GMT
    ETag: "fba9dede5f27731c9771645a3986****"
    Content-Length: 344606
    Content-Type: image/jpg
    Connection: keep-alive
    Server: AliyunOSS
    ```


## 错误码

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|Not Found|404|请求的文件不存在。|
|SymlinkTargetNotExist|404|请求的文件类型为软链接。|
|InvalidTargetType|400|请求的文件类型为软链接，且对应的目标文件类型也为软链接。|
|Not Modified|304|-   指定了If-Modified-Since请求头，但源Object在指定的时间后没被修改过。
-   指定了If-None-Match请求头，且源Object的ETag值和您提供的ETag相等。 |
|Precondition Failed|412|-   指定了If-Unmodified-Since，但指定的时间早于Object实际修改时间 。
-   指定了If-Match，但源Object的ETag值和您提供的ETag不相等。 |

