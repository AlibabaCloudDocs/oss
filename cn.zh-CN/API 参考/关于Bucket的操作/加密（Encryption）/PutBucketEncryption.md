# PutBucketEncryption

PutBucketEncryption接口用于配置存储空间（Bucket）的加密规则。

**说明：** 只有Bucket的拥有者及授权的RAM用户才能为Bucket设置加密规则，否则返回403错误。有关Bucket加密的更多信息，请参见[服务器端加密](/cn.zh-CN/开发指南/数据安全/数据加密/服务器端加密.md)。

## 请求语法

```
PUT /?encryption HTTP/1.1
Date: GMT Date
Content-Length：ContentLength
Content-Type: application/xml
Host: BucketName.oss.aliyuncs.com
Authorization: SignatureValue
<?xml version="1.0" encoding="UTF-8"?>
<ServerSideEncryptionRule>
  <ApplyServerSideEncryptionByDefault>
    <SSEAlgorithm>AES256</SSEAlgorithm>
    <KMSMasterKeyID></KMSMasterKeyID>
  </ApplyServerSideEncryptionByDefault>
</ServerSideEncryptionRule>
```

## 请求头

此接口涉及的公共请求头，例如Date、Host等。更多信息，请参见[公共请求头（Common Request Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 请求元素

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|ServerSideEncryptionRule|容器|是|不涉及|服务器端加密规则的容器。 子元素：ApplyServerSideEncryptionByDefault |
|ApplyServerSideEncryptionByDefault|容器|是|不涉及|服务器端默认加密方式的容器。 子元素：SSEAlgorithm，KMSMasterKeyID |
|SSEAlgorithm|字符串|是|AES256|设置服务器端默认加密方式。 取值：KMS、AES256、SM4

使用KMS密钥功能时会产生少量的KMS密钥API调用费用，费用详情请参见[KMS计费标准](/cn.zh-CN/产品定价/计费说明.md)。

进行跨区域复制时，若目标Bucket启用了默认服务器端加密方式，且复制规则配置了ReplicaCMKID，有以下两种情况：

-   若源Bucket中的对象未加密，则使用目标Bucket的默认加密方式对跨区域复制过来的明文对象进行加密。
-   若源Bucket中的对象使用了SSE-KMS或SSE-OSS的加密方式，则目标Bucket针对这些对象仍然使用原加密方式进行加密。

更多信息，请参见[跨区域复制结合服务器端加密](/cn.zh-CN/开发指南/数据安全/数据容灾/特殊场景下的复制行为.md)。 |
|KMSDataEncryption|字符串|否|SM4|指定Object的加密算法。若未指定此选项，表明Object使用AES256加密算法。此选项仅当SSEAlgorithm取值为KMS有效。取值：SM4 |
|KMSMasterKeyID|字符串|否|9468da86-3509-4f8d-a61e-6eab1eac\*\*\*\*|当SSEAlgorithm值为KMS，且使用指定的密钥加密时，需输入KMSMasterKeyID。其他情况下，必须为空。|

## 响应头

此接口仅涉及公共响应头。更多信息，请参见[公共响应头（Common Response Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 示例

-   请求示例
    -   设置SSE-KMS加密

        以下示例用于对名为oss-example的Bucket设置SSE-KMS加密。

        ```
        PUT /?encryption HTTP/1.1
        Date: Thur, 5 Nov 2020 11:09:13 GMT
        Content-Length：ContentLength
        Content-Type: application/xml
        Host: oss-example.oss-cn-hangzhou.aliyuncs.com
        Authorization: OSS qn6qrrqxo2oawuk53otf****:ceOEyZavKY4QcjoUWYSpYbJ3****
        <?xml version="1.0" encoding="UTF-8"?>
        <ServerSideEncryptionRule>
          <ApplyServerSideEncryptionByDefault>
            <SSEAlgorithm>KMS</SSEAlgorithm>
            <KMSMasterKeyID>9468da86-3509-4f8d-a61e-6eab1eac****</KMSMasterKeyID>
          </ApplyServerSideEncryptionByDefault>
        </ServerSideEncryptionRule>
        ```

    -   设置SM4加密

        以下示例用于对名为oss-example的Bucket设置SM4加密。

        ```
        PUT /?encryption HTTP/1.1
        Date: Thur, 5 Nov 2020 11:09:13 GMT
        Content-Length：ContentLength
        Content-Type: application/xml
        Host: oss-example.oss-cn-hangzhou.aliyuncs.com
        Authorization: OSS qn6qrrqxo2oawuk53otf****:ceOEyZavKY4QcjoUWYSpYbJ3****
        <?xml version="1.0" encoding="UTF-8"?>
        <ServerSideEncryptionRule>
          <ApplyServerSideEncryptionByDefault>
            <SSEAlgorithm>SM4</SSEAlgorithm>
            <KMSMasterKeyID></KMSMasterKeyID>
          </ApplyServerSideEncryptionByDefault>
        </ServerSideEncryptionRule>
        ```

        您还可以在上传Object时，通过在请求头中携带`x-oss-server-side-encryption`并指定其值为SM4，表明该Object使用SM4加密算法。

    -   设置KMS-SM4加密

        以下示例用于对名为oss-example的Bucket设置KMS-SM4加密。

        ```
        PUT /?encryption HTTP/1.1
        Date: Thur, 5 Nov 2020 11:09:13 GMT
        Content-Length：ContentLength
        Content-Type: application/xml
        Host: oss-example.oss-cn-hangzhou.aliyuncs.com
        Authorization: OSS qn6qrrqxo2oawuk53otf****:ceOEyZavKY4QcjoUWYSpYbJ3****
        <?xml version="1.0" encoding="UTF-8"?>
        <ServerSideEncryptionRule>
          <ApplyServerSideEncryptionByDefault>
            <SSEAlgorithm>KMS</SSEAlgorithm>
            <KMSDataEncryption>SM4</KMSDataEncryption>
            <KMSMasterKeyID></KMSMasterKeyID>
          </ApplyServerSideEncryptionByDefault>
        </ServerSideEncryptionRule>
        ```

        您还可以在上传Object时，通过在请求头中携带`x-oss-server-side-encryption`并指定其值为KMS，`x-oss-server-side-data-encryption`指定其值为SM4，表明该Object使用KMS-SM4加密算法。

-   返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 5C1B138A109F4E405B2D****
    Date: Thur, 5 Nov 2020 11:09:13 GMT
    ```


## SDK

PutBucketEncryption接口对应的各语言SDK如下：

-   [Java](/cn.zh-CN/SDK 示例/Java/数据加密/服务器端加密.md)
-   [Python](/cn.zh-CN/SDK 示例/Python/数据加密/服务器端加密.md)
-   [Go](/cn.zh-CN/SDK 示例/Go/数据加密/服务器端加密.md)
-   [C++](/cn.zh-CN/SDK 示例/C++/数据加密/服务器端加密.md)
-   [.NET](/cn.zh-CN/SDK 示例/.NET/服务器端加密.md)
-   [Node.js](/cn.zh-CN/SDK 示例/Node.js/服务器端加密.md)

## 错误码

|错误码|HTTP状态码|说明|
|---|-------|--|
|InvalidEncryptionAlgorithmError|400|当SSEAlgorithm取值不为KMS或者AES256时，则返回此错误，错误提示为`The Encryption request you specified is not valid. Supported value: AES256/KMS`。|
|InvalidArgument|400|当SSEAlgorithm取值为AES256，但填写了KMSMasterKeyID，则返回此错误，错误提示为`KMSMasterKeyID is not applicable if the default sse algorithm is not KMS`。|

