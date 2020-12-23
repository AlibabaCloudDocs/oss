# PutLiveChannelStatus

LiveChannel分为启用（enabled）和禁用（disabled）两种状态。您可以使用PutLiveChannelStatus接口在两种状态之间进行切换。

## 注意事项

LiveChannel有如下注意事项：

-   LiveChannel处于disabled状态时，OSS会禁止您向该LiveChannel进行推流操作。如果您正在向该LiveChannel推流，那么推流的客户端会被强制断开（会有10s左右的延迟）。
-   当没有客户端向该LiveChannel推流时，调用PutLiveChannelStatus重新创建LiveChannel也可以达到修改Status的目的。
-   当有客户端向该LiveChannel推流时，只能将LiveChannel的状态修改为disabled，无法调用PutLiveChannelStatus重新创建LiveChannel。

## 请求语法

```
PUT /ChannelName?live&status=NewStatus HTTP/1.1
Date: Tue, 25 Dec 2018 17:35:24 GMT
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Authorization: SignatureValue
```

## 请求头

|名称|类型|是否必选|描述|
|:-|--|----|--|
|NewStatus|字符串|是|设置LiveChannel的Status。 有效值：

-   enabled：启用LiveChannel
-   disabled：禁用LiveChannel |

有关此接口涉及的其他公共请求头，请参见[公共请求头（Common Request Headers）](/intl.zh-CN/API 参考/公共HTTP头定义.md)。

## 响应头

此接口仅涉及公共响应头，详情请参见[公共响应头（Common Response Headers）](/intl.zh-CN/API 参考/公共HTTP头定义.md)。

## 示例

请求示例

```
PUT /test-channel?live&status=disabled HTTP/1.1
Date: Tue, 25 Dec 2018 17:35:24 GMT
Host: test-bucket.oss-cn-hangzhou.aliyuncs.com
Authorization: OSS YJjHKOKWDWIN****:X/mBrSbkNoqM/JoAfRC0ytyQ****
```

返回示例

```
HTTP/1.1 200
Content-Length: 0
Server: AliyunOSS
Connection: close
x-oss-request-id: 57BE8422B92475920B00****
Date: Tue, 25 Dec 2018 17:35:24 GMT
```

