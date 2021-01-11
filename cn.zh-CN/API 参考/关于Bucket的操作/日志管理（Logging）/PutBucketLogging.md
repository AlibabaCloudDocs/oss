# PutBucketLogging

PutBucketLogging接口用于为存储空间（Bucket）开启日志转存功能，可将OSS的访问日志按照固定命名规则，以小时为单位生成日志文件写入您指定的Bucket。

## 注意事项

-   生成日志的源Bucket和存储日志的目标Bucket可以相同也可以不同，但是必须属于同一账号下的相同地域。
-   日志文件以小时为单位生成，但并不表示某个时段的日志文件记录了该时段的所有请求，部分请求可能会出现在上一时段或下一时段的日志文件中。

    日志文件命名规则及日志格式说明，请参见[日志转存](/cn.zh-CN/开发指南/日志管理/访问日志存储.md)。

-   在您关闭日志转存功能前，OSS的日志文件会一直生成。请及时清理不再需要的日志文件，以减少您的存储费用。

    您可以通过生命周期规则定期删除日志文件。更多信息，请参见[生命周期规则介绍](/cn.zh-CN/开发指南/对象/文件（Object）/文件生命周期/生命周期规则介绍.md)。

-   OSS会根据需求在日志的尾部添加一些字段，请您在开发日志处理工具时考虑兼容性的问题。

## 请求语法

```
PUT /?logging HTTP/1.1
Date: GMT Date
Content-Length：ContentLength
Content-Type: application/xml
Authorization: SignatureValue 
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
<?xml version="1.0" encoding="UTF-8"?>
<BucketLoggingStatus>
    <LoggingEnabled>
        <TargetBucket>TargetBucket</TargetBucket>
        <TargetPrefix>TargetPrefix</TargetPrefix>
    </LoggingEnabled>
</BucketLoggingStatus>
```

## 请求头

此接口仅涉及公共请求头。更多信息，请参见[公共请求头（Common Request Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 请求元素

|名称|类型|是否必需|示例值|描述|
|--|--|----|---|--|
|BucketLoggingStatus|容器|是|不涉及|存储访问日志状态信息的容器。子元素：LoggingEnabled

父元素：无 |
|LoggingEnabled|容器|开启日志转存时必选|不涉及|访问日志信息的容器。子元素：TargetBucket, TargetPrefix

父元素：BucketLoggingStatus |
|TargetBucket|字符串|开启日志转存时必选|examplebucket|指定存储访问日志的Bucket。子元素：无

父元素：BucketLoggingStatus.LoggingEnabled |
|TargetPrefix|字符串|否|MyLog-|指定保存的日志文件前缀，可以为空。子元素：无

父元素：BucketLoggingStatus.LoggingEnabled |

## 响应头

此接口仅涉及公共响应头。更多信息，请参见[公共响应头（Common Response Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 示例

-   开启Bucket日志转存的请求示例

    ```
    PUT /?logging HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Content-Length: 186
    Date: Fri, 04 May 2012 03:21:12 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:KU5h8YMUC78M30dXqf3JxrTZ****
    <?xml version="1.0" encoding="UTF-8"?>
    <BucketLoggingStatus>
    <LoggingEnabled>
    <TargetBucket>examplebucket</TargetBucket>
    <TargetPrefix>MyLog-</TargetPrefix>
    </LoggingEnabled>
    </BucketLoggingStatus>
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E888648906008B
    Date: Fri, 04 May 2012 03:21:12 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   关闭Bucket日志转存的请求示例

    关闭Bucket的日志转存功能时，只需发送一个空的BucketLoggingStatus即可。示例如下：

    ```
    PUT /?logging HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Content-Type: application/xml
    Content-Length: 86
    Date: Fri, 04 May 2012 04:21:12 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:KU5h8YMUC78M30dXqf3JxrTZ****
    <?xml version="1.0" encoding="UTF-8"?>
    <BucketLoggingStatus>
    </BucketLoggingStatus>
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674125A4D8906008B
    Date: Fri, 04 May 2012 04:21:12 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```


## SDK

此接口所对应的各语言SDK如下：

-   [Java](/cn.zh-CN/SDK 示例/Java/存储空间/访问日志.md)
-   [Python](/cn.zh-CN/SDK 示例/Python/存储空间/访问日志.md)
-   [PHP](/cn.zh-CN/SDK 示例/PHP/存储空间/访问日志.md)
-   [Go](/cn.zh-CN/SDK 示例/Go/存储空间/访问日志.md)
-   [C](/cn.zh-CN/SDK 示例/C/存储空间/访问日志.md)
-   [.NET](/cn.zh-CN/SDK 示例/.NET/存储空间/访问日志.md)
-   [Node.js](/cn.zh-CN/SDK 示例/Node.js/存储空间/访问日志.md)
-   [Ruby](/cn.zh-CN/SDK 示例/Ruby/存储空间/访问日志.md)

## 错误码

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|NoSuchBucket|404|源Bucket不存在。|
|InvalidTargetBucketForLogging|400|源Bucket和目标Bucket不属于同一个数据中心。|
|InvalidDigest|400|上传了Content-MD5请求头后，OSS会计算消息体的Content-MD5并检查一致性，如果不一致会返回此错误码。|
|MalformedXML|400|请求中的XML不合法。|
|InvalidTargetBucketForLogging|403|请求发起者不是目标Bucket的拥有者。|
|AccessDenied|403|请求发起者不是源Bucket的拥有者。|

