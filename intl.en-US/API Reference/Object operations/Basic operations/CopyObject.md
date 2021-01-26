# CopyObject

You can call this operation to copy objects within a bucket or between buckets in the same region.

## Versioning

By default, if the `x-oss-copy-source` header is included in an CopyObject request, the current version of the object is copied. If the current version of the object is a delete marker, OSS returns 404 Not Found, which indicates that the object does not exist. You can specify a version ID in the `x-oss-copy-source` header to copy the specified object version. Delete markers cannot be copied.

You can copy a previous version of an object to the same bucket. The copied previous version becomes the new current version of the object. This way, the object is restored to the specified previous version.

If versioning is enabled for the destination bucket, OSS generates a unique version ID for the new object. The version ID is returned as the `x-oss-version-id` header in the response. If versioning is not enabled or is suspended for the destination bucket, OSS generates a version whose ID is null for the new object and overwrites the original version whose ID is null.

## Usage notes

-   CopyObject supports only objects less than 1 GB in size. To copy objects larger than 1 GB in size, you can call the [UploadPartCopy](/intl.en-US/API Reference/Object operations/Multipart upload/UploadPartCopy.md) operation.

    To use CopyObject or UploadPartCopy, you must have read permissions on the source object.

-   When you call CopyObject to copy an object in an unversioned bucket, if the source object and the destination object are the same object, OSS only modifies the metadata of the source object but not copy the content of the object.
-   Objects created by using the AppendObject operation cannot be copied.
-   If the source object is a symbolic link, only the symbolic link but not the object to which the symbolic link points is copied.

## Billable items

-   Each time you call the CopyObject operation, fees are incurred for a GET request to the source bucket and a PUT request to the destination bucket.
-   Storage usage of the destination bucket is increased when you call the CopyObject operation.
-   If you call the CopyObject operation to modify the storage class of an object by overwriting the object, corresponding fees are incurred. For example, if you call CopyObject to convert an IA object to a Standard object by overwriting the IA object 10 days after the IA object is created, storage fees of 20 days are charged for the IA object because IA objects have a minimum storage period of 30 days. For more information about storage fees, see [Storage fees](/intl.en-US/Pricing/Billing items and methods/Storage fees.md).

## Request structure

```
PUT /DestObjectName HTTP/1.1
Host: DestBucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
x-oss-copy-source: /SourceBucketName/SourceObjectName
```

## Request headers

All headers in a CopyObject request start with x-oss-. Therefore, these headers must be added to the signed signature string.

|Header|Type|Required|Example|Description|
|:-----|:---|:-------|-------|:----------|
|x-oss-forbid-overwrite|String|No|true|Specifies whether the CopyObject operation overwrites the object with the same name. The x-oss-forbid-overwrite request header is invalid when versioning is enabled or suspended for the destination bucket. In this case, the CopyObject operation overwrites the object with the same name.-   By default, if x-oss-forbid-overwrite is not specified, the object with the same name is overwritten.
-   If x-oss-forbid-overwrite is set to true, the object with the same name cannot be overwritten. If x-oss-forbid-overwrite is set to false, the object with the same name can be overwritten.

If you specify the x-oss-forbid-overwrite request header, the queries per second \(QPS\) performance of OSS may be degraded. If you want to use the x-oss-forbid-overwrite request header to perform a large number of operations \(QPS greater than 1,000\), submit a ticket. |
|x-oss-copy-source|String|Yes|/oss-example/oss.jpg|The path of the source object. Default value: null |
|x-oss-copy-source-if-match|String|No|5B3C1A2E053D763E1B002CC607C5\*\*\*\*|If the ETag value of the source object is the same as the ETag value specified in the request, OSS copies the object and returns 200 OK. Otherwise, OSS returns 412 Precondition Failed, indicating that the preprocessing failed. Default value: null |
|x-oss-copy-source-if-none-match|String|No|5B3C1A2E053D763E1B002CC607C5\*\*\*\*|If the ETag value of the source object is different from the ETag value specified in the request, OSS copies the object and returns 200 OK. Otherwise, OSS returns 304 Not Modified, indicating that the preprocessing failed. Default value: null |
|x-oss-copy-source-if-unmodified-since|String|No|2019-04-09T07:01:56.000Z|If the specified time is the same as or later than the modified time of the object, OSS copies the object and returns 200 OK. Otherwise, OSS returns 412 Precondition Failed, indicating that the preprocessing failed. Default value: null |
|x-oss-copy-source-if-modified-since|String|No|2019-04-09T07:01:56.000Z|If the source object is modified after the specified time, OSS copies the object. Otherwise, OSS returns 304 Not Modified, indicating that the preprocessing failed. Default value: null |
|x-oss-metadata-directive|String|No|COPY|The method used to set the metadata of the destination object. Default value: COPY. Valid values: -   COPY: The metadata of the source object is copied to the destination object.

x-oss-server-side-encryption of the source object is not copied to the destination object. The encoding method of the server-side encryption on the destination object is specified by the x-oss-server-side-encryption header in the CopyObject request.

-   REPLACE: The metadata specified in the request instead of the metadata of the source object is used as the metadata of the destination object.

**Note:** If the path of the source object is the same as that of the destination object and versioning is not enabled for the bucket in which the source and destination objects are stored, metadata specified in the CopyObject request is used as the metadata of the destination object regardless of the value of x-oss-metadata-directive. |
|x-oss-server-side-encryption|String|No|AES256|The entropy coding-based encryption algorithm that OSS uses to encrypt a new object at the server side. Valid values: AES256 and KMS. You must activate KMS to use the KMS-based encryption algorithm. Otherwise, the KmsServiceNotEnabled error code is returned.

-   If the x-oss-server-side-encryption header is not specified in the CopyObject request, the destination object is not encrypted on the server regardless of whether the source object has been encrypted on the server.
-   If the x-oss-server-side-encryption header is specified in the CopyObject request, the destination object is encrypted on the server regardless of whether the source object has been encrypted on the server. In this case, the x-oss-server-side-encryption header is included in the response to the CopyObject request. The value of the header is the algorithm used to encrypt the destination object. When the destination object is downloaded, the x-oss-server-side-encryption header is included in the response and its value is the algorithm used to encrypt the object. |
|x-oss-server-side-encryption-key-id|String|No|9468da86-3509-4f8d-a61e-6eab1eac\*\*\*\*|The ID of the customer master key \(CMK\) hosted in KMS. This parameter is valid only when x-oss-server-side-encryption is set to KMS. |
|x-oss-object-acl|String|No|private|The ACL of the destination object when it is created. Valid values: public-read, private, public-read-write, and default

For more information about object ACLs, see [ACL](/intl.en-US/Developer Guide/Data security/Access and control/ACL.md). |
|x-oss-storage-class|String|No|Standard|The storage class of the destination object. Valid values: Standard, IA, Archive, and ColdArchive.

For more information about storage classes, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md).

Supported operations: PutObject, InitiateMultipartUpload, AppendObject, PutObjectSymlink, and CopyObject |
|x-oss-tagging|String|No|a:1|The tags of the destination object. You can set multiple tags for the destination object. Example: TagA=A&TagB=B. **Note:** The tag key and value must be URL-encoded. If a key-value pair does not contain an equal sign \(=\), the tag value is considered to be an empty string. |
|x-oss-tagging-directive|String|No|Copy|The method used to set the tags of the destination object. Default value: Copy. Valid values: -   Copy: The tags of the source object are copied to the destination object.
-   Replace: The tags of the destination object is set to the tags specified in the request instead of the tags of the source object. |

For more information about the common request headers included in CopyObject requests such as Host and Date, see [Common request headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Response headers

For more information about the common headers in the response to a CopyObject request such as Content-Length and Connection, see [Common response headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Response elements

|Element|Type|Example|Description|
|:------|:---|-------|:----------|
|CopyObjectResult|String|N/A|The returned result of the CopyObject operation. Default value: null |
|ETag|String|5B3C1A2E053D763E1B002CC607C5\*\*\*\*|The ETag value of the destination object. Parent nodes: CopyObjectResult |
|LastModified|String|Fri, 24 Feb 2012 07:18:48 GMT|The time when the destination object was last modified. Parent nodes: CopyObjectResult |

## Examples

-   Sample request for copying an object in an unversioned bucket

    ```
    PUT /test%2FAK.txt HTTP/1.1
    Host: tesx.oss-cn-zhangjiakou.aliyuncs.com
    Accept-Encoding: identity
    User-Agent: aliyun-sdk-python/2.6.0(Windows/7/AMD64;3.7.0)
    Accept: text/html
    Connection: keep-alive
    x-oss-copy-source: /test/AK.txt
    date: Fri, 28 Dec 2018 09:41:55 GMT
    authorization: OSS qn6qrrqxo2oawuk53otfjbyc:gmnwPKuu20LQEjd+iPkL259A****
    Content-Length: 0
    ```

    Sample response

    `x-oss-hash-crc64ecma` indicates the 64-bit CRC value of the object. This value is calculated based on the [ECMA-182](http://www.ecma-international.org/publications/standards/Ecma-182.htm) standard. An object generated by the CopyObject operation may not have this value.

    ```
    HTTP/1.1 200 OK
    Server: AliyunOSS
    Date: Fri, 28 Dec 2018 09:41:56 GMT
    Content-Type: application/xml
    Content-Length: 184
    Connection: keep-alive
    x-oss-request-id: 5C25EFE4462CE00EC6D87156
    ETag: "F2064A169EE92E9775EE5324D0B1****"
    x-oss-hash-crc64ecma: 12753002859196105360
    x-oss-server-time: 150
    <? xml version="1.0" encoding="UTF-8"? >
    <CopyObjectResult>
      <ETag>"F2064A169EE92E9775EE5324D0B1****"</ETag>
      <LastModified>2018-12-28T09:41:56.000Z</LastModified>
    </CopyObjectResult>
    ```

-   Sample request for copying an object without specifying a version ID

    ```
    PUT /dest-object-example HTTP/1.1
    Host: versioning-copy.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 03:45:32 GMT
    Authorization: OSS qeyxjc9arppwa0t:QqwOjq7U7j04NVpPqdfcVk0I****
    x-oss-copy-source: /versioning-copy-source/source-object
    ```

    Sample response

    `x-oss-copy-source-version-id` in the sample response indicates the version ID of the source object. In this example, x-oss-copy-source-version-id indicates the current version of the source object. `x-oss-version-id` indicates the version ID of the destination object.

    ```
    HTTP/1.1 200 OK
    x-oss-copy-source-version-id: CAEQNRiBgIC28uaA0BYiIDY5OGIwNmNlNjYyMTRjNTc4N2M2OGNiMjZkZTQ2****
    x-oss-version-id: CAEQNxiBgIDG8uaA0BYiIGZhZDRkZTk5Zjg3YzRhNzdiMWEwZGViNDM1NTFh****
    x-oss-request-id: 5CAC155CB7AEADE01700****
    Content-Type: application/xml
    Content-Length: 184
    Connection: keep-alive
    Date: Tue, 09 Apr 2019 03:45:32 GMT
    Server: AliyunOSS
    <? xml version="1.0" encoding="UTF-8"? >
    <CopyObjectResult>
      <ETag>"C81E728D9D4C2F636F067F89CC14****"</ETag>
      <LastModified>2019-04-09T03:45:32.000Z</LastModified>
    </CopyObjectResult>
    ```

-   Sample request for copying an object by specifying a version ID

    ```
    PUT /dest-object-example HTTP/1.1
    Host: versioning-copy.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 03:45:32 GMT
    Authorization: OSS qeyxjc9arppwa0t:5qG4DLaHjxDPtpLlf2e8fBfX****
    x-oss-copy-source: /versioning-copy-source/source-object? versionId=CAEQNRiBgICv8uaA0BYiIDliZDc3MTc1NjE5MjRkMDI4ZGU4MTZkYjY1ZDgy****
    ```

    Sample response

    `x-oss-copy-source-version-id` in the sample response indicates the version ID of the source object. In this example, x-oss-copy-source-version-id indicates the version ID specified by `x-oss-copy-source` in the request. `x-oss-version-id` indicates the version ID of the destination object.

    ```
    HTTP/1.1 200 OK
    x-oss-copy-source-version-id: CAEQNRiBgICv8uaA0BYiIDliZDc3MTc1NjE5MjRkMDI4ZGU4MTZkYjY1ZDgy****
    x-oss-version-id: CAEQNxiBgMDP8uaA0BYiIDIyNGNhZDQ1M2M3NzRkZThiNzE0N2I3ZDkxOWY4****
    x-oss-request-id: 5CAC155CB7AEADE01700****
    Content-Type: application/xml
    Content-Length: 184
    Connection: keep-alive
    Date: Tue, 09 Apr 2019 03:45:32 GMT
    Server: AliyunOSS
    <? xml version="1.0" encoding="UTF-8"? >
    <CopyObjectResult>
      <ETag>"C4CA4238A0B923820DCC509A6F75****"</ETag>
      <LastModified>2019-04-09T03:45:32.000Z</LastModified>
    </CopyObjectResult>
    ```


## SDK

You can use OSS SDKs for the following programming languages to call the CopyObject operation:

-   [Java](/intl.en-US/SDK Reference/Java/Manage objects/Copy objects.md)
-   [Python](/intl.en-US/SDK Reference/Python/Manage objects/Copy objects.md)
-   [PHP](/intl.en-US/SDK Reference/PHP/Manage objects/Copy objects.md)
-   [Go](/intl.en-US/SDK Reference/Go/Manage objects/Copy objects.md)
-   [C](/intl.en-US/SDK Reference/C/Manage objects/Copy objects.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Manage objects/Copy objects.md)
-   [Android](/intl.en-US/SDK Reference/Android/Manage objects/Copy an object.md)
-   [iOS](/intl.en-US/SDK Reference/iOS/Manage objects/Overview.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Manage objects/Overview.md)
-   [Ruby](/intl.en-US/SDK Reference/Ruby/Manage objects.md)

## Error codes

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|InvalidArgument|400|The error message returned because the values of parameters such as x-oss-storage-class are invalid.|
|Precondition Failed|412|-   The error message returned because the x-oss-copy-source-if-match header is specified in the request, but the ETag value of the source object is different from the ETag value in the request.
-   The error message returned because the x-oss-copy-source-if-unmodified-since header is specified in the request, but the time specified in the request is earlier than the last modified time of the object. |
|Not Modified|304|-   The error message returned because the x-oss-copy-source-if-none-match header is specified in the request, but the ETag value of the source object is the same as the ETag value in the request.
-   The error message returned because the x-oss-copy-source-if-modified-since header is specified in the request, but the source object has not been modified after the time specified in the request. |
|KmsServiceNotEnabled|403|The error message returned because the x-oss-server-side-encryption header is set to KMS, but KMS is not activated in advance.|
|FileAlreadyExists|409|The error message returned because an object with the same object name already exists and the request contains the x-oss-forbid-overwrite header and the value of this header is true.|
|FileImmutable|409|The error message returned because you delete or modify data that is protected. During the protection period, data in the bucket cannot be deleted or modified.|

