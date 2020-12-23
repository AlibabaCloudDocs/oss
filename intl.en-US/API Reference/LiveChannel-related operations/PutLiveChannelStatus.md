# PutLiveChannelStatus

A LiveChannel can be in one of the following states: enabled or disabled. You can call this operation to set a LiveChannel to one of the two states.

## Usage notes

When you call PutLiveChannelStatus, take note of the following items:

-   If a LiveChannel is in the disabled state, OSS prohibits you from ingesting streams to the LiveChannel. If you are ingesting a stream to a LiveChannel when the status of the LiveChannel is switched to disabled, your client is disconnected from the LiveChannel after about 10 seconds.
-   When no stream is ingested to a LiveChannel, You can also re-create the LiveChannel by calling PutLiveChannel to change the status of the LiveChannel.
-   When a stream is being ingested to a LiveChannel, you can only call PutLiveChannelStatus to set the LiveChannel to the disabled state.

## Request structure

```
PUT /ChannelName? live&status=NewStatus HTTP/1.1
Date: Tue, 25 Dec 2018 17:35:24 GMT
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Authorization: SignatureValue
```

## Request headers

|Header|Type|Required|Description|
|:-----|----|--------|-----------|
|NewStatus|String|Yes|Specifies the status of the LiveChannel. Valid values:

-   enabled: enables the LiveChannel.
-   disabled: disables the LiveChannel. |

For more information about the common request headers contained in a PutLiveChannelStatus request, see [Common request headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Response headers

The response to a PutLiveChannelStatus request contains only common response headers. For more information, see [Common response headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Examples

Sample request

```
PUT /test-channel? live&status=disabled HTTP/1.1
Date: Tue, 25 Dec 2018 17:35:24 GMT
Host: test-bucket.oss-cn-hangzhou.aliyuncs.com
Authorization: OSS YJjHKOKWDWIN****:X/mBrSbkNoqM/JoAfRC0ytyQ****
```

Sample response

```
HTTP/1.1 200
Content-Length: 0
Server: AliyunOSS
Connection: close
x-oss-request-id: 57BE8422B92475920B00****
Date: Tue, 25 Dec 2018 17:35:24 GMT
```

