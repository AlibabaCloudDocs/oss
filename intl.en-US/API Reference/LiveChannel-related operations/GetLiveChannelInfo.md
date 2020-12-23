# GetLiveChannelInfo

You can call this operation to query the configuration information about a specified LiveChannel.

## Request structure

```
GET /ChannelName? live HTTP/1.1
Date: GMT date
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Authorization: SignatureValue
```

## Request headers

A GetLiveChannelInfo request contains only common request headers. For more information, see [Common request headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Response headers

The response to a GetLiveChannelInfo request contains only common response headers. For more information, see [Common response headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Response elements

|Element|Type|Example|Description|
|:------|:---|-------|:----------|
|LiveChannelConfiguration|Container|N/A|The container that stores the returned results of the GetLiveChannelInfo request. Child nodes: Description, Status, and Target

Parent node: none |
|Description|String|test|The description of the LiveChannel. Child node: none

Parent node: LiveChannelConfiguration |
|Status|Enumerated string|enabled|The status of the LiveChannel. Child node: none

Parent node: LiveChannelConfiguration

Valid values:

-   enabled: indicates that the LiveChannel is enabled.
-   disabled: indicates that the LiveChannel is disabled. |
|Target|Container|N/A|The container that stores the configurations used by the LiveChannel to store uploaded data. Child nodes: Type, FragDuration, FragCount, and PlaylistName

**Note:** FragDuration, FragCount, and PlaylistName are returned only when the value of Type is HLS.

Parent node: LiveChannelConfiguration |
|Type|Enumerated string|HLS|The format in which the uploaded data is stored when the value of Type is HLS. Child node: none

Parent node: Target

Valid value: HLS |
|FragDuration|String|2|The duration of each ts file when the value of Type is HLS. Unit: seconds

Child node: none

Parent node: Target |
|FragCount|String|3|The number of ts files included in the m3u8 file when the value of Type is HLS. Child node: none

Parent node: Target |
|PlaylistName|String|playlist.m3u8|The name of the generated m3u8 file when the value of Type is HLS. Child node: none

Parent node: Target |

## Examples

Sample requests

```
GET /test-channel? live HTTP/1.1
Date: Thu, 25 Aug 2016 05:52:40 GMT
Host: test-bucket.oss-cn-hangzhou.aliyuncs.com
Authorization: OSS YJjHKOKWDWIN****:D6bDCRXKht58hin1BL83wxyG****
```

Sample responses

```
HTTP/1.1 200
content-length: 475
server: AliyunOSS
connection: close
x-oss-request-id: 57BE87A8B92475920B00****
date: Thu, 25 Aug 2016 05:52:40 GMT
content-type: application/xml
<? xml version="1.0" encoding="UTF-8"? >
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

