# 在Header中包含签名 {#concept_aml_vv2_xdb .concept}

用户可以在HTTP请求中增加 `Authorization` 的Header来包含签名（Signature）信息，表明这个消息已被授权。

## SDK 签名实现 {#section_dbw_1bf_xdb .section}

OSS SDK已经实现签名，用户使用OSS SDK不需要关注签名问题。如果您想了解具体语言的签名实现，请参考OSS SDK的代码。OSS SDK签名实现的文件如下表：

|SDK|签名实现|
|:--|:---|
|Java SDK|[OSSRequestSigner.java](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/main/java/com/aliyun/oss/internal/OSSRequestSigner.java)|
|Python SDK|[auth.py](https://github.com/aliyun/aliyun-oss-python-sdk/blob/master/oss2/auth.py)|
|.Net SDK|[OssRequestSigner.cs](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/sdk/Util/OssRequestSigner.cs)|
|PHP SDK|[OssClient.php](https://github.com/aliyun/aliyun-oss-php-sdk/blob/master/src/OSS/OssClient.php)|
|C SDK|[oss\_auth.c](https://github.com/aliyun/aliyun-oss-c-sdk/blob/master/oss_c_sdk/oss_auth.c)|
|JavaScript SDK|[client.js](https://github.com/ali-sdk/ali-oss/blob/master/lib/client.js)|
|Go SDK|[auth.go](https://github.com/aliyun/aliyun-oss-go-sdk/blob/master/oss/auth.go)|
|Ruby SDK|[util.rb](https://github.com/aliyun/aliyun-oss-ruby-sdk/blob/master/lib/aliyun/oss/util.rb)|
|iOS SDK|[OSSModel.m](https://github.com/aliyun/aliyun-oss-ios-sdk/blob/master/AliyunOSSSDK/OSSModel.m)|
|Android SDK|[OSSUtils.java](https://github.com/aliyun/aliyun-oss-android-sdk/blob/master/oss-android-sdk/src/main/java/com/alibaba/sdk/android/oss/common/utils/OSSUtils.java)|

## Authorization字段计算的方法 {#section_w3s_bw2_xdb .section}

```
Authorization = "OSS " + AccessKeyId + ":" + Signature
Signature = base64(hmac-sha1(AccessKeySecret,
            VERB + "\n"
            + Content-MD5 + "\n" 
            + Content-Type + "\n" 
            + Date + "\n" 
            + CanonicalizedOSSHeaders
            + CanonicalizedResource))
```

-   `AccessKeySecret` 表示签名所需的密钥。
-   `VERB`表示HTTP 请求的Method，主要有PUT、GET、POST、HEAD、DELETE等。
-   `\n` 表示换行符。
-   `Content-MD5` 表示请求内容数据的MD5值，对消息内容（不包括头部）计算MD5值获得128比特位数字，对该数字进行base64编码而得到。该请求头可用于消息合法性的检查（消息内容是否与发送时一致），如”eB5eJF1ptWaXm4bijSPyxw==”，也可以为空。详情请参见[RFC2616 Content-MD5](https://www.ietf.org/rfc/rfc2616.txt)。
-   `Content-Type` 表示请求内容的类型，如”application/octet-stream”，也可以为空。
-   `Date` 表示此次操作的时间，且必须为GMT格式，如”Sun, 22 Nov 2015 08:16:38 GMT”。
-   `CanonicalizedOSSHeaders` 表示以 x-oss- 为前缀的HTTP Header的字典序排列。
-   `CanonicalizedResource` 表示用户想要访问的OSS资源。

其中，Date和CanonicalizedResource不能为空；如果请求中的Date时间和OSS服务器的时间差15分钟以上，OSS服务器将拒绝该服务，并返回HTTP 403错误。

## 构建CanonicalizedOSSHeaders的方法 {#section_w2k_sw2_xdb .section}

所有以 x-oss- 为前缀的HTTP Header被称为CanonicalizedOSSHeaders。它的构建方法如下：

1.  将所有以 x-oss- 为前缀的HTTP请求头的名字转换成小写 。如`X-OSS-Meta-Name: TaoBao`转换成`x-oss-meta-name: TaoBao`。
2.  如果请求是以STS获得的AccessKeyId和AccessKeySecret发送时，还需要将获得的security-token值以 `security-token:SecurityToken` 的形式加入到签名字符串中。
3.  将上一步得到的所有HTTP请求头按照名字的字典序进行升序排列。
4.  删除请求头和内容之间分隔符两端出现的任何空格。如`x-oss-meta-name: TaoBao`转换成：`x-oss-meta-name:TaoBao`。
5.  将每一个头和内容用 `\n` 分隔符分隔拼成最后的CanonicalizedOSSHeaders。

**说明：** 

-   CanonicalizedOSSHeaders可以为空，无需添加最后的 `\n`。
-   如果只有一个，则如 `x-oss-meta-a\n`，注意最后的`\n`。
-   如果有多个，则如 `x-oss-meta-a:a\nx-oss-meta-b:b\nx-oss-meta-c:c\n`，注意最后的`\n`。

## 构建CanonicalizedResource的方法 {#section_rvv_dx2_xdb .section}

用户发送请求中想访问的OSS目标资源被称为CanonicalizedResource。它的构建方法如下：

1.  将CanonicalizedResource置成空字符串 `""`。
2.  放入要访问的OSS资源 `/BucketName/ObjectName`（如果没有ObjectName则CanonicalizedResource为”/BucketName/“，如果同时也没有BucketName则为“/”）。
3.  如果请求的资源包括子资源\(SubResource\) ，那么将所有的子资源按照字典序，从小到大排列并以 `&` 为分隔符生成子资源字符串。在CanonicalizedResource字符串尾添加 `？`和子资源字符串。此时的CanonicalizedResource如：`/BucketName/ObjectName?acl&uploadId=UploadId`。

**说明：** 

-   OSS目前支持的子资源\(sub-resource\)包括：acl，uploads，location，cors，logging，website，referer，lifecycle，delete，append，tagging，objectMeta，uploadId，partNumber，security-token，position，img，style，styleName，replication，replicationProgress，replicationLocation，cname，bucketInfo，comp，qos，live，status，vod，startTime，endTime，symlink，x-oss-process，response-content-type，response-content-language，response-expires，response-cache-control，response-content-disposition，response-content-encoding等。
-   子资源\(sub-resource\)有三种类型：
    -   资源标识，如子资源中的acl、append、uploadId、symlink等，详见[关于Bucket的操作](intl.zh-CN/API 参考/关于Bucket的操作/PutBucket.md#)和[关于Object的操作](intl.zh-CN/API 参考/关于Object操作/PutObject.md#)。
    -   指定返回Header字段，如 `response-***`，详见[GetObject](intl.zh-CN/API 参考/关于Object操作/GetObject.md#)的`Request Parameters`。
    -   文件（Object）处理方式，如 `x-oss-process`，用于文件的处理方式，如[图片处理](../../../../../intl.zh-CN/数据处理/图片处理指南/图片处理访问规则.md#)。

## 计算签名头规则 {#section_qcb_p1f_xdb .section}

-   签名的字符串必须为 `UTF-8` 格式。含有中文字符的签名字符串必须先进行 `UTF-8` 编码，再与 `AccessKeySecret`计算最终签名。
-   签名的方法用[RFC 2104](http://www.ietf.org/rfc/rfc2104.txt)中定义的HMAC-SHA1方法，其中Key为 AccessKeySecret。
-   `Content-Type` 和 `Content-MD5` 在请求中不是必须的，但是如果请求需要签名验证，空值的话以换行符 `\n` 代替。
-   在所有非HTTP标准定义的header中，只有以 `x-oss-` 开头的header，需要加入签名字符串；其他非HTTP标准header将被OSS忽略（如下方签名示例中的x-oss-magic是需要加入签名字符串的）。
-   以 `x-oss-` 开头的header在签名验证前需要符合以下规范：
    -   header的名字需要变成小写。
    -   header按字典序自小到大排序。
    -   分割header name和value的冒号前后不能有空格。
    -   每个Header之后都有一个换行符“\\n”，如果没有Header，CanonicalizedOSSHeaders就设置为空。

## 签名示例 {#section_qxg_s1f_xdb .section}

|请求|签名字符串计算公式|签名字符串|
|:-|:--------|:----|
|PUT /nelson HTTP/1.0 Content-MD5: eB5eJF1ptWaXm4bijSPyxw== Content-Type: text/html Date: Thu, 17 Nov 2005 18:49:58 GMT Host: oss-example.oss-cn-hangzhou.aliyuncs.com X-OSS-Meta-Author: foo@bar.com X-OSS-Magic: abracadabra|Signature = base64\(hmac-sha1\(AccessKeySecret,VERB + “\\n” + Content-MD5 + “\\n”+ Content-Type + “\\n” + Date + “\\n” + CanonicalizedOSSHeaders+ CanonicalizedResource\)\)|“PUT\\n eB5eJF1ptWaXm4bijSPyxw==\\n text/html\\n Thu, 17 Nov 2005 18:49:58 GMT\\n x-oss-magic:abracadabra\\nx-oss-meta-author:foo@bar.com\\n/oss-example/nels|

假如AccessKeyId是“44CF959\*\*\*\*\*\*252F707”，AccessKeySecret是“OtxrzxIsfpFjA7Sw\*\*\*\*\*\*8Bw21TLhquhboDYROV”，可用以下方法计算签名\(Signature\)：

python示例代码：

```
import base64
import hmac
import sha
h = hmac.new("OtxrzxIsfpFjA7Sw******8Bw21TLhquhboDYROV",
             "PUT\nODBGOERFMDMzQTczRUY3NUE3NzA5QzdFNUYzMDQxNEM=\ntext/html\nThu, 17 Nov 2005 18:49:58 GMT\nx-oss-magic:abracadabra\nx-oss-meta-author:foo@bar.com\n/oss-example/nelson", sha)
Signature = base64.b64encode(h.digest())
print("Signature: %s" % Signature)
```

签名\(Signature\)计算结果应该为 26NBxoKd\*\*\*\*\*\*Dv6inkoDft/yA=，因为Authorization = “OSS”+ AccessKeyId + “:” + Signature，所以最后Authorization为 “OSS 44CF95900\*\*\*BF252F707:26NBxoKd\*\*\*\*\*\*Dv6inkoDft/yA=”，然后加上Authorization头来组成最后需要发送的消息：

```
PUT /nelson HTTP/1.0
Authorization:OSS 44CF95900***BF252F707:26NBxoKd******Dv6inkoDft/yA=
Content-Md5: eB5eJF1ptWaXm4bijSPyxw==
Content-Type: text/html
Date: Thu, 17 Nov 2005 18:49:58 GMT
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
X-OSS-Meta-Author: foo@bar.com
X-OSS-Magic: abracadabra
```

细节分析如下：

-   如果传入的AccessKeyId不存在或inactive，返回403 Forbidden。错误码：InvalidAccessKeyId。
-   若用户请求头中Authorization值的格式不对，返回400 Bad Request。错误码：InvalidArgument。
-   OSS所有的请求都必须使用HTTP 1.1协议规定的GMT时间格式。其中，日期的格式为：

    ```
    date1 = 2DIGIT SP month SP 4DIGIT; day month year (e.g., 02 Jun
            1982)
    ```

    上述日期格式中，“天”所占位数都是“2 DIGIT”。因此，“Jun 2”、“2 Jun 1982”和“2-Jun-82”都是非法日期格式。

-   如果签名验证的时候，头中没有传入Date或者格式不正确，返回403 Forbidden错误。错误码：AccessDenied。
-   传入请求的时间必须在OSS服务器当前时间之后的15分钟以内，否则返回403 Forbidden。错误码：RequestTimeTooSkewed。
-   如果AccessKeyId是active的，但OSS判断用户的请求发生签名错误，则返回403 Forbidden，并在返回给用户的response中告诉用户正确的用于验证加密的签名字符串。用户可以根据OSS的response来检查自己的签名字符串是否正确。

    返回示例：

    ```
    <?xml version="1.0" ?>
    <Error>
     <Code>
         SignatureDoesNotMatch
     </Code>
     <Message>
         The request signature we calculated does not match the signature you provided. Check your key and signing method.
     </Message>
     <StringToSignBytes>
         47 45 54 0a 0a 0a 57 65 64 2c 20 31 31 20 4d 61 79 20 32 30 31 31 20 30 37 3a 35 39 3a 32 35 20 47 4d 54 0a 2f 75 73 72 65 61 6c 74 65 73 74 3f 61 63 6c
     </StringToSignBytes>
     <RequestId>
         1E446260FF9B10C2
     </RequestId>
     <HostId>
         oss-cn-hangzhou.aliyuncs.com
     </HostId>
     <SignatureProvided>
         y5H7yzPsA/tP4+0tH1HHvPEwUv8=
     </SignatureProvided>
     <StringToSign>
         GET
    Wed, 11 May 2011 07:59:25 GMT
    /oss-example?acl
     </StringToSign>
     <OSSAccessKeyId>
         AKIAIVAKMSMOY7VOMRWQ
     </OSSAccessKeyId>
    </Error>
    ```


## 常见问题 {#section_vkz_sbf_xdb .section}

Content-MD5的计算方法

```
Content-MD5的计算
以消息内容为"123456789"来说，计算这个字符串的Content-MD5
正确的计算方式：
标准中定义的算法简单点说就是：
1. 先计算MD5加密的二进制数组（128位）。
2. 再对这个二进制进行base64编码（而不是对32位字符串编码）。 
以Python为例子：
正确计算的代码为：
>>> import base64,hashlib
>>> hash = hashlib.md5()
>>> hash.update("0123456789")
>>> base64.b64encode(hash.digest())
'eB5eJF1ptWaXm4bijSPyxw=='
需要注意
正确的是：hash.digest()，计算出进制数组（128位）
>>> hash.digest()
'x\x1e^$]i\xb5f\x97\x9b\x86\xe2\x8d#\xf2\xc7'
常见错误是直接对计算出的32位字符串编码进行base64编码。
例如，错误的是：hash.hexdigest()，计算得到可见的32位字符串编码
>>> hash.hexdigest()
'781e5e245d69b566979b86e28d23f2c7'
错误的MD5值进行base64编码后的结果：
>>> base64.b64encode(hash.hexdigest())
'NzgxZTVlMjQ1ZDY5YjU2Njk3OWI4NmUyOGQyM2YyYzc='
```

