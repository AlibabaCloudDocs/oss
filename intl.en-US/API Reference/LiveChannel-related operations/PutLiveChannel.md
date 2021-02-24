# PutLiveChannel

You must call this operation to create a LiveChannel before you upload audio and video data by using the RTMP protocol. The response to a PutLiveChannel request includes the URL used to ingest a stream to the LiveChannel and the URL used to play the stream ingested to the LiveChannel.

**Note:** You can use the returned URLs to ingest and play streams. In addition, you can perform operations based on the returned LiveChannel name, such as query stream ingesting status, query stream ingesting records, and disable stream ingesting.

## Request structure

```
PUT /ChannelName? live HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT date
Content-Length: Size
Authorization: SignatureValue
<? xml version="1.0" encoding="UTF-8"? >
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

## Request headers

|Header|Type|Required|Description|
|------|----|--------|-----------|
|ChannelName|String|Yes|The name of the LiveChanel that you want to create. The name must comply with the naming conventions for objects and cannot contain forward slashes \(/\).|

## Request elements

|Element|Type|Required|Description|
|:------|:---|--------|:----------|
|LiveChannelConfiguration|Container|Yes|The container that stores the configurations of the LiveChannel. Child nodes: Description, Status, and Target

Parent nodes: none |
|Description|String|No|The description of the LiveChannel. The description can be up to 128 bytes in length. Child nodes: none

Parent nodes: LiveChannelConfiguration |
|Status|Enumerated string|No|The status of the LiveChannel. Child nodes: none

Parent nodes: LiveChannelConfiguration

Valid values: enabled and disabled

Default value: enabled |
|Target|Container|Yes|The container that stores the configurations used by the LiveChannel to store uploaded data. Child nodes: Type, FragDuration, FragCount, and PlaylistName

Parent nodes: LiveChannelConfiguration |
|Type|Enumerated string|Yes|The format in which the LiveChannel stores uploaded data. Child nodes: none

Parent nodes: Target

Valid value: HLS

**Note:**

-   If the value of Type is HLS, OSS updates the m3u8 file each time when a ts file is generated. The maximum number of the latest ts files that can be contained in the m3u8 file is specified by the FragCount element.
-   If the value of Type is HLS, when the duration of the audio and video data written to the current ts file exceeds the duration specified by FragDuration, OSS switches to the next ts file to write data before the next key frame is received. If OSS does not receive the next key frame after max\(2\*FragDuration, 60s\), OSS forcibly switches to the next ts file. In this case, the playback of the stream may stall. |
|FragDuration|String|No|The duration of each ts file when the value Type is HLS. Unit: seconds

Child nodes: none

Parent nodes: Target

Valid values: \[1, 100\]

Default value: 5

**Note:** The default values of FragDuration and FragCount take effect only when the values of the two elements are both not specified. The values of the FragDuration and FragCount elements must be specified at the same time. |
|FragCount|String|No|The number of ts files included in the m3u8 file when the value of Type is HLS. Child nodes: none

Parent nodes: Target

Valid values: \[1, 100\]

Default value: 3

**Note:** The default values of FragDuration and FragCount take effect only when the values of the two elements are both not specified. The values of the FragDuration and FragCount elements must be specified at the same time. |
|PlaylistName|String|No|The name of the generated m3u8 file when the value of Type is HLS. The name must end with .m3u8. Child nodes: none

Parent nodes: Target

Default value: playlist.m3u8

Valid values: \[6, 128\] |
|Snapshot|Container|No|The container that stores the options of the high-frequency snapshot operations. Child nodes: RoleName, DestBucket, NotifyTopic, Interval, and PornRec

Parent nodes: Snapshot |
|RoleName|String|No|The name of the role used to perform high-frequency snapshot operations. The role must have write permissions on DestBucket and the permission to send messages to NotifyTopic. Child nodes: none

Parent nodes: Snapshot |
|DestBucket|String|No|The bucket that stores the results of high-frequency snapshot operations. The bucket must be in the same region as the current bucket. Child nodes: none

Parent nodes: Snapshot |
|NotifyTopic|String|No|The MNS topic used to notify users of the results of high- frequency snapshot operations. Child nodes: none

Parent nodes: Snapshot |
|Interval|Number|No|The interval of high-frequency snapshot operations. If no key frame \(inline frame\) exists within the interval, no snapshot is captured. Unit: seconds

Child nodes: none

Parent nodes: Snapshot

Valid values: \[1, 100\] |

## Response elements

|Element|Type|Description|
|:------|:---|:----------|
|CreateLiveChannelResult|Container|The container that stores the result of the CreateLiveChannel request. Child nodes: PublishUrls and PlayUrls

Parent nodes: none |
|PublishUrls|Container|The container that stores the URL used to ingest streams to the LiveChannel. Child nodes: Url

Parent nodes: CreateLiveChannelResult |
|Url|String|The URL used to ingest streams to the LiveChannel. Child nodes: none

Parent nodes: PublishUrls

**Note:**

-   The URL used to ingest streams is not signed. If the ACL of the bucket is not public read/write, you must add a signature to the URL before you use the URL.
-   The URL used to play streams is not signed. If the ACL of the bucket is private, you must add a signature to the URL before you use the URL. |
|PlayUrls|Container|The container that stores the URL used to play the streams ingested to the LiveChannel. Child nodes: Url

Parent nodes: CreateLiveChannelResult |
|Url|String|The URL used to play the streams ingested to the LiveChannel. Child nodes: none

Parent nodes: PlayUrls |

## Examples

Sample requests

```
PUT /test-channel? live HTTP/1.1
Date: Wed, 24 Aug 2016 11:11:28 GMT
Content-Length: 333
Host: test-bucket.oss-cn-hangzhou.aliyuncs.com
Authorization: OSS YJjHKOKWDWINLKXv:hvwOZJRh8toAj3DZvtsuPgf+a****
<? xml version="1.0" encoding="utf-8"? >
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

Sample responses

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
<? xml version="1.0" encoding="UTF-8"? >
<CreateLiveChannelResult>
  <PublishUrls>
    <Url>rtmp://test-bucket.oss-cn-hangzhou.aliyuncs.com/live/test-channel</Url>
  </PublishUrls>
  <PlayUrls>
    <Url>http://test-bucket.oss-cn-hangzhou.aliyuncs.com/test-channel/playlist.m3u8</Url>
  </PlayUrls>
</CreateLiveChannelResult>
```

