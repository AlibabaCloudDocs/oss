# RTMP推流地址及签名

本文介绍RTMP推流地址及其签名规则。

**说明：** 仅当Bucket ACL为非public-read-write时，推流地址需要签名后才可以使用。签名方法与OSS的URL签名类似。

## RTMP推流地址

RTMP推流地址组成规则为`rtmp://${bucket}.${host}/live/${channel}?${params}`，例如`rtmp://examplebucket.oss-cn-hangzhou.aliyuncs.com/live/test-channel`。

-   `bucket`：Bucket名称，例如`examplebucket`。有关Bucket命名规范的更多信息，请参见[存储空间（Bucket）](/intl.zh-CN/开发指南/基本概念.md)。
-   `host`：填写地域节点Endpoint，例如`oss-cn-hangzhou.aliyuncs.com`。有关Endpoint的更多信息，请参见[访问域名和数据中心](/intl.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md)。
-   `live`：RTMP协议的App名称，OSS固定使用live。
-   `channel`：channel名称。例如`test-channel`。有关channel命名规范的更多信息，请参见[PutLiveChannel](/intl.zh-CN/API 参考/关于LiveChannel的操作/PutLiveChannel.md)。
-   `params`：推流参数，与HTTP请求的query string相同，格式为`varA=valueA&varB=valueB`。

## RTMP推流支持的URL参数

RTMP推流支持的URL参数及描述如下表所示。

|名称|描述|
|:-|:-|
|playlistName|指定生成的m3u8文件名称。 **说明：** 生成的m3u8文件名称仍被添加`${channel_name}/`前缀。 |

## 推流地址的签名规则

带签名的推流地址格式为`rtmp://${bucket}.${host}/live/${channel}?OSSAccessKeyId=xxx&Expires=yyy&Signature=zzz&${params}`。

推流地址的签名规则中包含的参数及描述如下表所示。

|参数名称|描述|
|:---|:-|
|OSSAccessKeyId|与OSS HTTP签名的AccessKeyId相同。|
|Expires|Unix时间戳。|
|Signature|签名字符串。|
|params|其他参数。 所有的参数都需要经过签名。|

Signature的计算规则如下。

```
base64(hmac-sha1(AccessKeySecret,
    + Expires + "\n"
    + CanonicalizedParams
    + CanonicalizedResource))
```

Signature计算规则中涉及的参数及描述如下表所示。

|名称|描述|
|:-|:-|
|CanonicalizedParams|按照param key字典序拼接所有参数，格式为`key:value\n`。 **说明：**

-   如果参数个数为0，那此项为空。
-   参数中不包含SecurityToken、OSSAccessKeyId、Expire以及Signature。
-   每个param key只能出现一次。 |
|CanonicalizedResource|格式为`/BucketName/ChannelName`，例如`examplebucket/test-channel`。|

