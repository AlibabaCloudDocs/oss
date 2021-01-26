# ListParts

You can call this operation to list all parts that are uploaded by using a specified upload ID.

## Usage notes

-   The results returned by OSS are listed in ascending order of their part numbers.
-   Errors may occur during network transmission. Therefore, to generate the list of uploaded parts, we recommend that you do not use the results \(part numbers and ETag values\) of ListParts.

## Request structure

```
Get  /ObjectName? uploadId=UploadId HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: Signature
```

## Request headers

A ListParts request contains only common request headers. For more information, see [Common request headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Request parameters

|Parameter|Type|Example|Description|
|:--------|:---|-------|:----------|
|uploadId|String|0004B999EF5A239BB9138C6227D69F95|The ID of the multipart upload task. Default value: null |
|max-parts|Integer|1000|The maximum number of parts to list in the OSS response. Default value: 1000

Maximum value: 1000 |
|part-number-marker|Integer|100|The number of the part after which the list operation begins. All parts whose numbers are greater than the value of this parameter are listed. Default value: null |
|encoding-type|String|url|The encoding type of the object name in the response. The object name can contain any characters encoded in UTF-8. However, the XML 1.0 standard cannot be used to parse certain control characters, such as characters with an ASCII value from 0 to 10. You can set the Encoding-type parameter to encode the returned object name. Default value: null

Valid value: url |

## Response headers

The response to a ListParts request contains only common response headers. For more information, see [Common response headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Response elements

|Element|Type|Example|Description|
|:------|:---|-------|:----------|
|ListPartsResult|Container|N/A|The container that stores the response to the ListParts request. Child nodes: Bucket, Key, UploadId, PartNumberMarker, NextPartNumberMarker, MaxParts, IsTruncated, and Part

Parent nodes: none |
|Bucket|String|multipart\_upload|The bucket name. Parent nodes: ListPartsResult |
|EncodingType|String|url|The encoding type of the object name in the response. If the Encoding-type parameter is specified in the request, the object name in the response is encoded. Parent nodes: ListPartsResult |
|Key|String|multipart.data|The name of the object Parent nodes: ListPartsResult |
|UploadId|String|0004B999EF5A239BB9138C6227D69F95|The ID of the upload task. Parent nodes: ListPartsResult |
|PartNumberMarker|Integer|10|The number of the part after which the list operation begins. All parts whose numbers are greater than the value of this parameter are listed. Parent nodes: ListPartsResult |
|NextPartNumberMarker|Integer|5|If the response does not contain all required results, all parts whose part numbers are greater than the value of this parameter are listed. Parent nodes: ListPartsResult |
|MaxParts|Integer|1000|The maximum number of parts in this response. Parent nodes: ListPartsResult |
|IsTruncated|Enumerated string|false|Indicates whether the list of parts returned in the response has been truncated. "true" indicates that the response does not contain all required results. "false" indicates that the response contains all required results. Valid values: true and false

Parent nodes: ListPartsResult |
|Part|Container|N/A|The container that stores part information.Child nodes: PartNumber, LastModified, ETag, and Size

Parent nodes: ListPartsResult |
|PartNumber|Integer|1|The number that identifies a part. Parent nodes: ListPartsResult.Part |
|LastModified|Date|2012-02-23T07:01:34.000Z|The time when the part was uploaded. Parent nodes: ListPartsResult.Part |
|ETag|String|3349DC700140D7F86A0784842780\*\*\*\*|The ETag value of the uploaded part. Parent nodes: ListPartsResult and Part |
|Size|Integer|6291456|The size of the uploaded part. Parent nodes: ListPartsResult and Part |

## Examples

Sample requests

```
Get  /multipart.data? uploadId=0004B999EF5A239BB9138C6227D69F95  HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Thu, 23 Feb 2012 07:13:28 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:4qOnUMc9UQWqkz8wDqD3lIsa9P8=
```

Sample responses

```
HTTP/1.1 200 
Server: AliyunOSS
Connection: keep-alive
Content-length: 1221
Content-type: application/xml
x-oss-request-id: 106452c8-10ff-812d-736e-c865294afc1c
Date: Thu, 23 Feb 2012 07:13:28 GMT

<? xml version="1.0" encoding="UTF-8"? >
<ListPartsResult xmlns="http://doc.oss-cn-hangzhou.aliyuncs.com">
    <Bucket>multipart_upload</Bucket>
    <Key>multipart.data</Key>
    <UploadId>0004B999EF5A239BB9138C6227D69F95</UploadId>
    <NextPartNumberMarker>5</NextPartNumberMarker>
    <MaxParts>1000</MaxParts>
    <IsTruncated>false</IsTruncated>
    <Part>
        <PartNumber>1</PartNumber>
        <LastModified>2012-02-23T07:01:34.000Z</LastModified>
        <ETag>"3349DC700140D7F86A0784842780****"</ETag>
        <Size>6291456</Size>
    </Part>
    <Part>
        <PartNumber>2</PartNumber>
        <LastModified>2012-02-23T07:01:12.000Z</LastModified>
        <ETag>"3349DC700140D7F86A0784842780****"</ETag>
        <Size>6291456</Size>
    </Part>
    <Part>
        <PartNumber>5</PartNumber>
        <LastModified>2012-02-23T07:02:03.000Z</LastModified>
        <ETag>"7265F4D211B56873A381D321F586****"</ETag>
        <Size>1024</Size>
    </Part>
</ListPartsResult>
```

## SDK

You can use OSS SDKs for the following programming languages to call the ListParts operation:

-   [Java](/intl.en-US/SDK Reference/Java/Upload objects/Multipart upload.md)
-   [PHP](/intl.en-US/SDK Reference/PHP/Upload objects/Multipart upload.md)
-   [Go](/intl.en-US/SDK Reference/Go/Upload objects/Multipart upload.md)
-   [C](/intl.en-US/SDK Reference/C/Upload objects/Multipart upload.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Upload objects/Multipart upload.md)

