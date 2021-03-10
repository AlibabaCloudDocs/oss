# RTMP ingest URLs and signatures

This topic describes RTMP ingest URLs and their signature method.

**Note:** You must add a signature to an RTMP ingest URL only when the bucket ACL is not set to public-read-write. The signature method of RTMP ingest URLs is similar to that of OSS URLs.

## RTMP ingest URL

An RTMP ingest URL must be in the following format:`rtmp://${bucket}.${host}/live/${channel}?${params}`. Example: `rtmp://your-bucket.oss-cn-hangzhou.aliyuncs.com/live/test-channel`.

-   `bucket`: The bucket name. Example: `examplebucket`. For more information about the naming conventions for buckets, see [bucket](/intl.en-US/Developer Guide/Terms.md).
-   `host`: The endpoint of the region. Example: `oss-cn-hangzhou.aliyuncs.com`. For more information about endpoints, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).
-   `live`: The name of the app used for RTMP ingest. OSS uses "live" for RTMP ingest.
-   `channel`: The name of the LiveChannel. Example: `test-channel`. For more information about the naming conventions for LiveChannels, see [PutLiveChannel](/intl.en-US/API Reference/LiveChannel-related operations/PutLiveChannel.md).
-   `params`: The ingest parameter. The format of the parameter must be the same as that of the query string of an HTTP request. Example: `varA=valueA&varB=valueB`.

## RTMP ingest URL parameters

The following table describes the parameters in RTMP ingest URLs.

|Parameter|Description|
|:--------|:----------|
|playlistName|The name of the generated M3U8 file. **Note:** The generated M3U8 file will still contain the `${channel_name}/` prefix. |

## Signature method of RTMP ingest URLs

A signed RTMP ingest URL is in the following format:`rtmp://${bucket}.${host}/live/${channel}?OSSAccessKeyId=xxx&Expires=yyy&Signature=zzz&${params}`.

The following table describes the parameters in signed RTMP ingest URLs.

|Parameter|Description|
|:--------|:----------|
|OSSAccessKeyId|Plays the same role as AccessKeyId in signed HTTP requests.|
|Expires|The UNIX timestamp.|
|Signature|The signature string.|
|params|Other parameters. All parameters must be included in the signature.|

The signature is calculated using the following method:

```
base64(hmac-sha1(AccessKeySecret,
    + Expires + "\n"
    + CanonicalizedParams
    + CanonicalizedResource))
```

The following table describes the parameters involved in signature calculation.

|Parameter|Description|
|:--------|:----------|
|CanonicalizedParams|The canonicalized query string created by arranging the parameter keys in alphabetical order. Parameters must be in the `key:value\n` format. **Note:**

-   The value of this parameter is null if no parameter is specified.
-   SecurityToken, OSSAccessKeyId, Expire, and Signature are not used for creating a canonicalized query string.
-   Every parameter key is used in the string only once. |
|CanonicalizedResource|The value of this parameter is in the `/BucketName/ChannelName` format. Example: `examplebucket/test-channel`.|

