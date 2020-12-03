# GetLiveChannelInfo

GetLiveChannelInfo接口用于获取指定LiveChannel的配置信息。

## 请求语法

```
GET /ChannelName?live HTTP/1.1
Date: GMT date
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Authorization: SignatureValue
```

## 请求头

此接口仅涉及公共请求头。更多信息，请参见[公共请求头（Common Request Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 响应头

此接口仅涉及公共响应头。更多信息，请参见[公共响应头（Common Response Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 响应元素

|名称|类型|示例值|描述|
|:-|:-|---|:-|
|LiveChannelConfiguration|容器|不涉及|保存GetLiveChannelInfo返回结果的容器。 子节点：Description、Status、Target

父节点：无 |
|Description|字符串|test|LiveChannel的描述信息。 子节点：无

父节点：LiveChannelConfiguration |
|Status|枚举字符串|enabled|LiveChannel的状态信息。 子节点：无

父节点：LiveChannelConfiguration

有效值：

-   enabled：启用状态
-   disabled：禁用状态 |
|Target|容器|不涉及|保存LiveChannel转储配置的容器。 子节点：Type、FragDuration、FragCount、PlaylistName

**说明：** FragDuration、FragCount、PlaylistName只有当Type取值为HLS时才会返回。

父节点：LiveChannelConfiguration |
|Type|枚举字符串|HLS|当Type为HLS时，指定推流时转储文件类型。 子节点：无

父节点：Target

有效值：HLS |
|FragDuration|字符串|2|当Type为HLS时，指定每个ts文件的时长。 单位：秒

子节点：无

父节点：Target |
|FragCount|字符串|3|当Type为HLS时，指定m3u8文件中包含ts文件的个数。 子节点：无

父节点：Target |
|PlaylistName|字符串|playlist.m3u8|当Type为HLS时，指定生成的m3u8文件的名称。 子节点：无

父节点：Target |

## 示例

请求示例

```
GET /test-channel?live HTTP/1.1
Date: Thu, 25 Aug 2016 05:52:40 GMT
Host: test-bucket.oss-cn-hangzhou.aliyuncs.com
Authorization: OSS YJjHKOKWDWIN****:D6bDCRXKht58hin1BL83wxyG****
```

返回示例

```
HTTP/1.1 200
content-length: 475
server: AliyunOSS
connection: close
x-oss-request-id: 57BE87A8B92475920B00****
date: Thu, 25 Aug 2016 05:52:40 GMT
content-type: application/xml
<?xml version="1.0" encoding="UTF-8"?>
<LiveChannelConfiguration>
  <Description></Description>
  <Status>enabled</Status>
  <Target>
    <Type>HLS</Type>
    <FragDuration>2</FragDuration>
    <FragCount>3</FragCount>
    <PlaylistName>playlist.m3u8</PlaylistName>
  </Target>
</LiveChannelConfiguration>
```

## SDK

[Java](/cn.zh-CN/用户实践/Java SDK 的 LiveChannel 常见操作.md)

