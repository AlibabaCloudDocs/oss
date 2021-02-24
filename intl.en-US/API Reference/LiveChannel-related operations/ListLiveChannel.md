# ListLiveChannel

You can call this operation to list specified LiveChannels.

## Request structure

```
GET /? live HTTP/1.1
Date: GMT date
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Authorization: SignatureValue
```

## Request elements

|Element|Type|Required|Description|
|:------|----|--------|:----------|
|Marker|String|No|The name of the LiveChannel from which the list operation begins.|
|Max-keys|String|No|The maximum number of LiveChannels that can be returned in the operation. The value of max-keys cannot exceed 1,000. Default value: 100 |
|Prefix|String|No|The prefix that the names of the returned LiveChannels must contain. If you specify a prefix in the request, the specified prefix is included in the response.|

## Response elements

|Element|Type|Description|
|:------|:---|:----------|
|ListLiveChannelResult|Container|The container that stores the result of the ListLiveChannel request. Child nodes: Prefix, Marker, MaxKeys, IsTruncated, NextMarker, and LiveChannel

Parent nodes: none |
|Prefix|String|The prefix that the names of the returned LiveChannels contain. Child nodes: none

Parent nodes: ListLiveChannelResult |
|Marker|String|The name of the LiveChannel from which the ListLiveChannel operation begins. Child nodes: none

Parent nodes: ListLiveChannelResult |
|MaxKeys|String|The maximum number of buckets returned each time. Child nodes: none

Parent nodes: ListLiveChannelResult |
|IsTruncated|String|Indicates whether the returned results are truncated. -   true: indicates that all results are returned.
-   false: indicates that not all results are returned.

Child nodes: none

Parent nodes: ListLiveChannelResult |
|NextMarker|String|If not all results are returned, the NextMarker element is included in the response to indicate the Marker value of the next request. Child nodes: none

Parent nodes: ListLiveChannelResult |
|LiveChannel|Container|The container that stores the information about each returned LiveChannel. Child nodes: Name, Description, Status, LastModified, PublishUrls, and PlayUrls

Parent nodes: ListLiveChannelResult |
|Name|String|The name of the LiveChannel. Child nodes: none

Parent nodes: LiveChannel |
|Description|String|The description of the LiveChannel. Child nodes: none

Parent nodes: LiveChannel |
|Status|Enumerated string|The status of the LiveChannel. Child nodes: none

Parent nodes: LiveChannel

Valid values:

-   disabled: indicates that the LiveChannel is disabled.
-   enabled: indicates that the LiveChannel is enabled. |
|LastModified|String|The time when the LiveChannel configuration is last modified. Format: ISO 8601 timestamp

Child nodes: none

Parent nodes: LiveChannel |
|PublishUrls|Container|The container that stores the URL used to push a stream to the LiveChannel. Child nodes: Url

Parent nodes: LiveChannel |
|Url|String|The URL used to push a stream to the LiveChannel. Child nodes: none

Parent nodes: PublishUrls |
|PlayUrls|Container|The container that stores the URL used to play a stream pushed to the LiveChannel. Child nodes: Url

Parent nodes: LiveChannel |
|Url|String|The URL used to play a stream pushed to the LiveChannel. Child nodes: none

Parent nodes: PlayUrls |

## Examples

Sample requests

```
GET /? live&max-keys=1 HTTP/1.1
Date: Thu, 25 Aug 2016 07:50:09 GMT
Host: test-bucket.oss-cn-hangzhou.aliyuncs.com
Authorization: OSS YJjHKOKWDWIN****:TaX+tlc/Xsgpz6uRuqcbmUJs****
```

Sample responses

```
HTTP/1.1 200
content-length: 656
server: AliyunOSS
connection: close
x-oss-request-id: 57BEA331B92475920B00****
date: Thu, 25 Aug 2016 07:50:09 GMT
content-type: application/xml
<? xml version="1.0" encoding="UTF-8"? >
<ListLiveChannelResult>
  <Prefix></Prefix>
  <Marker></Marker>
  <MaxKeys>1</MaxKeys>
  <IsTruncated>true</IsTruncated>
  <NextMarker>channel-0</NextMarker>
  <LiveChannel>
    <Name>channel-0</Name>
    <Description></Description>
    <Status>disabled</Status>
    <LastModified>2016-07-30T01:54:21.000Z</LastModified>
    <PublishUrls>
      <Url>rtmp://test-bucket.oss-cn-hangzhou.aliyuncs.com/live/channel-0</Url>
    </PublishUrls>
    <PlayUrls>
      <Url>http://test-bucket.oss-cn-hangzhou.aliyuncs.com/channel-0/playlist.m3u8</Url>
    </PlayUrls>
  </LiveChannel>
```

