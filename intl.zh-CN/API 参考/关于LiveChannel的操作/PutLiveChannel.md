# PutLiveChannel

通过RTMP协议上传音视频数据前，必须先调用该接口创建一个LiveChannel。调用PutLiveChannel接口会返回RTMP推流地址，以及对应的播放地址。

**说明：** 您可以使用返回的地址进行推流、播放，您还可以根据该LiveChannel的名称来发起相关的操作，如查询推流状态、查询推流记录、禁止推流等。

## 请求语法

```
PUT /ChannelName?live HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT date
Content-Length: Size
Authorization: SignatureValue
<?xml version="1.0" encoding="UTF-8"?>
<LiveChannelConfiguration>
  <Description>ChannelDescription</Description>
  <Status>ChannelStatus</Status>
  <Target>
     <Type>HLS</Type>
     <FragDuration>FragDuration</FragDuration>
     <FragCount>FragCount</FragCount>
     <PlaylistName>PlaylistName</PlaylistName>
  </Target>
  <Snapshot>
    <RoleName>Snapshot ram role</RoleName>
    <DestBucket>Snapshot dest bucket</DestBucket>
    <NotifyTopic>Notify topic of MNS</NotifyTopic>
    <Interval>Snapshot interval in second</Interval>
  </Snapshot>
</LiveChannelConfiguration>
```

## 请求头

|名称|类型|是否必选|描述|
|--|--|----|--|
|ChannelName|字符串|是|必须符合ObjectName的命名规范，另外，ChannelName不能包含斜杠（/）。|

## 请求元素

|名称|类型|是否必选|描述|
|:-|:-|----|:-|
|LiveChannelConfiguration|容器|是|保存LiveChannel配置的容器。 子节点：Description、Status、Target

 父节点：无 |
|Description|字符串|否|LiveChannel的描述信息，最长128字节。 子节点：无

 父节点：LiveChannelConfiguration |
|Status|枚举字符串|否|指定LiveChannel的状态。 子节点：无

 父节点：LiveChannelConfiguration

 有效值：enabled、disabled

 默认值：enabled |
|Target|容器|是|保存转储配置的容器。 子节点：Type、FragDuration、FragCount、PlaylistName

 父节点：LiveChannelConfiguration |
|Type|枚举字符串|是|指定转储的类型。 子节点：无

 父节点：Target

 有效值：HLS

 **说明：**

-   转储类型为HLS时，OSS会在生成每个ts文件后更新m3u8文件。m3u8文件中最多包含最近的FragCount个ts文件。
-   转储类型为HLS时，写入当前ts文件的音视频数据时长达到FragDuration指定的时长后，OSS会在收到下一个关键帧的时切换到下一个ts文件；如果max\(2\*FragDuration, 60s\)后仍未收到下一个关键帧，OSS将强制切换文件，此时可能引起播放时卡顿。 |
|FragDuration|字符串|否|当Type为HLS时，指定每个ts文件的时长。 单位：秒

 子节点：无

 父节点：Target

 取值范围：\[1, 100\]

 默认值：5

 **说明：** FragDuration和FragCount的默认值只有在两者都未指定时才会生效；指定了其中一个，则另一个的值也必须指定。 |
|FragCount|字符串|否|当Type为HLS时，指定m3u8文件中包含ts文件的个数。 子节点：无

 父节点：Target

 取值范围：\[1, 100\]

 默认值：3

 **说明：** FragDuration和FragCount的默认值只有在两者都未指定时才会生效；指定了其中一个，则另一个的值也必须指定。 |
|PlaylistName|字符串|否|当Type为HLS时，指定生成的m3u8文件的名称。必须以”.m3u8”结尾，长度范围为\[6, 128\]。 子节点：无

 父节点：Target

 默认值：playlist.m3u8

 取值范围：\[6, 128\] |
|Snapshot|容器|否|保存高频截图操作Snapshot 选项的容器。 子节点：RoleName、DestBucket、NotifyTopic、Interval、PornRec

 父节点：Snapshot |
|RoleName|字符串|否|用于高频截图操作的角色名称，要求有DestBucket的写权限和向NotifyTopic发消息的权限。 子节点：无

 父节点：Snapshot |
|DestBucket|字符串|否|保存高频截图目标Bucket，要求与当前Bucket是同一个Owner。 子节点：无

 父节点：Snapshot |
|NotifyTopic|字符串|否|用于通知用户高频截图操作结果的MNS的Topic。 子节点：无

 父节点：Snapshot |
|Interval|数字|否|高频截图的间隔长度。如果该段间隔时间内没有关键帧（I 帧），那么该间隔时间不截图。 单位：秒

 子节点：无

 父节点：Snapshot

 取值范围：\[1, 100\] |

## 响应元素

|名称|类型|描述|
|:-|:-|:-|
|CreateLiveChannelResult|容器|保存CreateLiveChannel请求结果的容器。 子节点：PublishUrls,PlayUrls

 父节点：无 |
|PublishUrls|容器|保存推流地址的容器。 子节点：Url

 父节点：CreateLiveChannelResult |
|Url|字符串|推流地址。 子节点：无

 父节点：PublishUrls

 **说明：**

-   推流地址是未加签名的URL，如Bucket ACL非public-read-write，则需先进行签名才可访问。
-   播放地址是未加签名的URL，如Bucket ACL为private，则需先进行签名才可访问。 |
|PlayUrls|容器|保存播放地址的容器。 子节点：Url

 父节点：CreateLiveChannelResult |
|Url|字符串|播放地址。 子节点：无

 父节点：PlayUrls |

## 示例

请求示例

```
PUT /test-channel?live HTTP/1.1
Date: Wed, 24 Aug 2016 11:11:28 GMT
Content-Length: 333
Host: test-bucket.oss-cn-hangzhou.aliyuncs.com
Authorization: OSS YJjHKOKWDWINLKXv:hvwOZJRh8toAj3DZvtsuPgf+a****
<?xml version="1.0" encoding="utf-8"?>
<LiveChannelConfiguration>
    <Description/>
    <Status>enabled</Status>
    <Target>
        <Type>HLS</Type>
        <FragDuration>2</FragDuration>
        <FragCount>3</FragCount>
    </Target>
    <Snapshot>
        <RoleName>role_for_snapshot</RoleName>
        <DestBucket>snapshotdest</DestBucket>
        <NotifyTopic>snapshotnotify</NotifyTopic>
        <Interval>1</Interval>
     </Snapshot>
</LiveChannelConfiguration>
```

返回示例

```
HTTP/1.1 200
content-length: 259
server: AliyunOSS
x-oss-server-time: 4
connection: close
x-oss-request-id: 57BD8419B92475920B0002F1
date: Wed, 24 Aug 2016 11:11:28 GMT
x-oss-bucket-storage-type: standard
content-type: application/xml
<?xml version="1.0" encoding="UTF-8"?>
<CreateLiveChannelResult>
  <PublishUrls>
    <Url>rtmp://test-bucket.oss-cn-hangzhou.aliyuncs.com/live/test-channel</Url>
  </PublishUrls>
  <PlayUrls>
    <Url>http://test-bucket.oss-cn-hangzhou.aliyuncs.com/test-channel/playlist.m3u8</Url>
  </PlayUrls>
</CreateLiveChannelResult>
```

## SDK

[Java]()

