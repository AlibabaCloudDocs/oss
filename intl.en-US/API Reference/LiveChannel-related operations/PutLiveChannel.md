# PutLiveChannel

Before uploading audio or video data to OSS through the RTMP protocol, you must use PutLiveChannel to create a LiveChannel. PutLiveChannel returns a URL used to push streams through the RTMP protocol and a URL used to play the uploaded data.

You can use the URLs returned by PutLiveChannel to push streams and play the uploaded data. In addition, you can perform operations on the created LiveChannel, such as query the stream pushing status, query stream pushing records, or disable stream pushing.

## Request syntax

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

## Request elements

|Element|Type|Description|Required|
|:------|:---|:----------|:-------|
|LiveChannelConfiguration|Container|Specifies the container used to store the settings of the LiveChannel.Sub-node: Description, Status and Target

Parent node: None

|Yes|
|Description|String|Specifies the description of the LiveChannel, which is 128 bytes in maximum.Sub-node: None

Parent node: LiveChannelConfiguration

|No|
|Status|Enumerated string|Specifies the status of the LiveChannel.Sub-node: None

Parent node: LiveChannelConfiguration

Valid values: enabled and disabled

Default value: enabled

|No|
|Target|Container|Specifies the container used to store the settings for storing uploaded data.Sub-node: Type, FragDuration, FragCount, and PlaylistName

Parent node: LiveChannelConfiguration

|Yes|
|Type|Enumerated string|Specifies the format that the uploaded data is stored as.Sub-node: None

Parent node: Target

Valid value: HLS

|Yes|
|FragDuration|String|Specifies the duration \(in seconds\) of each ts file when the value of Type is HLS.Sub-node: None

Parent node: Target

Default value: 5

Value range: \[1, 100\]

|No|
|FragCount|String|Specifies the number of ts files included in the m3u8 file when the value of Type is HLS.Sub-node: None

Parent node: Target

Default value: 3

Value range: \[1, 100\]

|No|
|PlaylistName|String|Specifies the name of the m3u8 file generated when the value of Type is HLS. The name must be ended with ".m3u8" and in the following length range: \[6, 128\].Sub-node: None

Parent node: Target

Default value: playlist.m3u8

Value range: \[6, 128\]

|No|
|Snapshot|Container|Specifies the container used to store the Snapshot \(high-frequent snapshot operation\) options.Sub-node: RoleName, DestBucket, NotifyTopic, Interval, and PornRec

Parent node: Snapshot

|No|
|RoleName|String|Specifies the name of the role who performs the high-frequent snapshot operations. The role must have the permission to write data into DestBucket and send messages to NotifyTopic.Sub-node: None

Parent node: Snapshot

|No|
|DestBucket|String|Specifies the bucket where the snapshots are stored. The DestBucket and the current bucket must be owned by the same user. Sub-node: None

Parent node: Snapshot

|No|
|NotifyTopic|String|Specifies the topic of the MNS used to notify the user of the result of high-frequent snapshot operations.Sub-node: None

Parent node: Snapshot

|No|
|Interval|Numeric|Specifies the interval \(in seconds\) between each snapshot operation. If no key frame \(I-frame\) exists in an interval, no snapshot is captured in the interval.Sub-node: None

Parent node: Snapshot

Value range: \[1, 100\]

|No|

## Detail analysis

-   ChannelName must conform to the naming conventions for objects and cannot include "/".
-   The default values of FragDuration and FragCount take effect only when the values are both not specified. If you specify the value of one of the two parameters, the value of the other must also be specified.
-   If the value of Type is HLS, OSS updates the generated m3u8 file each time when a ts file is generated. The number of newly-generated ts files included in the m3u8 file is specified by FragCount.
-   If the value of Type is HLS, when the duration of the video or audio data in the current ts file reaches the value of FragDuration, OSS generates a new ts file when receiving the next key frame. If OSS does not receive the next key frame with in a time peroid \(calculated by max\(2\*FragDuration, 60s\)\), a new ts file is generated, which results lag in audio or video playing.

## Response element

|Element|Type|Description|
|:------|:---|:----------|
|CreateLiveChannelResult|Container|Specifies the container used to store the response fo the CreateLiveChannel request.Sub-nodes: PublishUrls and PlayUrls

Parent node: None |
|PublishUrls|Container|Specifies the container used to store the stream pushing URL.Sub-node: Url

Parent node: CreateLiveChannelResult |
|Url|String|Specifies the stream pushing URL.Sub-node: None

Parent node: PublishUrls |
|PlayUrls|Container|Specifies the container used to store the stream pushing URL.Sub-node: Url

Parent node: CreateLiveChannelResult |
|Url|String|Specifies the URL used to play the audio or video data.Sub-node: None

Parent node: PlayUrls |

## Detail analysis

-   The stream pushing URL is not signed. If the ACL for the bucket is not public-read-write, you must sign the URL before accessing it.
-   The URL used to play the audio or video data is not signed. If the ACL for the bucket is private, you must sign the URL before accessing it.

## Examples

Request example

```
PUT /test-channel?live HTTP/1.1
Date: Wed, 24 Aug 2016 11:11:28 GMT
Content-Length: 333
Host: test-bucket.oss-cn-hangzhou.aliyuncs.com
Authorization: OSS YJjHKOKWDWINLKXv:hvwOZJRh8toAj3DZvtsuPgf+agA=
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

Response example

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

