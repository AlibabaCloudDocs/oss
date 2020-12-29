# DeleteLiveChannel

You can call this operation to delete a specified LiveChannel.

## Request structure

**Note:**

-   If you send a DeleteLiveChannel request to delete a LiveChannel to which a client is ingesting streams, the request fails.
-   When you call this operation to delete a LiveChannel, only the LiveChannel is deleted. Files generated during stream ingestion are not deleted.

```
DELETE /ChannelName? live HTTP/1.1
Date: GMT date
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Authorization: SignatureValue
```

## Examples

Sample request

```
DELETE /test-channel? live HTTP/1.1
Date: Thu, 25 Aug 2016 07:32:26 GMT
Host: test-bucket.oss-cn-hangzhou.aliyuncs.com
Authorization: OSS YJjHKOKWDWINLKXv:ZbfvQ3XwmYEE8O9CX8kwVQY****
```

Sample response

```
HTTP/1.1 204
content-length: 0
server: AliyunOSS
connection: close
x-oss-request-id: 57BE9F0AB92475920B0*****
date: Thu, 25 Aug 2016 07:32:26 GMT
```

