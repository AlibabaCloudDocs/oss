# 在URL中包含签名

除了使用Authorization Header，您还可以在URL中加入签名信息，以便将该URL转给第三方实现授权访问。

## 注意事项

-   使用在URL中签名的方式，会将您授权的数据在过期时间内曝露在互联网上，请预先评估使用风险。
-   OSS不支持同时在URL和Header中包含签名。
-   PUT和GET请求都支持在URL中签名。
-   您可以为PUT操作生成一个预签名的URL，该URL检查用户是否上传了正确的内容。SDK对请求进行预签名时，将计算请求正文的校验和，并生成包含在预签名URL中的MD5校验和。用户必须上传与SDK生成的MD5校验和相同的内容，否则操作失败。要验证MD5，只需在请求中增加Content-MD5头。

## 示例代码

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

OSS SDK中提供了URL签名方法，详细请参见[SDK文档](/cn.zh-CN/SDK 示例/简介.md)。

OSS SDK的URL签名实现，请参看下表：

|SDK|URL签名方法|实现文件|
|:--|:------|:---|
|Java SDK|OSSClient.generatePresignedUrl|[OSSClient.java](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/main/java/com/aliyun/oss/OSSClient.java?spm=a2c4g.11186623.2.6.30uUQV&file=OSSClient.java)|
|Python SDK|Bucket.sign\_url|[api.py](https://github.com/aliyun/aliyun-oss-python-sdk/blob/master/oss2/api.py?spm=a2c4g.11186623.2.7.30uUQV&file=api.py)|
|.Net SDK|OssClient.GeneratePresignedUri|[OssClient.cs](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/sdk/OssClient.cs?spm=a2c4g.11186623.2.8.30uUQV&file=OssClient.cs)|
|PHP SDK|OssClient.signUrl|[OssClient.php](https://github.com/aliyun/aliyun-oss-php-sdk/blob/master/src/OSS/OssClient.php?spm=a2c4g.11186623.2.9.30uUQV)|
|JavaScript SDK|signatureUrl|[Object.js](https://github.com/ali-sdk/ali-oss/blob/master/lib/object.js?spm=a2c4g.11186623.2.10.30uUQV&file=object.js)|
|C SDK|oss\_gen\_signed\_url|[oss\_object.c](https://github.com/aliyun/aliyun-oss-c-sdk/blob/master/oss_c_sdk/oss_object.c?spm=a2c4g.11186623.2.11.30uUQV&file=oss_object.c)|
|C++ SDK|OssClient::GeneratePresignedUrl|[OssClient.cc](https://github.com/aliyun/aliyun-oss-cpp-sdk/blob/master/sdk/src/OssClient.cc)|

## 实现方式

URL签名示例：

```
http://oss-example.oss-cn-hangzhou.aliyuncs.com/oss-api.pdf?OSSAccessKeyId=nz2pc56s936**9l&Expires=1141889120&Signature=vjbyPxybdZaNmGa%2ByT272YEAiv4%3D
```

URL签名必须至少包含Signature、Expires和OSSAccessKeyId三个参数。生成签名字符串时，除Date被替换成Expires参数外，仍然包含content-type、content-md5等[在Header中包含签名](/cn.zh-CN/API 参考/访问控制/在Header中包含签名.md)中定义的Header（请求中虽然仍有Date这个请求Header，但不需要将Date加入签名字符串中）。

-   Expires参数的值是一个[Unix time](https://baike.baidu.com/item/unix时间戳/2078227?fr=aladdin)（自UTC时间1970年01月01号开始的秒数），用于标识该URL的超时时间。如果OSS接收到这个URL请求的时间晚于签名中包含的Expires参数时，则返回请求超时的错误码。例如：当前时间是1141889060，开发者希望创建一个60秒后自动失效的URL，则可以设置Expires时间为1141889120。

    **说明：** 出于安全考虑，OSS控制台中默认URL的有效时间为3600秒，最大为32400秒。

-   OSSAccessKeyId即密钥中的AccessKeyId。
-   Signature表示签名信息。所有OSS支持的请求和各种Header参数，在URL中进行签名的算法和[在Header中包含签名](/cn.zh-CN/API 参考/访问控制/在Header中包含签名.md)的算法基本一样。

    ```
    Signature = urlencode(base64(hmac-sha1(AccessKeySecret,
              VERB + "\n" 
              + CONTENT-MD5 + "\n" 
              + CONTENT-TYPE + "\n" 
              + EXPIRES + "\n" 
              + CanonicalizedOSSHeaders
              + CanonicalizedResource)))
    ```

    `CONTENT-MD5`、`CanonicalizedOSSHeaders`、`CONTENT-TYPE`等值的详细说明，请参见[在Header中包含签名](/cn.zh-CN/API 参考/访问控制/在Header中包含签名.md)。

    **说明：** 与Header中包含签名相比主要区别如下：

    -   通过URL包含签名时，Header中包含签名的Date参数替换为Expires参数。
    -   如果多次传入Signature、Expires或OSSAccessKeyId，以第一次为准。
    -   先验证请求时间是否晚于Expires时间，然后再验证签名。
    -   将签名字符串放到URL时，须对URL进行urlencode。
-   临时用户使用URL签名时，需要携带`security-token`，格式如下：

    ```
    http://oss-example.oss-cn-hangzhou.aliyuncs.com/oss-api.pdf?OSSAccessKeyId=nz2pc56s936**9l&Expires=1141889120&Signature=vjbyPxybdZaNmGa%2ByT272YEAiv4%3D&security-token=SecurityToken
    ```


## 错误码

|错误码|返回消息|描述|
|---|----|--|
|AccessDenied|403 Forbidden|在URL中添加签名时，Signature、Expires和OSSAccessKeyId顺序可以调换，但缺少Signature、Expires或OSSAccessKeyId中的一个或者多个。|
|AccessDenied|403 Forbidden|访问的当前时间晚于请求中设定的Expires时间或时间格式错误。|
|InvalidArgument|400 Bad Request|URL中包含Signature、Expires、OSSAccessKeyId中的一个或者多个，并且Header中也包含签名消息。|

