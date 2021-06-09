# InitiateMultipartUpload

Before you use multipart upload to upload data, you must call the InitiateMultipartUpload operation to notify OSS to initiate a multipart upload task.

## Usage notes

-   When you call the InitiateMultipartUpload operation, OSS creates and returns a unique upload ID to identify the multipart upload task. You can initiate operations such as stopping or querying the multipart upload task based on this upload ID.
-   When you initiate a multipart upload request to upload an object, the existing object that has the same name is not affected.
-   If you want to calculate the signature for authentication when you call this operation, you must add`?uploads` to the `CanonicalizedResource` header.

## Request structure

```
POST /ObjectName?uploads HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT date
Authorization: SignatureValue
```

## Request parameters

When you call the InitiateMultipartUpload operation, you can specify the encoding-type parameter in the request to encode the object name returned in the response.

|Element|Type|Description|
|:------|:---|:----------|
|encoding-type|String|The method used to encode the object name in the response. Only URL encoding is supported. The object name can contain characters encoded in UTF-8. However, the XML 1.0 standard cannot be used to parse specific control characters, such as characters whose ASCII values range from 0 to 10. You can configure the encoding-type parameter to encode object names that include characters that cannot be parsed by XML 1.0 in the response. Default value: null

Valid value: url |

## Request headers

InitiateMultipartUpload requests support custom headers that start with `x-oss-meta-` and the following standard HTTP request headers: Cache-Control, Content-Disposition, Content-Encoding, Content-Type, and Expires. For more information, see [PutObject](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.md).

|Header|Type|Description|
|:-----|:---|:----------|
|Cache-Control|String|The web page caching behavior that is performed when the object is downloaded. For more information, see [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt). Default value: null |
|Content-Disposition|String|The name of the object when the object is downloaded. For more information, see [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt). Default value: null |
|Content-Encoding|String|The method used to encode the object when the object is downloaded. For more information, see [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt). Default value: null |
|Expires|Integer|The timeout period after which the response is considered expired. Unit: millisecond. For more information, see [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt). Default value: null |
|x-oss-forbid-overwrite|String|Specifies whether the InitiateMultipartUpload operation overwrites the object that have the same name as that of the object to upload. The x-oss-forbid-overwrite request header does not take effect when versioning is enabled or suspended for the bucket to which the object is uploaded. In this case, the InitiateMultipartUpload operation overwrites the existing object that has the same name as that of the object you want to upload. -   If x-oss-forbid-overwrite is not specified or the value of x-oss-forbid-overwrite is set to false, an existing object that has the same name as that of the object you want to upload is overwritten.
-   If the value of x-oss-forbid-overwrite is set to true, an existing object that has the same name as that of the object you want to upload cannot be overwritten.

If you specify the x-oss-forbid-overwrite request header, the queries per second \(QPS\) performance of OSS may be degraded. If you want to specify the x-oss-forbid-overwrite header in a large number of requests \(QPS greater than 1,000\), submit a ticket. |
|x-oss-server-side-encryption|String|The algorithm that is used to encrypt each part of the object to upload on the OSS server. Valid values: AES256 and KMS.

**Note:** You must activate Key Management Service \(KMS\) before you set this header to KMS.

If you specify this header in the request, it is included in the response. OSS uses the algorithm specified by this header to encrypt each uploaded part. When you download the encrypted object, the x-oss-server-side-encryption header is included in the response and the header value is set to the algorithm used to encrypt the object. |
|x-oss-server-side-encryption-key-id|String|The ID of the customer master key \(CMK\) hosted in KMS. This parameter is valid only when x-oss-server-side-encryption is set to KMS. |
|x-oss-storage-class|String|The storage class of the uploaded object. If you specify the storage class when you upload the object, the specified storage class applies regardless of the storage class of the bucket to which the object is uploaded. For example, if you set x-oss-storage-class to Standard when you upload an object to an IA bucket, the storage class of the uploaded object is Standard.

Valid values:

-   Standard
-   IA
-   Archive
-   ColdArchive

For more information about storage classes, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md). |
|x-oss-tagging|String|Specifies the tags of the object. You can configure multiple tags for the object. Example: TagA=A&TagB=B. **Note:** The tag key and value must be URL-encoded. If a tag does not contain an equal sign \(`=`\), the tag value is considered an empty string. |

## Response elements

After OSS receives an InitiateMultipartUpload request, OSS returns a response that contains a message body in the XML format. The message body contains the following elements: Bucket, Key, and UploadID.

|Element|Type|Description|
|:------|:---|:----------|
|Bucket|String|The name of the bucket to which the object is uploaded by the multipart upload task. Parent nodes: InitiateMultipartUploadResult |
|InitiateMultipartUploadResult|Container|The container that is used to store the result of the InitiateMultipartUpload request. Child nodes: Bucket, Key, and UploadId

Parent nodes: none |
|Key|String|The name of the object that is uploaded by the multipart upload task. Parent nodes: InitiateMultipartUploadResult |
|UploadId|String|The unique ID of the multipart upload task. Parent nodes: InitiateMultipartUploadResult

**Note:** Record the upload ID for subsequent multipart-related operations. |
|EncodingType|String|The method used to encode the object name in the response. If the encoding-type parameter is specified in the request, the object name in the response is encoded. Parent nodes: CompleteMultipartUploadResult |

## Examples

-   Sample requests

    ```
    POST /multipart.data?uploads HTTP/1.1 
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com 
    Date: Wed, 22 Feb 2012 08:32:21 GMT 
    x-oss-storage-class: Archive
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:/cluRFtRwMTZpC2hTj4F67AG****
    ```

-   Sample responses

    ```
    HTTP/1.1 200 OK
    Content-Length: 230
    Server: AliyunOSS
    Connection: keep-alive
    x-oss-request-id: 42c25703-7503-fbd8-670a-bda01eae****
    Date: Wed, 22 Feb 2012 08:32:21 GMT
    Content-Type: application/xml
    <?xml version="1.0" encoding="UTF-8"?>
    <InitiateMultipartUploadResult xmlns="http://doc.oss-cn-hangzhou.aliyuncs.com">
        <Bucket> multipart_upload</Bucket>
        <Key>multipart.data</Key>
        <UploadId>0004B9894A22E5B1888A1E29F823****</UploadId>
    </InitiateMultipartUploadResult>
    ```


## SDK

You can use OSS SDKs for the following programming languages to call the InitiateMultipartUpload operation:

-   [Java](/intl.en-US/SDK Reference/Java/Upload objects/Multipart upload.md)
-   [Python](/intl.en-US/SDK Reference/Python/Upload objects/Multipart upload.md)
-   [Go](/intl.en-US/SDK Reference/Go/Upload objects/Multipart upload.md)
-   [C++](/intl.en-US/SDK Reference/C++/Upload objects/Multipart upload.md)
-   [PHP](/intl.en-US/SDK Reference/PHP/Upload objects/Multipart upload.md)
-   [C](/intl.en-US/SDK Reference/C/Upload objects/Multipart upload.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Upload objects/Multipart upload.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Upload objects/Multipart upload.md)
-   [Browser.js](/intl.en-US/SDK Reference/Browser.js/Upload objects.md)

## Error codes

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|InvalidEncryptionAlgorithmError|400|The error message returned because the x-oss-server-side-encryption headeris set to a value other than AES256 and KMS.|
|InvalidArgument|400|The error message returned because the x-oss-server-side-encryption header is specified each time a part is uploaded.|
|InvalidArgument|400|The error message returned because the specified storage class of the object to upload is invalid.|
|KmsServiceNotEnabled|403|The error message returned because KMS is specified as the server-side encryption method but KMS is not activated in the console.|
|FileAlreadyExists|409|Possible causes:-   An object that has the same name already exists when the header contains `x-oss-forbid-overwrite=true` that prevents OSS from overwriting the object that has the same name.
-   The error message returned because the object that you want to upload by using InitiateMultipartUpload is a directory in a bucket for which the hierarchical namespace feature is enabled. |

