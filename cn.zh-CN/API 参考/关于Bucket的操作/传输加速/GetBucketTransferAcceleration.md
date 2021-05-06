# GetBucketTransferAcceleration

GetBucketTransferAcceleration接口用于获取目标存储空间（Bucket）的传输加速配置。

## 注意事项

-   只有Bucket拥有者以及被授予oss:GetBucketTransferAcceleration权限的RAM用户才能发起获取传输加速配置的请求。
-   如果Bucket未配置过传输加速，调用该接口时不返回加速配置状态。

有关传输加速的更多信息，请参见开发指南的[传输加速](/cn.zh-CN/开发指南/存储空间（Bucket）/传输加速.md)。

## 请求语法

```
GET /?transferAcceleration HTTP/1.1
Date: GMT Date
Content-Length：ContentLength
Content-Type: application/xml
Host: BucketName.oss.aliyuncs.com
Authorization: SignatureValue
```

## 返回参数

|名称|类型|示例值|描述|
|--|--|---|--|
|TransferAccelerationConfiguration|容器|不涉及|保存传输加速配置信息的容器。|
|Enabled|字符串|true|传输加速状态。取值范围如下：-   true：开启传输加速。
-   false：关闭传输加速。 |

此接口涉及的x-oss-request-id、Date等其他公共响应头的更多信息，请参见[公共响应头（Common Response Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 示例

-   请求示例

    以下请求示例用于获取目标存储空间examplebucket的传输加速状态。

    ```
    GET /?transferAcceleration HTTP/1.1
    Date: Fri , 30 Apr 2021 13:08:38 GMT
    Content-Length：443
    Content-Type: application/xml
    Host: examplebucket.oss.aliyuncs.com
    Authorization: OSS qn6qrrqxo2oawuk53otf****:PYbzsdWAIWAlMW8luk****
    ```

-   返回示例

    以下返回示例表明examplebucket已开启传输加速。

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Fri , 30 Apr 2021 13:08:38 GMT
    <TransferAccelerationConfiguration>
     <Enabled>true</Enabled>
    </TransferAccelerationConfiguration>
    ```


## 错误码

|错误码|HTTP状态码|描述|
|---|-------|--|
|NoSuchTransferAccelerationConfiguration|404|请求的目标Bucket未配置传输加速。|

