# AppendObject

调用AppendObject接口用于以追加写的方式上传文件（Object）。通过AppendObject操作创建的Object类型为Appendable Object，而通过PutObject上传的Object是Normal Object。

## 版本控制

在目标Bucket处于开启或暂停版本控制状态下，对Appendable类型Object执行相关操作时，有如下注意事项：

-   仅允许对当前版本为Appendable类型的Object执行追加上传（AppendObject）操作，且OSS不会为该Appendable类型的Object生成历史版本。

    对当前版本为Appendable类型的Object执行PutObject或DeleteObject操作时，OSS会将该Appendable类型的Object保留为历史版本，但该Object不允许继续追加。

-   不支持对Appendable类型Object执行拷贝操作。
-   不支持对非Appendable类型的Object，包括Normal Object、删除标记（Delete Marker）等执行AppendObject操作。

## 使用限制

-   通过AppendObject方式最后生成的Object大小不得超过5 GB。
-   处于[合规保留策略](/cn.zh-CN/开发指南/数据安全/合规保留策略.md)保护期的Object不支持AppendObject操作。
-   AppendableObject不支持指定CMK ID进行服务端KMS加密。

## 请求语法

```
POST /ObjectName?append&position=Position HTTP/1.1
Content-Length：ContentLength
Content-Type: ContentType
Host: BucketName.oss.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 请求头

**说明：** append和position均为[CanonicalizedResource](/cn.zh-CN/API 参考/访问控制/在Header中包含签名.md)，需要包含在签名中。

|名称|类型|是否必选|示例值|描述|
|:-|:-|:---|---|:-|
|append|字符串|是|不涉及|用于指定AppendObject操作。每次执行AppendObject都会更新该Object的最后修改时间。|
|position|字符串|是|0|用于指定从何处进行追加。 每次操作成功后，响应消息头x-oss-next-append-position会标明下一次追加的position。首次追加操作的position必须为0，后续追加操作的position是Object的当前大小。例如，第一次AppendObject请求指定position值为0，content-length是65536，则第二次AppendObject需要指定position为65536。

-   当position值为0，且不存在同名Object时，则AppendObject与PutObject请求类似，即允许设置x-oss-server-side-encryption等请求头。如果加入了正确的x-oss-server-side-encryption头，那么后续的AppendObject响应头部也会包含x-oss-server-side-encryption头。后续如需更改元数据，可以使用CopyObject接口。
-   在position值正确的情况下，对已存在的Appendable Object追加一个大小为0的内容，该操作不会改变Object的状态。 |
|Cache-control|字符串|否|no-cache|指定该Object的网页缓存行为。更多信息，请参见[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 默认值：无 |
|Content-Disposition|字符串|否|attachment;filename=oss\_download.jpg|指定该Object被下载时的名称。更多信息，请参见[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。

默认值：无 |
|Content-Encoding|字符串|否|utf-8|指定该Object的内容编码格式。更多信息，请参见[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 默认值：无 |
|Content-MD5|字符串|否|ohhnqLBJFiKkPSBO1eNaUA==|Content-MD5是一串由MD5算法生成的值，该请求头用于检查消息内容是否与发送时一致。 获取Content-MD5值：对消息内容（不包括头部）执行MD5算法，获得128比特位数字，然后对该数字进行base64编码。

默认值：无

限制：无 |
|Expires|GMT|否|Wed, 08 Jul 2015 16:57:01 GMT|过期时间。更多信息，请参见[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 默认值：无 |
|x-oss-server-side-encryption|字符串|否|AES256|指定服务器端加密方式。 合法值：

-   AES256：使用OSS完全托管密钥进行加解密（SSE-OSS）。
-   KMS：使用KMS托管密钥进行加解密。
-   SM4：国密SM4算法。 |
|x-oss-object-acl|字符串|否|private|指定Object的访问权限。 取值：

-   default（默认）：Object遵循所在存储空间的访问权限。
-   private：Object是私有资源。只有Object的拥有者和授权用户有该Object的读写权限，其他用户没有权限操作该Object。
-   public-read：Object是公共读资源。只有Object的拥有者和授权用户有该Object的读写权限，其他用户只有该Object的读权限。请谨慎使用该权限。
-   public-read-write：Object是公共读写资源。所有用户都有该Object的读写权限。请谨慎使用该权限。

关于访问权限的更多信息，请参见[读写权限ACL](/cn.zh-CN/开发指南/数据安全/访问控制/读写权限ACL.md)。 |
|x-oss-storage-class|字符串|否|Standard|指定Object的存储类型。对于任意存储类型的Bucket，如果上传Object时指定此参数，则此次上传的Object将存储为指定的类型。例如在IA类型的Bucket中上传Object时，如果指定x-oss-storage-class为Standard，则该Object直接存储为Standard。

取值：

-   Standard：标准存储
-   IA：低频访问
-   Archive：归档存储

关于存储类型的更多信息，请参见[存储类型介绍](/cn.zh-CN/开发指南/存储类型/存储类型介绍.md)。

**说明：** 该值仅在首次执行AppendObject操作时有效，后续追加时不生效。 |
|x-oss-meta-\*|字符串|否|x-oss-meta-location|创建AppendObject时可以添加x-oss-meta-\*，继续追加时不可以携带此参数。如果配置以x-oss-meta-\*为前缀的参数，则该参数视为元数据。元数据大小限制：一个Object可以包含多个元数据，但所有的元数据总大小不能超过8 KB。

元数据命名规则：支持短划线（-）、数字、英文字母（a~z）。英文字符的大写字母会被转成小写字母，不支持下划线（\_）在内的其他字符。 |

关于此接口涉及的其他公共请求头的更多信息，请参见[公共请求头（Common Request Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 响应头

|响应消息头|类型|示例|描述|
|-----|--|--|--|
|x-oss-next-append-position|64位整型|1717|下一次请求应当提供的position，即当前Object大小。 当AppendObject执行成功，或者因position和Object大小不匹配而引起的409错误时，会返回此消息头。 |
|x-oss-hash-crc64ecma|64位整型|3231342946509354535|表明Object的64位CRC值。该64位CRC由[ECMA-182](http://www.ecma-international.org/publications/standards/Ecma-182.htm)标准计算得出。|

关于此接口涉及的其他公共响应头的更多信息，请参见[公共响应头（Common Response Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## CRC64的计算方式

Appendable Object的CRC64采用[ECMA-182](http://www.ecma-international.org/publications/standards/Ecma-182.htm)标准。您可以使用以下方法进行计算：

-   用Boost CRC模块的方式计算

    ```
    typedef boost::crc_optimal<64, 0x42F0E1EBA9EA3693ULL, 0xffffffffffffffffULL, 0xffffffffffffffffULL, true, true> boost_ecma;
    uint64_t do_boost_crc(const char* buffer, int length)
    {
        boost_ecma crc;
        crc.process_bytes(buffer, length);
        return crc.checksum();
    }
    ```

-   用Python crcmod的方式计算

    ```
    do_crc64 = crcmod.mkCrcFun(0x142F0E1EBA9EA3693L, initCrc=0L, xorOut=0xffffffffffffffffL, rev=True)
    print do_crc64(“123456789”)
    ```


## 和其他操作的关系

|其他操作|关系描述|
|:---|:---|
|[PutObject](/cn.zh-CN/API 参考/关于Object操作/基础操作/PutObject.md)|如果对一个已经存在的Appendable Object进行PutObject操作，该Appendable Object会被新的Object覆盖，类型转换为Normal Object。|
|[HeadObject](/cn.zh-CN/API 参考/关于Object操作/基础操作/HeadObject.md)|对Appendable Object执行HeadObject操作会返回x-oss-next-append-position、x-oss-hash-crc64ecma以及x-oss-object-type。Appendable Object的x-oss-object-type为Appendable。|
|[GetBucket \(ListObjects\)](/cn.zh-CN/API 参考/关于Bucket的操作/基础操作/GetBucket (ListObjects).md)|在GetBucket请求的响应中，会将Appendable Object的Type设为Appendable。|
|[CopyObject](/cn.zh-CN/API 参考/关于Object操作/基础操作/CopyObject.md)|-   允许使用CopyObject修改Appendable Object自定义的元信息。
-   不允许使用CopyObject拷贝一个Appendable Object。
-   不允许使用CopyObject修改Appendable Object的服务器端加密属性。 |

## 示例

-   请求示例

    ```
    POST /oss.jpg?append&position=0 HTTP/1.1 
    Host: oss-example.oss.aliyuncs.com 
    Cache-control: no-cache 
    Expires: Wed, 08 Jul 2015 16:57:01 GMT 
    Content-Encoding: utf-8 
    x-oss-storage-class: Archive
    Content-Disposition: attachment;filename=oss_download.jpg 
    Date: Wed, 08 Jul 2015 06:57:01 GMT 
    Content-Type: image/jpg 
    Content-Length: 1717 
    Authorization: OSS qn6qrrqxo2oawuk53otf****:kZoYNv66bsmc10+dcGKw5x2P****  
    [1717 bytes of object data]
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    Date: Wed, 08 Jul 2015 06:57:01 GMT
    ETag: "0F7230CAA4BE94CCBDC99C550000****"
    Connection: keep-alive
    Content-Length: 0  
    Server: AliyunOSS
    x-oss-hash-crc64ecma: 14741617095266562575
    x-oss-next-append-position: 1717
    x-oss-request-id: 559CC9BDC755F95A6448****
    ```

-   版本控制请求示例

    对于开启或暂停了版本控制的Bucket，AppendObject的响应Header中会返回x-oss-version-id，且x-oss-version-id只能为null。

    ```
    POST /example?append&position=0 HTTP/1.1 
    Host: versioning-append.oss.aliyuncs.com 
    Date: Tue, 09 Apr 2019 03:59:33 GMT
    Content-Length: 3
    Content-Type: application/octet-stream
    Authorization: OSS bwo4j5l8d3j****:MCY5nnfgfJU/f3Xe0odqBtG5****
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    Date: Tue, 09 Apr 2019 03:59:33 GMT
    ETag: "2776271A4A09D82CA518AC5C0000****"
    Connection: keep-alive
    Content-Length: 0  
    Server: AliyunOSS
    x-oss-version-id: null
    x-oss-hash-crc64ecma: 3231342946509354535
    x-oss-next-append-position: 3
    x-oss-request-id: 5CAC18A5B7AEADE01700****
    ```


## SDK

此接口所对应的各语言SDK如下：

-   [Java](/cn.zh-CN/SDK 示例/Java/上传文件/追加上传.md)
-   [Python](/cn.zh-CN/SDK 示例/Python/上传文件/追加上传.md)
-   [PHP](/cn.zh-CN/SDK 示例/PHP/上传文件/追加上传.md)
-   [Go](/cn.zh-CN/SDK 示例/Go/上传文件/追加上传.md)
-   [C](/cn.zh-CN/SDK 示例/C/上传文件/追加上传.md)
-   [.NET](/cn.zh-CN/SDK 示例/.NET/上传文件/追加上传.md)
-   [Android](/cn.zh-CN/SDK 示例/Android/上传文件/追加上传.md)
-   [iOS](/cn.zh-CN/SDK 示例/iOS/上传文件/概述.md)
-   [Ruby](/cn.zh-CN/SDK 示例/Ruby/上传文件.md)

## 错误码

|错误代码|HTTP 状态码|说明|
|:---|:-------|:-|
|ObjectNotAppendable|409|对一个非Appendable Object进行AppendObject操作。|
|PositionNotEqualToLength|409|-   position的值和当前Object的长度不一致。

您可以通过响应头x-oss-next-append-position得到下一次position，并再次进行请求。由于并发的关系，即使把position的值设置为x-oss-next-append-position，该请求依然可能因为PositionNotEqualToLength而失败。

-   position值为0时，如果没有同名Appendable Object，或者同名Appendable Object长度为0，该请求成功；其他情况均视为position和Object长度不匹配的情形，返回此错误码。 |
|InvalidArgument|400|x-oss-storage-class、x-oss-object-acl等值设置无效。|
|FileImmutable|409|Bucket内的数据处于被保护状态时，如果您尝试删除或修改这些数据，将返回此错误码。|
|KmsServiceNotEnabled|403|使用KMS加密算法时，没有在控制台预先开通密钥管理服务KMS。|

