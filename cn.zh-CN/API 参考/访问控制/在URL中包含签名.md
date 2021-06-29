# 在URL中包含签名

除了使用Authorization Header，您还可以在URL中加入签名信息，以便将该URL转给第三方实现授权访问。

## 注意事项

-   使用在URL中签名的方式，会将授权的数据在过期时间内曝露在互联网上，请预先评估使用风险。
-   OSS不支持同时在URL和Header中包含签名。
-   PUT和GET请求都支持在URL中签名。
-   您可以为PUT操作生成一个预签名的URL，该URL检查用户是否上传了正确的内容。SDK对请求进行预签名时，将计算请求正文的校验和，并生成包含在预签名URL中的MD5校验和。用户必须上传与SDK生成的MD5校验和相同的内容，否则操作失败。如果要验证MD5，只需在请求中增加Content-MD5头即可。

## 实现方式

-   URL签名示例

    ```
    http://oss-example.oss-cn-hangzhou.aliyuncs.com/oss-api.pdf?OSSAccessKeyId=nz2pc56s936****&Expires=1141889120&Signature=vjbyPxybdZaNmGa%2ByT272YEAiv****
    ```

    如果需要使用STS用户构造URL签名，则必须携带`security-token`，URL签名示例如下：

    ```
    http://oss-example.oss-cn-hangzhou.aliyuncs.com/oss-api.pdf?OSSAccessKeyId=nz2pc56s936****&Expires=1141889120&Signature=vjbyPxybdZaNmGa%2ByT272YEAiv****&security-token=SecurityToken
    ```

-   参数

    |名称|是否必选|描述|
    |--|----|--|
    |Expires|是|一个Unix时间戳（自UTC时间1970年01月01号开始的秒数），用于标识该URL的超时时间。如果OSS接收到该URL请求的时间晚于签名中包含的Expires参数时，则返回请求超时的错误码。例如当前时间是1141889060，开发者希望创建一个60秒后自动失效的URL，则可以设置Expires时间为1141889120。 **说明：** 出于安全考虑，OSS控制台中默认URL的有效时间为3600秒，最大值为32400秒。 |
    |OSSAccessKeyId|是|密钥中的AccessKeyId。|
    |Signature|是|签名信息。签名信息的格式如下：

    ```
Signature = urlencode(base64(hmac-sha1(AccessKeySecret,
          VERB + "\n" 
          + CONTENT-MD5 + "\n" 
          + CONTENT-TYPE + "\n" 
          + EXPIRES + "\n" 
          + CanonicalizedOSSHeaders
          + CanonicalizedResource)))
    ```

所有OSS支持的请求和各种Header参数，在URL中进行签名的算法和在Header中包含签名的算法基本一样。

生成URL中的签名字符串时，除了将Date参数替换为Expires参数外，仍然包含`CONTENT-TYPE`、`CONTENT-MD5`、`CanonicalizedOSSHeaders`等[在Header中包含签名](/cn.zh-CN/API 参考/访问控制/在Header中包含签名.md)中定义的Header（请求中虽然仍有Date请求Header，但无需将Date加入签名字符串中）。

在URL中包含签名时必须对URL进行urlencode。如果在URL中多次传入Signature、Expires或OSSAccessKeyId，则以第一次传入的值为准。

使用URL签名时，OSS会先验证请求时间是否晚于Expires时间，然后再验证签名。 |
    |security-token|否|安全令牌。只有当使用STS用户构造URL签名时，才需要设置此参数。**说明：** 关于搭建STS服务的具体操作，请参见开发指南中的[使用STS临时访问凭证访问OSS](/cn.zh-CN/开发指南/数据安全/使用STS临时访问凭证访问OSS.md)。您可以通过调用STS服务的[AssumeRole](/cn.zh-CN/API参考/API 参考（STS）/操作接口/AssumeRole.md)接口或者使用[各语言STS SDK](/cn.zh-CN/SDK参考/SDK参考（STS）/STS SDK概览.md)来获取临时访问凭证。临时访问凭证包括临时访问密钥（AccessKeyId和AccessKeySecret）和安全令牌（SecurityToken）。 |

-   使用SDK生成URL签名

    URL中添加签名的Python示例代码：

    ```
    import base64
    import hmac
    import hashlib
    import urllib
    h = hmac.new("OtxrzxIsfpFjA7SwPzILwy8Bw21TLhquhboDYROV",
                 "GET\n\n\n1141889120\n/oss-example/oss-api.pdf",
                 hashlib.sha1)
    urllib.quote (base64.encodestring(h.digest()).strip())
    ```

    OSS SDK的URL签名实现，请参见下表。

    |SDK|URL签名方法|实现文件|
    |:--|:------|:---|
    |Java SDK|OSSClient.generatePresignedUrl|[OSSClient.java](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/main/java/com/aliyun/oss/OSSClient.java?spm=a2c4g.11186623.2.6.30uUQV&file=OSSClient.java)|
    |Python SDK|Bucket.sign\_url|[api.py](https://github.com/aliyun/aliyun-oss-python-sdk/blob/master/oss2/api.py?spm=a2c4g.11186623.2.7.30uUQV&file=api.py)|
    |.Net SDK|OssClient.GeneratePresignedUri|[OssClient.cs](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/sdk/OssClient.cs?spm=a2c4g.11186623.2.8.30uUQV&file=OssClient.cs)|
    |PHP SDK|OssClient.signUrl|[OssClient.php](https://github.com/aliyun/aliyun-oss-php-sdk/blob/master/src/OSS/OssClient.php?spm=a2c4g.11186623.2.9.30uUQV)|
    |JavaScript SDK|signatureUrl|[Object.js](https://github.com/ali-sdk/ali-oss/blob/master/lib/object.js?spm=a2c4g.11186623.2.10.30uUQV&file=object.js)|
    |C SDK|oss\_gen\_signed\_url|[oss\_object.c](https://github.com/aliyun/aliyun-oss-c-sdk/blob/master/oss_c_sdk/oss_object.c?spm=a2c4g.11186623.2.11.30uUQV&file=oss_object.c)|
    |C++ SDK|OssClient::GeneratePresignedUrl|[OssClient.cc](https://github.com/aliyun/aliyun-oss-cpp-sdk/blob/master/sdk/src/OssClient.cc)|


## SDK

通过签名URL上传或下载文件的各语言SDK如下：

-   [Java](/cn.zh-CN/SDK 示例/Java/授权访问.md)
-   [Python](/cn.zh-CN/SDK 示例/Python/授权访问.md)
-   [PHP](/cn.zh-CN/SDK 示例/PHP/授权访问.md)
-   [Go](/cn.zh-CN/SDK 示例/Go/授权访问.md)
-   [C](/cn.zh-CN/SDK 示例/C/授权访问.md)
-   [C++](/cn.zh-CN/SDK 示例/C++/授权访问.md)
-   [.NET](/cn.zh-CN/SDK 示例/.NET/授权访问.md)
-   [Node.js](/cn.zh-CN/SDK 示例/Node.js/授权访问.md)
-   [Browser.js](/cn.zh-CN/SDK 示例/Browser.js/授权访问.md)
-   [Android](/cn.zh-CN/SDK 示例/Android/授权访问.md)
-   [iOS](/cn.zh-CN/SDK 示例/iOS/授权访问.md)

## 错误码

|错误码|返回消息|描述|
|---|----|--|
|AccessDenied|403 Forbidden|在URL中添加签名时，Signature、Expires和OSSAccessKeyId顺序可以调换，但缺少Signature、Expires或OSSAccessKeyId中的一个或者多个。|
|AccessDenied|403 Forbidden|访问的当前时间晚于请求中设定的Expires时间或时间格式错误。|
|InvalidArgument|400 Bad Request|URL中包含Signature、Expires、OSSAccessKeyId中的一个或者多个，并且Header中也包含签名消息。|

