# CopyObject

CopyObject接口用于拷贝同一地域下相同或不同存储空间（Bucket）之间的文件（Object）。

## 版本控制

`x-oss-copy-source`默认拷贝Object的当前版本。如果当前版本为删除标记，则返回404表示该Object不存在。您可以在`x-oss-copy-source`中加入versionId来拷贝指定的Object版本，删除标记不会被拷贝。

您可以将Object的早期版本拷贝到同一个Bucket中，OSS将为该Object生成新的版本，并将其置为该Object的当前版本，达到恢复Object早期版本的目的。

如果目标Bucket已开启版本控制，OSS将会为新拷贝的Object自动生成唯一的版本ID，此版本ID将会在响应Header中的`x-oss-version-id`返回。如果目标Bucket未曾开启或者暂停了版本控制，OSS将会为新拷贝的Object自动生成version ID为null的版本，且会覆盖原先versionId为null的版本。

## 使用限制

-   CopyObject接口仅支持拷贝小于1 GB的Object。如要拷贝大于1 GB的Object，建议使用[UploadPartCopy](/cn.zh-CN/API 参考/关于Object操作/分片上传（MulitipartUpload）/UploadPartCopy.md)。

    使用CopyObject或UploadPartCopy接口均要求对源Object有读权限。

-   在非版本控制的Bucket中，当拷贝的源Object与目标Object为同一个Object，调用CopyObject接口时，不拷贝源Object的内容，仅修改源Object的元数据。
-   不支持拷贝通过追加上传方式产生的Object。
-   如果源Object为软链接，则只拷贝软链接，无法拷贝软链接指向的文件内容。

## 计量计费

-   调用一次CopyObject接口会对源Object和目标Object所在的Bucket各增加一次Get请求次数。
-   调用CopyObject接口会对目标Object所在的Bucket增加相应的存储量。
-   调用CopyObject接口更改Object存储类型会涉及数据覆盖。例如低频访问IA创建后10天内被覆盖为标准存储Standard，则会产生20天的低频访问不足规定时长容量费用。有关存储费用的更多信息，请参见[存储费用](/cn.zh-CN/计量计费/计量项和计费项/存储费用.md)。

## 请求语法

```
PUT /DestObjectName HTTP/1.1
Host: DestBucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
x-oss-copy-source: /SourceBucketName/SourceObjectName
```

## 请求头

拷贝操作涉及到的请求头均以x-oss-开头，因此所有请求头都要加到签名字符串中。

|名称|类型|是否必选|取值|描述|
|:-|:-|:---|--|:-|
|x-oss-forbid-overwrite|字符串|否|true|指定CopyObject操作时是否覆盖同名目标Object。当目标Bucket处于已开启或已暂停版本控制状态时，x-oss-forbid-overwrite请求Header设置无效，即允许覆盖同名Object。-   不指定x-oss-forbid-overwrite时，默认覆盖同名目标Object。
-   指定x-oss-forbid-overwrite为true时，表示禁止覆盖同名Object；指定x-oss-forbid-overwrite为false时，表示允许覆盖同名目标Object。

设置x-oss-forbid-overwrite请求Header会导致QPS处理性能下降，如果您有大量的操作需要使用x-oss-forbid-overwrite请求Header（QPS \> 1000），请联系技术支持，避免影响您的业务。 |
|x-oss-copy-source|字符串|是|/oss-example/oss.jpg|指定拷贝的源地址。 默认值：无 |
|x-oss-copy-source-if-match|字符串|否|5B3C1A2E053D763E1B002CC607C5\*\*\*\*|如果源Object的ETag值和您提供的ETag相等，则执行拷贝操作，并返回200 OK；否则返回412 Precondition Failed错误码（预处理失败）。 默认值：无 |
|x-oss-copy-source-if-none-match|字符串|否|5B3C1A2E053D763E1B002CC607C5\*\*\*\*|如果源Object的ETag值和您提供的ETag不相等，则执行拷贝操作，并返回200 OK；否则返回304 Not Modified错误码（预处理失败）。 默认值：无 |
|x-oss-copy-source-if-unmodified-since|字符串|否|2019-04-09T07:01:56.000Z|如果指定的时间等于或者晚于文件实际修改时间，则正常拷贝文件，并返回200 OK；否则返回412 Precondition Failed错误码（预处理失败）。 默认值：无 |
|x-oss-copy-source-if-modified-since|字符串|否|2019-04-09T07:01:56.000Z|如果源Object在用户指定的时间以后被修改过，则执行拷贝操作；否则返回304 Not Modified错误码（预处理失败）。 默认值：无 |
|x-oss-metadata-directive|字符串|否|COPY|指定如何设置目标Object的元信息。取值如下： -   COPY（默认值）：复制源Object的元数据到目标Object。

OSS不会复制源Object的x-oss-server-side-encryption属性配置到目标Object。目标Object的服务器端加密编码方式取决于当前拷贝操作是否指定了x-oss-server-side-encryption。

-   REPLACE：忽略源Object的元数据，直接采用请求中指定的元数据。

**说明：** 如果拷贝操作的源Object地址和目标Object地址相同，且未开启版本控制时，则无论x-oss-metadata-directive为何值，都会忽略源Object的元数据，目标Object将直接采用请求中指定的元数据。 |
|x-oss-server-side-encryption|字符串|否|AES256|指定OSS创建目标Object时，服务器端熵编码加密算法 。 取值：AES256、KMS（您需要购买 KMS 套件，才可以使用KMS加密算法，否则会返回KmsServiceNotEnabled错误码）

-   如果拷贝操作中未指定x-oss-server-side-encryption，则无论源Object是否进行过服务器端加密编码，拷贝后的目标Object均不进行服务器端加密编码。
-   如果拷贝操作中指定了x-oss-server-side-encryption，则无论源Object是否进行过服务器端加密编码，拷贝后的目标Object均会进行服务器端加密编码。并且拷贝操作的响应头中会包含x-oss-server-side-encryption，值为目标Object的加密算法。在目标Object被下载时，响应头中也会包含x-oss-server-side-encryption，值为该Object的加密算法。 |
|x-oss-server-side-encryption-key-id|字符串|否|9468da86-3509-4f8d-a61e-6eab1eac\*\*\*\*|表示KMS托管的用户主密钥。 该参数仅在x-oss-server-side-encryption为KMS时有效。 |
|x-oss-object-acl|字符串|否|private|指定OSS创建目标Object时的访问权限。 取值：public-read、private、public-read-write、default。

有关访问权限的更多信息，请参见[读写权限ACL](/cn.zh-CN/开发指南/数据安全/访问控制/读写权限ACL.md)。 |
|x-oss-storage-class|字符串|否|Standard|指定Object的存储类型。 取值：Standard、IA、Archive和ColdArchive

有关存储类型的更多信息，请参见[存储类型介绍](/cn.zh-CN/开发指南/存储类型/存储类型介绍.md)。

支持配置该请求头的的接口：PutObject、InitMultipartUpload、AppendObject、 PutObjectSymlink、CopyObject。 |
|x-oss-tagging|字符串|否|a:1|指定Object的对象标签，可同时设置多个标签，例如：TagA=A&TagB=B。 **说明：** Key和Value需要先进行URL编码，如果某项没有“=”，则看作Value为空字符串。 |
|x-oss-tagging-directive|字符串|否|Copy|指定如何设置目标Object的对象标签。取值如下： -   Copy（默认值）：复制源Object的对象标签到目标 Object。
-   Replace：忽略源Object的对象标签，直接采用请求中指定的对象标签。 |

此接口涉及的公共请求头，例如Host、Date等。更多信息，请参见[公共请求头（Common Request Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 响应头

此接口涉及的公共响应头，例如Content-Length、Connection等。更多信息，请参见[公共响应头（Common Response Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 响应元素

|名称|类型|示例值|描述|
|:-|:-|---|:-|
|CopyObjectResult|字符串|不涉及|CopyObject的结果。 默认值：无 |
|ETag|字符串|5B3C1A2E053D763E1B002CC607C5\*\*\*\*|目标Object的ETag值。 父元素：CopyObjectResult |
|LastModified|字符串|Fri, 24 Feb 2012 07:18:48 GMT|目标Object最后更新时间。 父元素：CopyObjectResult |

## 示例

-   未开启版本控制的简单请求示例

    ```
    PUT /test%2FAK.txt HTTP/1.1
    Host: tesx.oss-cn-zhangjiakou.aliyuncs.com
    Accept-Encoding: identity
    User-Agent: aliyun-sdk-python/2.6.0(Windows/7/AMD64;3.7.0)
    Accept: text/html
    Connection: keep-alive
    x-oss-copy-source: /test/AK.txt
    date: Fri, 28 Dec 2018 09:41:55 GMT
    authorization: OSS qn6qrrqxo2oawuk53otfjbyc:gmnwPKuu20LQEjd+iPkL259A****
    Content-Length: 0
    ```

    返回示例

    `x-oss-hash-crc64ecma`表明Object的64位CRC值。该64位CRC值根据[ECMA-182](http://www.ecma-international.org/publications/standards/Ecma-182.htm)标准计算得出。进行CopyObject操作时，生成的Object不保证具有64位CRC值。

    ```
    HTTP/1.1 200 OK
    Server: AliyunOSS
    Date: Fri, 28 Dec 2018 09:41:56 GMT
    Content-Type: application/xml
    Content-Length: 184
    Connection: keep-alive
    x-oss-request-id: 5C25EFE4462CE00EC6D87156
    ETag: "F2064A169EE92E9775EE5324D0B1****"
    x-oss-hash-crc64ecma: 12753002859196105360
    x-oss-server-time: 150
    <?xml version="1.0" encoding="UTF-8"?>
    <CopyObjectResult>
      <ETag>"F2064A169EE92E9775EE5324D0B1****"</ETag>
      <LastModified>2018-12-28T09:41:56.000Z</LastModified>
    </CopyObjectResult>
    ```

-   未指定versionId进行拷贝的请求示例

    ```
    PUT /dest-object-example HTTP/1.1
    Host: versioning-copy.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 03:45:32 GMT
    Authorization: OSS qeyxjc9arppwa0t:QqwOjq7U7j04NVpPqdfcVk0I****
    x-oss-copy-source: /versioning-copy-source/source-object
    ```

    返回示例

    示例中的`x-oss-copy-source-version-id`为源拷贝Object的versionId，在该示例中即为源拷贝Object的当前版本。`x-oss-version-id`为新拷贝生成Object的versionId。

    ```
    HTTP/1.1 200 OK
    x-oss-copy-source-version-id: CAEQNRiBgIC28uaA0BYiIDY5OGIwNmNlNjYyMTRjNTc4N2M2OGNiMjZkZTQ2****
    x-oss-version-id: CAEQNxiBgIDG8uaA0BYiIGZhZDRkZTk5Zjg3YzRhNzdiMWEwZGViNDM1NTFh****
    x-oss-request-id: 5CAC155CB7AEADE01700****
    Content-Type: application/xml
    Content-Length: 184
    Connection: keep-alive
    Date: Tue, 09 Apr 2019 03:45:32 GMT
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <CopyObjectResult>
      <ETag>"C81E728D9D4C2F636F067F89CC14****"</ETag>
      <LastModified>2019-04-09T03:45:32.000Z</LastModified>
    </CopyObjectResult>
    ```

-   指定versionId进行拷贝的请求示例

    ```
    PUT /dest-object-example HTTP/1.1
    Host: versioning-copy.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 03:45:32 GMT
    Authorization: OSS qeyxjc9arppwa0t:5qG4DLaHjxDPtpLlf2e8fBfX****
    x-oss-copy-source: /versioning-copy-source/source-object?versionId=CAEQNRiBgICv8uaA0BYiIDliZDc3MTc1NjE5MjRkMDI4ZGU4MTZkYjY1ZDgy****
    ```

    返回示例

    示例中的`x-oss-copy-source-version-id`为源拷贝Object的versionId，在该示例中即为`x-oss-copy-source`中versionId指定的版本，`x-oss-version-id`为新拷贝生成Object的versionId。

    ```
    HTTP/1.1 200 OK
    x-oss-copy-source-version-id: CAEQNRiBgICv8uaA0BYiIDliZDc3MTc1NjE5MjRkMDI4ZGU4MTZkYjY1ZDgy****
    x-oss-version-id: CAEQNxiBgMDP8uaA0BYiIDIyNGNhZDQ1M2M3NzRkZThiNzE0N2I3ZDkxOWY4****
    x-oss-request-id: 5CAC155CB7AEADE01700****
    Content-Type: application/xml
    Content-Length: 184
    Connection: keep-alive
    Date: Tue, 09 Apr 2019 03:45:32 GMT
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <CopyObjectResult>
      <ETag>"C4CA4238A0B923820DCC509A6F75****"</ETag>
      <LastModified>2019-04-09T03:45:32.000Z</LastModified>
    </CopyObjectResult>
    ```


## SDK

此接口所对应的各语言SDK如下：

-   [Java](/cn.zh-CN/SDK 示例/Java/管理文件/拷贝文件.md)
-   [Python](/cn.zh-CN/SDK 示例/Python/管理文件/拷贝文件.md)
-   [PHP](/cn.zh-CN/SDK 示例/PHP/管理文件/拷贝文件.md)
-   [Go](/cn.zh-CN/SDK 示例/Go/管理文件/拷贝文件.md)
-   [C](/cn.zh-CN/SDK 示例/C/管理文件/拷贝文件.md)
-   [.NET](/cn.zh-CN/SDK 示例/.NET/管理文件/拷贝文件.md)
-   [Android](/cn.zh-CN/SDK 示例/Android/管理文件/拷贝文件.md)
-   [iOS](/cn.zh-CN/SDK 示例/iOS/管理文件/概述.md)
-   [Node.js](/cn.zh-CN/SDK 示例/Node.js/管理文件/概述.md)
-   [Ruby](/cn.zh-CN/SDK 示例/Ruby/管理文件.md)

## 错误码

|错误码|HTTP状态码|描述|
|:--|:------|:-|
|InvalidArgument|400|x-oss-storage-class等参数的值无效。|
|Precondition Failed|412|-   指定了x-oss-copy-source-if-match请求头，但源Object的ETag值和您提供的ETag不相等。
-   指定了x-oss-copy-source-if-unmodified-since请求头，且指定的时间早于文件实际修改时间。 |
|Not Modified|304|-   指定了x-oss-copy-source-if-none-match请求头，且源Object的ETag值和您提供的ETag相等。
-   指定了x-oss-copy-source-if-modified-since请求头，但源Object在指定的时间后没被修改过。 |
|KmsServiceNotEnabled|403|将x-oss-server-side-encryption指定为KMS，但没有预先购买KMS套件。|
|FileAlreadyExists|409|当请求的Header中携带x-oss-forbid-overwrite=true时，表示禁止覆盖同名文件。如果文件已存在，则返回此错误。|
|FileImmutable|409|Bucket内的数据处于被保护状态时，若您尝试删除或修改这些数据，则返回此错误码。|

