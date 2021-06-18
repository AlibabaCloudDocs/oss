# ListMultipartUploads

Lists all multipart upload tasks in progress. The tasks are not completed or canceled.

## Request structure

```
Get /?uploads HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: Signature
```

## Request parameters

|Parameter|Type|Description|
|:--------|:---|:----------|
|delimiter|String|The character used to group objects by name. If you specify the Delimiter parameter in the request, the response contains the CommonPrefixes element. Objects whose names contain the same string from the prefix to the next occurrence of the delimiter are grouped as a single result element in CommonPrefixes.|
|max-uploads|String|The maximum number of multipart upload tasks that can be returned for the current request. Default value: 1000. Maximum value: 1000.|
|key-marker|String|This parameter is used together with the upload-id-marker parameter to specify the position from which the next list begins. -   If the upload-id-marker parameter is not set, Object Storage Service \(OSS\) returns all multipart upload tasks in which object names are alphabetically after the key-marker value.
-   If the upload-id-marker parameter is set, the response includes the following tasks:
    -   Multipart upload tasks in which object names are alphabetically after the key-marker value in alphabetical order
    -   Multipart upload tasks in which object names are the same as the key-marker parameter value but whose upload IDs are greater than the upload-id-marker parameter value |
|prefix|String|The prefix that the returned object names must contain. If you specify a prefix in the request, the specified prefix is included in the response. **Note:** You can use prefixes to group and manage objects in buckets in the same way you manage a folder in a file system. |
|upload-id-marker|String|The upload ID of the multipart upload task after which the list begins. This parameter is used together with the key-marker parameter. -   If the key-marker parameter is not set, OSS ignores the upload-id-marker parameter.
-   If the key-marker parameter is configured, the query result includes:
    -   Multipart upload tasks in which object names are alphabetically after the key-marker value in alphabetical order
    -   Multipart upload tasks in which object names are the same as the key-marker parameter value but whose upload IDs are greater than the upload-id-marker parameter value |
|encoding-type|String|The encoding type of the object name in the response. Values of Delimiter, KeyMarker, Prefix, NextKeyMarker, and Key can be encoded in UTF-8. However, the XML 1.0 standard cannot be used to parse control characters such as characters with an American Standard Code for Information Interchange \(ASCII\) value from 0 to 10. You can set the encoding-type parameter to encode values of Delimiter, KeyMarker, Prefix, NextKeyMarker, and Key in the response. Default value: null |

## Response elements

|Element|Type|Description|
|:------|:---|:----------|
|ListMultipartUploadsResult|Container|The container that stores the response to the ListMultipartUpload request. Child nodes: Bucket, KeyMarker, UploadIdMarker, NextKeyMarker, NextUploadIdMarker, MaxUploads, Delimiter, Prefix, CommonPrefixes, IsTruncated, and Upload

Parent nodes: none |
|Bucket|String|The name of the bucket. Parent nodes: ListMultipartUploadsResult |
|EncodingType|String|The method used to encode the object name in the response. If encoding-type is specified in the request, values of those elements including Delimiter, KeyMarker, Prefix, NextKeyMarker, and Key are encoded in the returned result. Parent nodes: ListMultipartUploadsResult |
|KeyMarker|String|The name of the object that corresponds to the multipart upload task after which the list begins. Parent nodes: ListMultipartUploadsResult |
|UploadIdMarker|String|The upload ID of the multipart upload task after which the list begins. Parent nodes: ListMultipartUploadsResult |
|NextKeyMarker|String|The object name marker in the response for the next request to return the remaining results. Parent nodes: ListMultipartUploadsResult |
|NextUploadMarker|String|The NextUploadMarker value that is used for the UploadMarker value in the next request if the response does not contain all required results. Parent nodes: ListMultipartUploadsResult |
|MaxUploads|Integer|The maximum number of multipart upload tasks returned by OSS. Parent nodes: ListMultipartUploadsResult |
|IsTruncated|Enumerated string|Indicates whether the list of multipart upload tasks returned in the response is truncated. Default value: false. Valid values:-   true: Only part of the results are returned this time.
-   false: All results are returned.

Parent nodes: ListMultipartUploadsResult |
|Upload|Container|The container that stores the information about multipart upload tasks. Child nodes: Key, UploadId, and Initiated

Parent nodes: ListMultipartUploadsResult |
|Key|String|The name of the object for which a multipart upload task was initiated. Parent nodes: Upload

**Note:**

The results returned by OSS are listed in ascending alphabetical order of object names. Multiple multipart upload tasks that are initiated to upload the same object are listed in ascending order of upload IDs. |
|UploadId|String|The ID of the multipart upload task. Parent nodes: Upload |
|Initiated|Date|The time when the multipart upload task was initialized. Parent nodes: Upload |

## Examples

Sample requests

```
Get /?uploads  HTTP/1.1
Host:oss-example. oss-cn-hangzhou.aliyuncs.com
Date: Thu, 23 Feb 2012 06:14:27 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:JX75CtQqsmBBz+dcivn7kwBM****
```

Sample responses

```
HTTP/1.1 200 
Server: AliyunOSS
Connection: keep-alive
Content-length: 1839
Content-type: application/xml
x-oss-request-id: 58a41847-3d93-1905-20db-ba6f561c****
Date: Thu, 23 Feb 2012 06:14:27 GMT

<?xml version="1.0" encoding="UTF-8"?>
<ListMultipartUploadsResult xmlns="http://doc.oss-cn-hangzhou.aliyuncs.com">
    <Bucket>oss-example</Bucket>
    <KeyMarker></KeyMarker>
    <UploadIdMarker></UploadIdMarker>
    <NextKeyMarker>oss.avi</NextKeyMarker>
    <NextUploadIdMarker>0004B99B8E707874FC2D692FA5D77D3F</NextUploadIdMarker>
    <Delimiter></Delimiter>
    <Prefix></Prefix>
    <MaxUploads>1000</MaxUploads>
    <IsTruncated>false</IsTruncated>
    <Upload>
        <Key>multipart.data</Key>
        <UploadId>0004B999EF518A1FE585B0C9360DC4C8</UploadId>
        <Initiated>2012-02-23T04:18:23.000Z</Initiated>
    </Upload>
    <Upload>
        <Key>multipart.data</Key>
        <UploadId>0004B999EF5A239BB9138C6227D6****</UploadId>
        <Initiated>2012-02-23T04:18:23.000Z</Initiated>
    </Upload>
    <Upload>
        <Key>oss.avi</Key>
        <UploadId>0004B99B8E707874FC2D692FA5D7****</UploadId>
        <Initiated>2012-02-23T06:14:27.000Z</Initiated>
    </Upload>
</ListMultipartUploadsResult>
```

## SDKs

You can use OSS SDKs for the following programming languages to call the ListMultipartUploads operation:

-   [Java](/intl.en-US/SDK Reference/Java/Upload objects/Multipart upload.md)
-   [Go](/intl.en-US/SDK Reference/Go/Upload objects/Multipart upload.md)
-   [C++](/intl.en-US/SDK Reference/C++/Upload objects/Multipart upload.md)
-   [PHP](/intl.en-US/SDK Reference/PHP/Upload objects/Multipart upload.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Upload objects/Multipart upload.md)

