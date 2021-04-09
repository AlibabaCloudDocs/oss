# InitiateMultipartUpload

Before you use multipart upload to upload data, you must call the InitiateMultipartUpload operation to notify OSS to initiate a multipart upload task.

## Usage notes

-   The operation returns a unique upload ID created by the OSS server to identify the multipart upload task. You can initiate operations such as stopping or querying the multipart upload task based on this upload ID.
-   When you initiate a multipart upload request to upload an object, the existing object of the same name is not affected.
-   If you want to calculate the signature for authentication when you call this operation, you must add`?uploads` to the `CanonicalizedResource` header.

## Request structure

```
POST /ObjectName?uploads HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT date
Authorization: SignatureValue
```

## Request parameters

When you call the InitiateMultipartUpload operation, you can specify the encoding-type parameter in the request to encode the object name in the response.

|Parameter|Type|Description|
|:--------|:---|:----------|
|encoding-type|String|The encoding type of the object name in the response. Only URL encoding is supported. The object name can contain characters encoded in UTF-8. However, the XML 1.0 standard cannot be used to parse specific control characters, such as characters whose ASCII values range from 0 to 10. You can configure the encoding-type parameter to encode object name that includes characters that cannot be parsed by XML 1.0 in the response. Default value: null

Valid value: url |

## Request headers

InitiateMultipartUpload requests support custom headers that start with `x-oss-meta-` and the following standard HTTP request headers: Cache-Control, Content-Disposition, Content-Encoding, Content-Type, and Expires. For more information, see [PutObject](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.md).

|Header|Type|Description|
|:-----|:---|:----------|
|Cache-Control|String|The web page caching behavior that is performed when the object is downloaded. For more information, see [RFC2616](https://www.ietf.org/rfc/rfc2616.txt). Default value: null |
|Content-Disposition|String|The name of the object when the object is downloaded. For more information, see [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt). Default value: null |
|Content-Encoding|String|The content encoding type of the object during the download. For more information, see [RFC2616](https://www.ietf.org/rfc/rfc2616.txt). Default value: null |
|Expires|Integer|The timeout period after which the response is considered expired. Unit: millisecond. For more information, see [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt). Default value: null |
|x-oss-forbid-overwrite|String|Specifies whether the InitiateMultipartUpload operation overwrites objects of the same name. When the versioning status of the requested bucket is enabled or suspended, the x-oss-forbid-overwrite request header is invalid. In this case, the PostObject operation overwrites objects of the same name. -   If x-oss-forbid-overwrite is not specified or set to false, the object that has the same name in the destination bucket can be overwritten.
-   If x-oss-forbid-overwrite is set to true, the object that has the same name in the destination bucket cannot be overwritten.

If you specify the x-oss-forbid-overwrite request header, the queries per second \(QPS\) performance of OSS may be degraded. If you want to use the x-oss-forbid-overwrite request header to perform a large number of operations \(QPS greater than 1,000\), submit a ticket. |
|x-oss-server-side-encryption|String|The server-side encryption algorithm that is used when each part of the object is uploaded. Valid values: AES256 and KMS.

**Note:** You must activate KMS before you set this header to KMS.

If you specify this header, it is included in the response. OSS uses the method specified by this header to encrypt each uploaded part. When you download the encrypted object, the x-oss-server-side-encryption header is included in the response and the header value is set to the algorithm used to encrypt the object. |
|x-oss-server-side-encryption-key-id|String|The ID of the customer master key \(CMK\) hosted in KMS. This parameter is valid only when x-oss-server-side-encryption is set to KMS. |
|x-oss-storage-class|String|The storage class of the destination object. Valid values: Standard, IA, Archive, and ColdArchive.

If the storage class is specified when you upload the object, the specified storage class applies regardless of the storage class of the bucket that contains the object. If you set x-oss-storage-class to Standard when you upload an object to an IA bucket, the object is stored as a Standard object.

Supported operations: PutObject, InitiateMultipartUpload, AppendObject, PutObjectSymlink, and CopyObject |
|x-oss-tagging|String|Specifies the tags of the object. You can configure multiple tags for the object. Example: TagA=A&TagB=B. **Note:** The tag key and value must be URL-encoded. If a tag does not contain an equal sign \(`=`\), the tag value is considered as an empty string. |

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
|EncodingType|String|The encoding type of the object name in the response. If the encoding-type parameter is specified in the request, the object name in the response is encoded. Parent nodes: CompleteMultipartUploadResult |

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
|FileAlreadyExists|409|Possible causes:-   An object that has the same name already exists when the header contains `x-oss-forbid-overwrite=true` that is used to prevent overwriting the object that has the same name.
-   The object that you want to upload by using InitiateMultipartUpload is a directory in a bucket for which the hierarchical namespace feature is enabled. |

