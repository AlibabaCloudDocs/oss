# PostObject

调用PostObject用于通过HTML表单上传的方式将文件（Object）上传到指定存储空间（Bucket）。

## 注意事项

-   Post请求需要对Bucket拥有写权限。如果Bucket为public-read-write，可以不上传签名信息，否则要求对该操作进行签名验证。
-   与Put操作不同，Post操作使用AccessKey Secret对Policy进行签名，计算出签名字符串作为Signature表单域的值，OSS通过验证该值从而判断签名的合法性。
-   提交表单的URL为Bucket域名，不需要在URL中指定Object。即请求行是`POST / HTTP/1.1`，不能写为`POST /ObjectName HTTP/1.1`。
-   如果POST请求中包含Header签名信息或URL签名信息，OSS不会对此类信息做检查。

## 版本控制

在开启了版本控制的Bucket中发起PostObject请求时，OSS将为新添加的Object自动生成唯一的版本ID，并在响应Header中通过x-oss-version-id的形式返回。

在暂停了版本控制的Bucket中发起PostObject请求时，OSS将为新添加的Object自动生成一个null的版本ID，并在响应Header中通过x-oss-version-id的形式返回。同一个Object仅允许一个null的版本ID。

## 请求语法

```
POST / HTTP/1.1 
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
User-Agent: browser_data
Content-Length：ContentLength
Content-Type: multipart/form-data; boundary=9431149156168
--9431149156168
Content-Disposition: form-data; name="key"
key
--9431149156168
Content-Disposition: form-data; name="success_action_redirect"
success_redirect
--9431149156168
Content-Disposition: form-data; name="Content-Disposition"
attachment;filename=oss_download.jpg
--9431149156168
Content-Disposition: form-data; name="x-oss-meta-uuid"
myuuid
--9431149156168
Content-Disposition: form-data; name="x-oss-meta-tag"
mytag
--9431149156168
Content-Disposition: form-data; name="OSSAccessKeyId"
access-key-id
--9431149156168
Content-Disposition: form-data; name="policy"
encoded_policy
--9431149156168
Content-Disposition: form-data; name="Signature"
signature
--9431149156168
Content-Disposition: form-data; name="file"; filename="MyFilename.jpg"
Content-Type: image/jpeg
file_content
--9431149156168
Content-Disposition: form-data; name="submit"
Upload to OSS
--9431149156168--
```

## 请求头

**说明：** PostObject的消息实体通过多重表单格式（multipart/form-data）编码，在PutObject操作中参数通过HTTP请求头传递，在PostObject操作中则作为消息体中的表单域传递。PostObject不支持传入x-oss-tagging请求头。

|名称|类型|是否必选|描述|
|:-|:-|----|:-|
|Content-Type|字符串|否|指定文件的类型和网页的编码，确定浏览器读取文件的形式和编码。Post操作提交表单编码必须为`multipart/form-data`，即Header中Content-Type为`multipart/form-data;boundary=xxxxxx`的形式。

boundary为边界字符串，是由表单随机生成的一个随机值，无需指定。如果通过SDK拼表单，则SDK也会生成一个随机值。 |
|x-oss-object-acl|字符串|否|在请求头中指定Object的访问权限。 当请求头和文件表单域中同时指定x-oss-object-acl时，文件表单域中设置的访问权限的优先级高于请求中设置的访问权限。

取值：

-   default（默认）：Object遵循所在存储空间的访问权限。
-   private：Object是私有资源。只有Object的拥有者和授权用户有该Object的读写权限，其他用户没有权限操作该Object。
-   public-read：Object是公共读资源。只有Object的拥有者和授权用户有该Object的读写权限，其他用户只有该Object的读权限。请谨慎使用该权限。
-   public-read-write：Object是公共读写资源。所有用户都有该Object的读写权限。请谨慎使用该权限。

关于访问权限的更多信息，请参见[读写权限ACL](/cn.zh-CN/开发指南/数据安全/访问控制/读写权限ACL.md)。 |

此接口还需要包含Host、Date等公共请求头。更多信息，请参见[公共请求头（Common Request Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 请求元素

|名称|类型|是否必选|描述|
|:-|:-|----|:-|
|Cache-Control|字符串|否|指定该Object被下载时网页的缓存行为。更多信息，请参见[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 默认值：无 |
|Content-Disposition|字符串|否|指定该Object被下载时的名称。更多信息，请参见[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 默认值：无 |
|Content-Encoding|字符串|否|指定该Object被下载时的内容编码格式。更多信息，请参见[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 默认值：无 |
|Expires|字符串|否|过期时间。更多信息，请参见[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 默认值：无 |
|OSSAccessKeyId|字符串|是，有条件|Bucket拥有者的AccessKey ID。 默认值：无

限制：当Bucket为非public-read-write或者提供了Policy（或Signature）表单域时，必须提供OSSAccessKeyId表单域。 |
|policy|字符串|是，有条件|Policy规定了请求表单域的合法性。不包含Policy表单域的请求被认为是匿名请求，并只能访问public-read-write的Bucket。默认值：无

限制：当Bucket为非public-read-write或者提供了OSSAccessKeyId（或Signature）表单域时，必须提供Policy表单域。

**说明：** 表单和Policy必须使用UTF-8编码，且Policy表单域要经过Base64编码。 |
|Signature|字符串|是，有条件|根据AccessKey Secret和Policy计算的签名信息，OSS验证该签名信息从而验证该Post请求的合法性。更多信息，请参见[附录：Post Signature](#section_wny_mww_wdb)。 默认值：无

限制：当Bucket为非public-read-write或者提供了OSSAccessKeyId（或Policy）表单域时，必须提供Signature表单域。

**说明：** 表单域对大小写不敏感，但表单域的值对大小写敏感。 |
|x-oss-server-side-data-encryption|字符串|否|仅当加密方式为KMS时，通过此选项将默认加密算法AES256修改为SM4。取值：SM4 |
|x-oss-server-side-encryption-key-id|字符串|否|表示KMS托管的用户主密钥。 此选项仅当x-oss-server-side-encryption值为KMS时有效。|
|x-oss-content-type|字符串|否|由于浏览器会自动在文件表单域中增加Content-Type，为了解决此问题，OSS支持用户在Post请求体中增加x-oss-content-type，该项允许用户指定Content-Type，且拥有最高优先级。优先级顺序：x-oss-content-type \> 文件表单域的Content-Type \> Content-Type

默认值：无 |
|x-oss-forbid-overwrite|字符串|否|指定PostObject操作时是否覆盖同名Object。当目标Bucket处于已开启或已暂停的版本控制状态时，x-oss-forbid-overwrite请求Header设置无效，即允许覆盖同名Object。

-   不指定x-oss-forbid-overwrite指定x-oss-forbid-overwrite为false时，表示允许覆盖同名Object。
-   指定x-oss-forbid-overwrite为true时，表示禁止覆盖同名Object；

设置x-oss-forbid-overwrite请求Header会导致QPS处理性能下降，如果您有大量的操作需要使用x-oss-forbid-overwrite请求头（QPS \> 1000），请联系技术支持，避免影响您的业务。 |
|x-oss-object-acl|字符串|否|在文件表单域中指定Object的访问权限。此项支持同时在请求头和文件表单域中指定。当请求头和文件表单域中同时指定x-oss-object-acl时，文件表单域中设置的访问权限的优先级高于请求中设置的访问权限。

取值：

-   default（默认）：Object遵循所在存储空间的访问权限。
-   private：Object是私有资源。只有Object的拥有者和授权用户有该Object的读写权限，其他用户没有权限操作该Object。
-   public-read：Object是公共读资源。只有Object的拥有者和授权用户有该Object的读写权限，其他用户只有该Object的读权限。请谨慎使用该权限。
-   public-read-write：Object是公共读写资源。所有用户都有该Object的读写权限。请谨慎使用该权限。

关于访问权限的更多信息，请参见[读写权限ACL](/cn.zh-CN/开发指南/数据安全/访问控制/读写权限ACL.md)。 |
|x-oss-storage-class|字符串|否|指定Object的存储类型。对于任意存储类型的Bucket，如果上传Object时指定此参数，则此次上传的Object将存储为指定的类型。例如在IA类型的Bucket中上传Object时，如果指定x-oss-storage-class为Standard，则该Object直接存储为Standard。

取值：

-   Standard：标准存储
-   IA：低频访问
-   Archive：归档存储
-   ColdArchive：冷归档存储

关于存储类型的更多信息，请参见[存储类型介绍](/cn.zh-CN/开发指南/存储类型/存储类型介绍.md)。 |
|key|字符串|是|上传Object的名称。如果名称包含路径，例如`destfolder/example.jpg`，则OSS会自动创建相应的文件夹。 默认值：无 |
|success\_action\_redirect|字符串|否|上传成功后客户端跳转到的URL。如果未指定该表单域，返回结果由success\_action\_status表单域指定。如果上传失败，OSS返回错误码，并不进行跳转。 默认值：无 |
|success\_action\_status|字符串|否|未指定success\_action\_redirect表单域时，该表单域指定了上传成功后返回给客户端的状态码。 默认值：无

有效值：200、201、204（默认）。

-   如果该域的值设置为200或者204，OSS返回一个空文档和相应的状态码。
-   如果该域的值设置为201，OSS返回一个XML文件和201状态码。
-   如果该域的值未设置或者设置为一个非法值，OSS返回一个空文档和204状态码。 |
|x-oss-meta-\*|字符串|否|用户指定的user meta值。 默认值：无

如果请求中携带以x-oss-meta-为前缀的表单域，则视为user meta，例如`x-oss-meta-location`。

**说明：** 一个Object可以有多个类似的参数，但所有的user meta总大小不能超过8 KB。 |
|x-oss-security-token|字符串|否|如果本次访问是使用STS临时授权方式，则需要指定该项为SecurityToken的值，同时OSSAccessKeyId需要使用与之配对的临时AccessKey Id。计算签名时，与使用普通AccessKey Id签名方式一致。 默认值：无

搭建STS服务的具体操作请参见开发指南中的[使用STS临时访问凭证访问OSS](/cn.zh-CN/开发指南/数据安全/访问控制/使用STS临时访问凭证访问OSS.md)。您可以通过调用STS服务的[AssumeRole](/cn.zh-CN/API参考/API 参考（STS）/操作接口/AssumeRole.md)接口或者使用[各语言STS SDK](/cn.zh-CN/SDK参考/SDK参考（STS）/STS SDK概览.md)来获取临时访问凭证。 |
|file|字符串|是|文件或文本内容。浏览器会自动根据文件类型来设置Content-Type，并覆盖用户的设置。OSS一次只能上传一个文件。 默认值：无

**说明：** file必须是表单中的最后一个域。 |

## 响应头

|名称|类型|描述|
|:-|:-|:-|
|x-oss-server-side-encryption|字符串|如果请求Header中指定了x-oss-server-side-encryption，则响应Header中将包含该头部，指明所使用的服务器端加密算法。|

此接口还需包含公共响应头。更多信息，请参见[公共响应头（Common Response Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 响应元素

|名称|类型|描述|
|:-|:-|:-|
|PostResponse|容器|保存Post请求结果的容器。 子节点：Bucket、ETag、Key、Location |
|Bucket|字符串|Bucket名称。 父节点：PostResponse |
|ETag|字符串|ETag \(Entity Tag\) 在每个Object生成的时候被创建。Post请求创建的Object，ETag值是该Object内容的UUID，可以用于检查该Object内容是否发生变化。 父节点：PostResponse |
|Location|字符串|新创建Object的URL。 父节点：PostResponse |

## 示例

-   请求示例：

    ```
    POST / HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Content-Length: 344606
    Content-Type: multipart/form-data; boundary=9431149156168
    --9431149156168
    Content-Disposition: form-data; name="key"
    /user/a/objectName.txt
    --9431149156168
    Content-Disposition: form-data; name="success_action_status"
    200
    --9431149156168
    Content-Disposition: form-data; name="Content-Disposition"
    content_disposition
    --9431149156168
    Content-Disposition: form-data; name="x-oss-meta-uuid"
    uuid
    --9431149156168
    Content-Disposition: form-data; name="x-oss-meta-tag"
    metadata
    --9431149156168
    Content-Disposition: form-data; name="OSSAccessKeyId"
    44CF9590006BF252F707
    --9431149156168
    Content-Disposition: form-data; name="policy"
    eyJleHBpcmF0aW9uIjoiMjAxMy0xMi0wMVQxMjowMDowMFoiLCJjb25kaXRpb25zIjpbWyJjb250ZW50LWxlbmd0aC1yYW5nZSIsIDAsIDEwNDg1NzYwXSx7ImJ1Y2tldCI6ImFoYWhhIn0sIHsiQSI6ICJhIn0seyJrZXkiOiAiQUJDIn1dfQ==
    --9431149156168
    Content-Disposition: form-data; name="Signature"
    kZoYNv66bsmc10+dcGKw5x2PRrk=
    --9431149156168
    Content-Disposition: form-data; name="file"; filename="MyFilename.txt"
    Content-Type: text/plain
    abcdefg
    --9431149156168
    Content-Disposition: form-data; name="submit"
    Upload to OSS
    --9431149156168--
    ```

-   返回示例：

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 61d2042d-1b68-6708-5906-33d81921362e 
    Date: Fri, 24 Feb 2014 06:03:28 GMT
    ETag: "5B3C1A2E053D763E1B002CC607C5****"
    Connection: keep-alive
    Content-Length: 0  
    Server: AliyunOSS
    ```


## 错误码

|错误码|HTTP状态码|描述|
|---|-------|--|
|InvalidArgument|400|无论Bucket是否为public-read-write，一旦上传OSSAccessKeyId、Policy、Signature表单域中的任意一个，则另两个表单域为必选项。如果缺失，将返回此错误。|
|InvalidDigest|400|用户上传了Content-MD5请求Header，OSS会计算body的Content-MD5并检查一致性，如果不一致，将返回此错误。|
|EntityTooLarge|400|请求的body总长度不允许超过5 GB。如果文件长度过大，将返回此错误。|
|InvalidEncryptionAlgorithmError|400|如果上传时指定了x-oss-server-side-encryption Header请求域，则必须设置其值为AES256、KMS或SM4。如果设置为其他值，将返回此错误。|
|IncorrectNumberOfFilesInPOSTRequest|400|请求必须指定key表单域。如果缺失，将返回此错误。|
|FileAlreadyExists|409|请求的Header中携带x-oss-forbid-overwrite=true时，表示禁止覆盖同名文件。如果文件已存在，将返回此错误。|
|KmsServiceNotEnabled|403|将x-oss-server-side-encryption指定为KMS，但没有预先购买KMS套件。|
|FileImmutable|409|Bucket内的数据处于被保护状态时，如果您尝试删除或修改这些数据，将返回此错误码。|

## 附录：Post Policy

Post请求的Policy表单域用于验证请求的合法性。Policy为一段经过UTF-8和Base64编码的JSON文本，声明了Post请求必须满足的条件。虽然对于public-read-write的Bucket上传时，Post表单域为可选项，但强烈建议使用该域来限制Post请求。

Post Policy示例如下：

```
{ "expiration": "2014-12-01T12:00:00.000Z",
  "conditions": [
    {"bucket": "johnsmith" },
    ["starts-with", "$key", "user/eric/"]
  ]
}
```

Post Policy中必须包含Expiration和Conditions。

-   Expiration

    Expiration项指定了Policy的过期时间，以ISO8601 GMT时间表示。例如`2014-12-01T12:00:00.000Z`指定了Post请求必须在2014年12月1日12点之前。

-   Conditions

    Conditions是一个列表，可以用于指定Post请求的表单域的合法值。

    **说明：** 表单域对应的值在检查Policy之后进行扩展，因此，Policy中设置的表单域的合法值应当对应于扩展之前的表单域的值。

    Policy中支持的Conditions项请参见下表。

    |名称|描述|
    |:-|:-|
    |content-length-range|上传Object的最小和最大允许大小。该Condition支持contion-length-range匹配方式。|
    |Cache-Control, Content-Type, Content-Disposition, Content-Encoding, Expires|HTTP请求Header。该Condition支持精确匹配和starts-with匹配方式。 **说明：** 建议您在Policy中增加Content-Type限制，防止通过表单上传时Content-Type被恶意修改。 |
    |key|Object名称。该Condition支持精确匹配和starts-with匹配方式。|
    |success\_action\_redirect|上传成功后的跳转URL地址。该Condition支持精确匹配和starts-with匹配方式。|
    |success\_action\_status|未指定success\_action\_redirect时，上传成功后的返回状态码。该Condition支持精确匹配和starts-with匹配方式。|
    |x-oss-meta-\*|用户指定的user meta。该Condition支持精确匹配和starts-with匹配方式。|

    如果Post请求中包含额外的表单域，OSS可以将这些额外的表单域加入到Policy的Conditions中并检查合法性。

-   Conditions匹配方式

    |Conditions匹配方式|描述|
    |:-------------|:-|
    |精确匹配|表单域的值必须精确匹配Conditions中声明的值。如指定key表单域的值必须为a：\{“key”: “a”\} ，则可以写为\[“eq”, “$key”, “a”\]|
    |Starts With|表单域的值必须以指定值开始。例如指定key的值必须以user/user1开始：\[“starts-with”, “$key”, “user/user1”\]|
    |指定文件大小范围|指定所允许上传的文件最大和最小范围，例如允许的文件大小为1~10字节：\[“content-length-range”, 1, 10\]|


在Post Policy中$表示变量。如果要描述$，需要使用转义字符\\$。下表描述了在Post Policy的JSON中需要进行转义的字符。

|转义字符|描述|
|:---|:-|
|\\/|斜杠|
|\\\\|反斜杠|
|\\”|双引号|
|\\$|美元符|
|\\b|空格|
|\\f|换页|
|\\n|换行|
|\\r|回车|
|\\t|水平制表符|
|\\uxxxx|Unicode字符|

## 附录：Post Signature

对于验证的Post请求，HTML表单中必须包含Policy和Signature信息。Policy控制请求中哪些值是允许的。

计算Signature的具体流程如下：

1.  创建一个UTF-8编码的Policy。
2.  将Policy进行Base64编码，其值即为Policy表单域填入的值，将该值作为将要签名的字符串。
3.  使用AccessKeySecret对要签名的字符串进行签名，签名方法为`Signature = base64(hmac-sha1(base64(policy), AccessKeySecret))`。

## 更多参考

-   Web端表单直传OSS示例：[JavaScript客户端签名直传](/cn.zh-CN/最佳实践/Web端上传数据至OSS/Web端PostObject直传实践/JavaScript客户端签名直传.md)
-   Java示例：[表单上传](/cn.zh-CN/SDK 示例/Java/上传文件/表单上传.md)

