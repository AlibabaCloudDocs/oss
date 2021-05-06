# PutBucketTransferAcceleration

PutBucketTransferAcceleration接口用于为存储空间（Bucket）配置传输加速。开启传输加速后，可提升全球各地用户对OSS的访问速度，适用于远距离数据传输、GB或TB级大文件上传和下载的场景。

## 注意事项

-   只有Bucket拥有者以及被授予oss:PutBucketTransferAcceleration权限的RAM用户才能发起配置传输加速的请求。
-   开启传输加速后，Bucket会在保留默认Endpoint的基础上新增传输加速域名，但必须使用OSS的传输加速域名才会提升访问速度。
-   使用传输加速域名访问Bucket时，OSS会收取传输加速费用。详情请参见[传输加速费用](/cn.zh-CN/计量计费/计量项和计费项/传输加速费用.md)。

有关传输加速的更多信息，请参见开发指南的[传输加速](/cn.zh-CN/开发指南/存储空间（Bucket）/传输加速.md)。

## 请求语法

```
PUT /?transferAcceleration HTTP/1.1
Date: GMT Date
Content-Length：ContentLength
Content-Type: application/xml
Host: BucketName.oss.aliyuncs.com
Authorization: SignatureValue
```

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|TransferAccelerationConfiguration|容器|是|不涉及|传输加速配置的容器。|
|Enabled|字符串|是|true|目标Bucket是否开启传输加速。取值如下：-   true：开启传输加速。
-   false：关闭传输加速。

**说明：** 传输加速开启及关闭操作在30分钟内生效。 |

此接口涉及Authorization、Content-Length等其他公共请求头的更多信息，请参见[公共请求头（Common Request Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 响应头

此接口仅涉及x-oss-request-id、Date等公共响应头。有关公共响应头的更多信息，请参见[公共响应头（Common Response Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 示例

-   请求示例

    为目标存储空间examplebucket开启传输加速的请求示例如下：

    ```
    PUT /?transferAcceleration HTTP/1.1
    Date: Fri , 30 Apr 2021 13:08:38 GMT
    Content-Length：443
    Content-Type: application/xml
    Host: examplebucket.oss.aliyuncs.com
    Authorization: OSS qn6qrrqxo2oawuk53otf****:PYbzsdWAIWAlMW8luk****
    <TransferAccelerationConfiguration>
      <Enabled>true</Enabled>
    </TransferAccelerationConfiguration>
    ```

-   返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674A4D890****
    Date: Fri , 30 Apr 2021 13:08:38 GMT
    Content-Length: 443
    Connection: keep-alive
    Server: AliyunOSS
    ```


## 错误码

|错误码|HTTP状态码|描述|
|---|-------|--|
|AccessDenied|404|没有操作权限。仅支持拥有oss:PutBucketTransferAcceleration权限的用户配置传输加速。|
|MalformedXML|400|请求的XML格式不合法。例如，请求字段<Enabled\>设置为true或者false以外的非法值。|

