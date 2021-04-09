# PutObject

You can call this operation to upload objects.

## Usage notes

-   PutObject cannot be used to upload objects larger than 5 GB.
-   By default, if you upload an object that has the same name as that of an accessible existing object, the existing object is overwritten by the uploaded object and 200 OK is returned.

## Versioning

If you initiate a PutObject request to a versioning-enabled bucket, OSS automatically generates a unique version ID for the uploaded object and returns the version ID as the x-oss-version-id header in the response.

If you initiate a PutObject request to a versioning-suspended bucket, OSS automatically generates the null version ID for the uploaded object. An object can have only one version whose version ID is null.

## Request structure

```
PUT /ObjectName HTTP/1.1
Content-Length: ContentLength
Content-Type: ContentType
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Request headers

OSS supports five HTTP request headers: Cache-Control, Expires, Content-Encoding, Content-Disposition, and Content-Type. If these headers are configured when you upload an object, the configured values of these headers are also automatically used when you download the object.

|Header|Type|Required|Description|
|:-----|:---|:-------|:----------|
|Authorization|String|No|Specifies that the request is authorized. For more information, see [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt). The Authorization header is required in most cases. However, if you use a signed URL in a PutObject request, this header is not required. For more information, see [Generate a signed URL](/intl.en-US/API Reference/Access control/Generate a signed URL.md).

Default value: null |
|Cache-Control|String|No|The web page caching behavior that is specified when the object is downloaded. For more information, see [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt). Default value: null |
|Content-Disposition|String|No|The name of the object when the object is downloaded. For more information, see [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt). Default value: null |
|Content-Encoding|String|No|The content encoding type of the object during the download. For more information, see [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt). Default value: null |
|Content-MD5|String|No|The MD5 hash of the object you want to upload. The value of Content-MD5 is calculated based on the MD5 algorithm. After the Content-MD5 request header is uploaded, OSS calculates the MD5 hash of the received object and checks whether the calculated MD5 hash is the same as the Content-MD5 value provided in the request. For more information, see [Calculate the Content-MD5 value](/intl.en-US/API Reference/Access control/Add signatures to headers.md). To ensure data integrity, OSS provides multiple methods for you to check the MD5 hashes of the data. To perform MD5 verification based on the Content-MD5 header, add the Content-MD5 header to the request.

Default value: null |
|Content-Length|String|No|The size of the data in the HTTP request body. If the value of the Content-Length header in the request is smaller than the size of the data in the request body, the object can still be created. However, the data is truncated to the object size specified by Content-Length. |
|ETag|String|No|The ETag that is generated when an object is created. ETags are used to identify the content of the objects. -   If an object is created by using a PutObject request, the ETag value is the MD5 hash of the object content.
-   If an object is created by using other methods, the ETag value is the UUID of the object content.

**Note:** The ETag value of the object can be used to check whether the object content is modified. To verify data integrity, we recommend that you do not use the ETag of an object as the MD5 hash of the object.

Default value: null |
|Expires|String|No|The time period after which the response is considered expired. For more information, see [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt). Default value: null |
|x-oss-forbid-overwrite|String|No|Specifies whether the PutObject operation overwrites objects of the same name. When the versioning status of the requested bucket is enabled or suspended, the x-oss-forbid-overwrite request header is invalid. In this case, the PutObject operation overwrites objects of the same name. -   If x-oss-forbid-overwrite is not specified or the value of x-oss-forbid-overwrite is set to false, an existing object has the same name as that of the object you want to upload can be overwritten.
-   If the value of x-oss-forbid-overwrite is set to true, an existing object that has the samename as that of the object you want to upload cannot be overwritten.

If you specify the x-oss-forbid-overwrite request header, the queries per second \(QPS\) performance of OSS may be degraded. If you want to use the x-oss-forbid-overwrite request header to perform a large number of operations \(QPS greater than 1,000\), submit a ticket.

Default value: false |
|x-oss-server-side-encryption|String|No|The server-side encryption method that is used when OSS creates the object. Valid values: AES256 and KMS

If you specify this parameter, this parameter is returned in the response header and the uploaded object is encrypted and stored. When you download the encrypted object, the x-oss-server-side-encryption header is included in the response and the header value is set to the algorithm used to encrypt the object. |
|x-oss-server-side-encryption-key-id|String|No|The ID of the customer master key \(CMK\) hosted in KMS. This parameter is valid only when x-oss-server-side-encryption is set to KMS. |
|x-oss-object-acl|String|No|The access control list \(ACL\) of the object you want to create. Valid values: public-read, private, and public-read-write |
|x-oss-storage-class|String|No|The storage class of an object. If you specify the storage class when you upload the object, the specified storage class applies regardless of the storage class of the bucket that contains the object. If you set x-oss-storage-class to Standard when you upload an object to an IA bucket, the object is stored as a Standard object.

Valid values: Standard, IA, Archive, and ColdArchive.

Supported operations: PutObject, InitiateMultipartUpload, AppendObject, PutObjectSymlink, and CopyObject. |
|x-oss-meta-\*|String|No|If the PutObject request contains a parameter prefixed with x-oss-meta-\*, the parameter is considered to be user metadata. Example: `x-oss-meta-location`. An object can have multiple similar parameters. However, the total size of the user metadata cannot exceed 8 KB. Metadata supports hyphens \(-\), digits, and letters. Uppercase letters are converted to lowercase letters, and other characters such as underscores \(\_\) are not supported. |
|x-oss-tagging|String|No|The object tag. You can configure multiple tags for the object. Example: TagA=A&TagB=B. **Note:** The tag key and value must be URL-encoded. If a key-value pair does not contain an equal sign \(=\), the tag value is considered an empty string. |

This API operation must also include common request headers such as Host and Date. For more information about common request headers, see [Common request headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Response headers

Response headers involved in this API operation contain only common response headers. For more information about common response headers, see [Common response headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Examples

-   Upload an object by using simple upload

    Sample requests

    ```
    PUT /test.txt HTTP/1.1
    Host: test.oss-cn-zhangjiakou.aliyuncs.com
    User-Agent: aliyun-sdk-python/2.6.0(Windows/7/AMD64;3.7.0)
    Accept: */*
    Connection: keep-alive
    Content-Type: text/plain
    date: Tue, 04 Dec 2018 15:56:37 GMT
    authorization: OSS qn6qrrqxo2oawuk53otfjbyc:kZoYNv66bsmc10+dcGKw5x2P****
    Transfer-Encoding: chunked
    ```

    Sample responses

    ```
    HTTP/1.1 200 OK
    Server: AliyunOSS
    Date: Tue, 04 Dec 2018 15:56:38 GMT
    Content-Length: 0
    Connection: keep-alive
    x-oss-request-id: 5C06A3B67B8B5A3DA422299D
    ETag: "D41D8CD98F00B204E9800998ECF8****"
    x-oss-hash-crc64ecma: 0
    Content-MD5: 1B2M2Y8AsgTpgAmY7PhCfg==
    x-oss-server-time: 7
    ```

-   Upload an Archive object

    Sample requests

    ```
    PUT /oss.jpg HTTP/1.1 
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com Cache-control: no-cache 
    Expires: Fri, 28 Feb 2012 05:38:42 GMT 
    Content-Encoding: utf-8
    Content-Disposition: attachment;filename=oss_download.jpg 
    Date: Fri, 24 Feb 2012 06:03:28 GMT 
    Content-Type: image/jpg 
    Content-Length: 344606 
    x-oss-storage-class: Archive
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:kZoYNv66bsmc10+dcGKw5x2P**** 
    [344606 bytes of object data]
    ```

    Sample responses

    ```
    HTTP/1.1 200 OK
    Server: AliyunOSS
    Date: Sat, 21 Nov 2015 18:52:34 GMT
    Content-Type: image/jpg
    Content-Length: 0
    Connection: keep-alive
    x-oss-request-id: 5650BD72207FB30443962F9A
    x-oss-bucket-version: 1418321259
    ETag: "A797938C31D59EDD08D86188F6D5B872"
    ```

-   Upload an object to a versioning enabled bucket

    Sample requests

    ```
    PUT /test HTTP/1.1
    Content-Length: 362149
    Content-Type: text/html
    Host: versioning-put.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 02:53:24 GMT
    Authorization: OSS lkojgn8y1exic6e:6yYhX+BuuEqzI1tAMW0wgIyl****
    ```

    Sample responses

    ```
    HTTP/1.1 200 OK
    Server: AliyunOSS
    Date: Tue, 09 Apr 2019 02:53:24 GMT
    Content-Length: 0
    Connection: keep-alive
    x-oss-request-id: 5CAC0A3DB7AEADE01700****
    x-oss-version-id: CAEQNhiBgMDJgZCA0BYiIDc4MGZjZGI2OTBjOTRmNTE5NmU5NmFhZjhjYmY0****
    ETag: "4F345B1F066DB1444775AA97D5D2****"
    ```


## SDK

You can use OSS SDKs for the following programming languages to call the PutObject operation:

-   [Java](/intl.en-US/SDK Reference/Java/Upload objects/Simple upload.md)
-   [Python](/intl.en-US/SDK Reference/Python/Upload objects/Simple upload.md)
-   [Go](/intl.en-US/SDK Reference/Go/Upload objects/Simple upload.md)
-   [C++](/intl.en-US/SDK Reference/C++/Upload objects/Simple upload.md)
-   [PHP](/intl.en-US/SDK Reference/PHP/Upload objects/Simple upload.md)
-   [C](/intl.en-US/SDK Reference/C/Upload objects/Simple upload.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Upload objects/Simple upload.md)
-   [Android](/intl.en-US/SDK Reference/Android/Upload objects/Simple upload.md)
-   [iOS](/intl.en-US/SDK Reference/iOS/Upload objects/Overview.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Upload objects/Overview.md)
-   [Browser.js](/intl.en-US/SDK Reference/Browser.js/Upload objects.md)
-   [Ruby](/intl.en-US/SDK Reference/Ruby/Upload objects.md)

## Error codes

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|MissingContentLength|411|The error message returned because the request header is not encoded by using [chunked encoding](https://tools.ietf.org/html/rfc2616#section-3.6.1) or because the Content-Length parameter is not specified.|
|InvalidEncryptionAlgorithmError|400|The error message returned because the specified value of x-oss-server-side-encryption is invalid. Valid values: AES256 and KMS. |
|AccessDenied|403|The error message returned because you do not have permissions to access the specified bucket when you add an object.|
|NoSuchBucket|404|The error message returned because the specified bucket does not exist when you add an object.|
|InvalidObjectName|400|The error message returned because the length of the input object key exceeds 1,023 bytes.|
|InvalidArgument|400|Possible causes:-   The object you want to upload is larger than 5 GB in size.
-   The values of parameters such as x-oss-storage-class are invalid. |
|RequestTimeout|400|The error message returned because Content-Length was specified, but the message body was not sent, or the sent message body was smaller than the specified size. In this case, the server waits until the request times out.|
|Bad Request|400|The error message returned because the Content-MD5 header value specified in a request is different from the MD5 hash calculated by OSS. OSS calculates the MD5 hash of the sent data and compares the calculated MD5 hash with the Content-MD5 value specified in the request.|
|KmsServiceNotEnabled|403|The error message returned because the x-oss-server-side-encryption header is set to KMS, but KMS is not activated in advance.|
|FileAlreadyExists|409|Possible causes:-   An object that has the same name already exists when the header contains `x-oss-forbid-overwrite=true` that is used to prevent overwriting the object that has the same name.
-   The hierarchical namespace feature is enabled for the bucket, and an existing object at the same folder level has the same name exists. |
|FileImmutable|409|The error message returned because you delete or modify data that is being protected. During the protection period, data in the bucket cannot be deleted or modified.|

