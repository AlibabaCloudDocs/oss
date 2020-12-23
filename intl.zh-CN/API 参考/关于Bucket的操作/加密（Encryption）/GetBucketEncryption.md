# GetBucketEncryption

GetBucketEncryption接口用于获取存储空间（Bucket）的加密规则。

**说明：** 只有Bucket的拥有者及授权的RAM用户才能获取Bucket的加密规则，否则返回403错误。有关Bucket加密的更多信息，请参见[服务器端加密](/intl.zh-CN/开发指南/数据安全/数据加密/服务器端加密.md)。

## 请求语法

```
Get /?encryption HTTP/1.1
Date: GMT Date
Host: BucketName.oss.aliyuncs.com
Authorization: SignatureValue
```

## 请求头

此接口仅涉及公共请求头。更多信息，请参见[公共请求头（Common Request Headers）](/intl.zh-CN/API 参考/公共HTTP头定义.md)。

## 响应头

此接口仅涉及公共响应头。更多信息，请参见[公共响应头（Common Response Headers）](/intl.zh-CN/API 参考/公共HTTP头定义.md)。

## 响应元素

|名称|类型|示例值|描述|
|--|--|---|--|
|ServerSideEncryptionRule|容器|不涉及|服务端加密规则的容器。 子元素：ApplyServerSideEncryptionByDefault |
|ApplyServerSideEncryptionByDefault|容器|不涉及|服务端默认加密方式的容器。 子元素：SSEAlgorithm，KMSMasterKeyID |
|SSEAlgorithm|字符串|KMS|显示服务端默认加密方式。 取值：KMS、AES256 |
|KMSMasterKeyID|字符串|9468da86-3509-4f8d-a61e-6eab1eac\*\*\*\*|显示当前使用的KMS密钥ID。 仅当SSEAlgorithm为KMS且指定了密钥ID时返回，其他情况下，此项为空。 |

## 示例

-   请求示例

    ```
    Get /?encryption HTTP/1.1
    Date: Tue, 20 Dec 2018 11:20:10 GMT
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Authorization: OSS qn6qrrqxo2oawuk53otf****:ceOEyZavKY4QcjoUWYSpYbJ3****
    ```

-   返回示例

    以下返回示例表明Bucket设置了SSE-KMS加密。

    ```
    HTTP/1.1 204 NoContent
    x-oss-request-id: 5C1B138A109F4E405B2D8AEF
    Date: Tue, 20 Dec 2018 11:22:05 GMT
    <?xml version="1.0" encoding="UTF-8"?>
    <ServerSideEncryptionRule>
      <ApplyServerSideEncryptionByDefault>
        <SSEAlgorithm>KMS</SSEAlgorithm>
        <KMSMasterKeyID>9468da86-3509-4f8d-a61e-6eab1eac****</KMSMasterKeyID>
      </ApplyServerSideEncryptionByDefault>
    </ServerSideEncryptionRule>
    ```


## SDK

GetBucketEncryption接口对应的各语言SDK如下：

-   [Java](/intl.zh-CN/SDK 示例/Java/数据加密/服务器端加密.md)
-   [Python](/intl.zh-CN/SDK 示例/Python/数据加密/服务器端加密.md)
-   [Go](/intl.zh-CN/SDK 示例/Go/数据加密/服务器端加密.md)
-   [C++](/intl.zh-CN/SDK 示例/C++/数据加密/服务器端加密.md)
-   [PHP](/intl.zh-CN/SDK 示例/PHP/数据加密/服务器端加密.md)
-   [.NET](/intl.zh-CN/SDK 示例/.NET/服务器端加密.md)
-   [Node.js](/intl.zh-CN/SDK 示例/Node.js/服务器端加密.md)

## 错误码

|错误码|HTTP状态码|说明|
|---|-------|--|
|AccessDenied|403|无获取Bucket加密规则的权限。|
|NoSuchBucket|400|指定获取加密规则对应的Bucket不存在。|
|NoSuchServerSideEncryptionRule|400|Bucket未设置加密规则。|

