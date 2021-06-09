# CopyObject

You can call this operation to copy objects within a bucket or between buckets in the same region.

## Versioning

By default, the value of the `x-oss-copy-source` header in a CopyObject request specifies the current version of the object to copy. You can add a version ID to the value of `x-oss-copy-source` to copy a specified version of the object. If the specified version of the object to copy is a delete marker, Object Storage Service \(OSS\) returns 404 Not Found, which indicates that the object does not exist.

To recover a previous version of an object to the current version, you can copy the previous version of the object to the bucket in which the object is stored. OSS then stores the previous version of the object as the current version.

If versioning is enabled for the destination bucket, OSS generates a unique version ID for the object copied to the destination bucket. The version ID is returned as the `x-oss-version-id` header in the response. If versioning is not enabled or suspended for the destination bucket, OSS generates a version whose ID is null for the object copied to the destination bucket, and overwrites the original version whose ID is null.

## Usage notes

-   CopyObject can be used to copy only objects smaller than 1 GB in size. To copy objects larger than 1 GB in size, we recommend that you call the [UploadPartCopy](/intl.en-US/API Reference/Object operations/Multipart upload/UploadPartCopy.md) operation.

    To call CopyObject or UploadPartCopy, you must have read permissions on the source object.

-   If you call CopyObject to copy an object to the same unversioned bucket in which the object is stored, only the metadata of the object is updated. The content of the object is not copied.
-   Append objects stored in versioned buckets cannot be copied.
-   If the source object is a symbolic link, only the symbolic link is copied. The object to which the symbolic link points is not copied.
-   Directories in buckets for which the hierarchical namespace feature is enabled cannot be copied.

## Billable items

-   Each time when you call the CopyObject operation, fees are incurred for a GET request to the source bucket and a PUT request to the destination bucket.
-   The storage usage of the destination bucket increases when you call the CopyObject operation.
-   If you call the CopyObject operation to modify the storage class of an object by overwriting the object, corresponding fees are incurred. For example, if you call CopyObject to convert an IA object to a Standard object by overwriting the IA object 10 days after the IA object is created, you are charged for the entire minimum storage period of 30 days. For more information about storage fees, see [Storage fees](/intl.en-US/Pricing/Billing items and methods/Storage fees.md).

## Request structure

```
PUT /DestObjectName HTTP/1.1
Host: DestBucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
x-oss-copy-source: /SourceBucketName/SourceObjectName
```

## Request headers

All headers in a CopyObject request start with x-oss-. Therefore, these headers must be added to the signature string.

|Header|Type|Required|Example|Description|
|:-----|:---|:-------|-------|:----------|
|x-oss-forbid-overwrite|String|No|true|Specifies whether the CopyObject operation overwrites the object with the same name. The x-oss-forbid-overwrite request header does not take effect when versioning is enabled or suspended for the destination bucket. In this case, the CopyObject operation overwrites the existing object that has the same name as that of the object you want to copy. -   If x-oss-forbid-overwrite is not specified or the value of x-oss-forbid-overwrite is set to false, an existing object that has the same name as that of the object you want to copy is overwritten.
-   If the value of x-oss-forbid-overwrite is set to true, an existing object that has the same name as that of the object you want to copy cannot be overwritten.

If you specify the x-oss-forbid-overwrite request header, the queries per second \(QPS\) performance of OSS may be degraded. If you want to specify the x-oss-forbid-overwrite header in a large number of requests \(QPS greater than 1,000\), submit a ticket.

Default value: false. |
|x-oss-copy-source|String|Yes|/oss-example/oss.jpg|The path of the source object. Default value: null |
|x-oss-copy-source-if-match|String|No|5B3C1A2E053D763E1B002CC607C5\*\*\*\*|If the ETag value of the source object is the same as the ETag value specified in the request, OSS copies the object and returns 200 OK. Default value: null |
|x-oss-copy-source-if-none-match|String|No|5B3C1A2E053D763E1B002CC607C5\*\*\*\*|If the ETag value of the source object is different from the ETag value specified in the request, OSS copies the object and returns 200 OK. Default value: null |
|x-oss-copy-source-if-unmodified-since|String|No|2019-04-09T07:01:56.000Z|If the time specified in the request is the same as or later than the modified time of the object, OSS copies the object and returns 200 OK. Default value: null |
|x-oss-copy-source-if-modified-since|String|No|2019-04-09T07:01:56.000Z|If the source object is modified after the time specified in the request, OSS copies the object. Default value: null |
|x-oss-metadata-directive|String|No|COPY|The method used to configure the metadata of the destination object. Default value: COPY. -   COPY: The metadata of the source object is copied to the destination object.

The x-oss-server-side-encryption attribute of the source object is not copied to the destination object. The x-oss-server-side-encryption parameter in the CopyObject request specifies the method used to encrypt the destination object.

-   REPLACE: The metadata specified in the request is used instead of the metadata of the source object as the metadata of the destination object.

**Note:** If the path of the source object is the same as that of the destination object and versioning is not enabled for the bucket in which the source and destination objects are stored, metadata specified in the CopyObject request is used as the metadata of the destination object regardless of the value of x-oss-metadata-directive. |
|x-oss-server-side-encryption|String|No|AES256|The entropy coding-based encryption algorithm that OSS uses to encrypt an object when you create the object. Valid values: AES256 and KMS.

You must enable Key Management Service \(KMS\) on the console before you can use the KMS encryption algorithm. Otherwise, KmsServiceNotEnabled is returned.

-   If the x-oss-server-side-encryption header is not specified in the CopyObject request, the destination object is not encrypted on the server regardless of whether the source object has been encrypted on the server.
-   If the x-oss-server-side-encryption header is specified in the CopyObject request, the destination object is encrypted on the server after the CopyObject operation is performed regardless of whether the source object is on the server. In addition, the response to a CopyObject request contains the x-oss-server-side-encryption header, the value of which is set to the encryption algorithm of the destination object.

When the destination object is downloaded, the x-oss-server-side-encryption header is included in the response header and its value is set to the algorithm used to encrypt the object. |
|x-oss-server-side-encryption-key-id|String|No|9468da86-3509-4f8d-a61e-6eab1eac\*\*\*\*|The ID of the customer master key \(CMK\) hosted in KMS. This parameter is valid only when x-oss-server-side-encryption is set to KMS. |
|x-oss-object-acl|String|No|private|The ACL of the destination object when the object is created. Default value: default. Valid values:

-   default: The ACL of the object is the same as that of the bucket in which the object is stored.
-   private: The object is a private resource. Only the owner of this object and authorized users have permissions to read and write this object.
-   public-read: The object is a public-read resource. Only the owner of this object and authorized users have permissions to write this object. Other users can only read the object. Exercise caution when you set the ACL of the object to this value.
-   public-read-write: The object is a public-read-write resource. All users have permissions to read and write the object. Exercise caution when you set the ACL of the object to this value.

For more information about ACLs, see [ACL](/intl.en-US/Developer Guide/Data security/Access and control/ACL.md). |
|x-oss-storage-class|String|No|Standard|The storage class of the object. If you specify the storage class when you upload the object, the specified storage class applies regardless of the storage class of the bucket in which the object is stored. For example, if you set x-oss-storage-class to Standard when you upload an object to an IA bucket, the storage class of the uploaded object is Standard.

Valid values:

-   Standard
-   IA
-   Archive
-   ColdArchive

For more information about storage classes, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md). |
|x-oss-tagging|String|No|a:1|The tags of the destination object. You can configure multiple tags for the destination object. Example: TagA=A&TagB=B. **Note:** The tag key and value must be URL-encoded. If a key-value pair does not contain an equal sign \(=\), the tag value is considered to be an empty string. |
|x-oss-tagging-directive|String|No|Copy|The method used to configure tags for the destination object. Default value: Copy. Valid values: -   Copy: The tags of the source object are copied to the destination object.
-   Replace: The tags specified in the request instead of the tags of the source object are configured for the destination object. |

For more information about the common headers included in CopyObject requests such as Host and Date, see [Common request headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Response headers

The response headers involved in this API operation contain only common response headers. For more information, see [Common response headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Response elements

|Element|Type|Example|Description|
|:------|:---|-------|:----------|
|CopyObjectResult|String|N/A|The result of the CopyObject operation. Default value: null |
|ETag|String|5B3C1A2E053D763E1B002CC607C5\*\*\*\*|The ETag value of the destination object. Parent nodes: CopyObjectResult |
|LastModified|String|Fri, 24 Feb 2012 07:18:48 GMT|The time when the destination object was last modified. Parent nodes: CopyObjectResult |

## Examples

-   Copy an object in an unversioned bucket.

    Sample requests

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

    Sample responses

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
    <?xml version="1.0" encoding="UTF-8"?>
    <CopyObjectResult>
      <ETag>"F2064A169EE92E9775EE5324D0B1****"</ETag>
      <LastModified>2018-12-28T09:41:56.000Z</LastModified>
    </CopyObjectResult>
    ```

-   Copy an object in a versioned bucket without specifying a version ID.

    Sample requests

    ```
    PUT /dest-object-example HTTP/1.1
    Host: versioning-copy.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 03:45:32 GMT
    Authorization: OSS qeyxjc9arppwa0t:QqwOjq7U7j04NVpPqdfcVk0I****
    x-oss-copy-source: /versioning-copy-source/source-object
    ```

    Sample responses

    `x-oss-copy-source-version-id` in the sample response indicates the ID of the copied version of the source object. In this example, x-oss-copy-source-version-id indicates the current version of the source object. `x-oss-version-id` indicates the version ID of the destination object.

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
    <?xml version="1.0" encoding="UTF-8"?>
    <CopyObjectResult>
      <ETag>"C81E728D9D4C2F636F067F89CC14****"</ETag>
      <LastModified>2019-04-09T03:45:32.000Z</LastModified>
    </CopyObjectResult>
    ```

-   Copy an object in a versioned bucket by specifying a version ID.

    Sample requests

    ```
    PUT /dest-object-example HTTP/1.1
    Host: versioning-copy.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 03:45:32 GMT
    Authorization: OSS qeyxjc9arppwa0t:5qG4DLaHjxDPtpLlf2e8fBfX****
    x-oss-copy-source: /versioning-copy-source/source-object?versionId=CAEQNRiBgICv8uaA0BYiIDliZDc3MTc1NjE5MjRkMDI4ZGU4MTZkYjY1ZDgy****
    ```

    Sample responses

    `x-oss-copy-source-version-id` in the sample response indicates the ID of copied version of the source object. In this example, x-oss-copy-source-version-id indicates the version ID specified by `x-oss-copy-source` in the request. `x-oss-version-id` indicates the version ID of the destination object.

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
    <?xml version="1.0" encoding="UTF-8"?>
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
|Precondition Failed|412|Possible causes:-   The x-oss-copy-source-if-match header is specified in the request, but the ETag value of the source object is different from the ETag value in the request.
-   The x-oss-copy-source-if-unmodified-since header is specified in the request, but the time specified in the request is earlier than the last modified time of the object. |
|Not Modified|304|Possible causes:-   The x-oss-copy-source-if-none-match header is specified in the request, but the ETag value of the source object is the same as the ETag value in the request.
-   The x-oss-copy-source-if-modified-since header is specified in the request, but the source object has not been modified after the time specified in the request. |
|KmsServiceNotEnabled|403|The error message returned because the x-oss-server-side-encryption header is set to KMS, but KMS is not activated in advance.|
|FileAlreadyExists|409|Possible causes:-   An object that has the same name already exists when the header contains `x-oss-forbid-overwrite=true` that prevents OSS from overwriting the object that has the same name.
-   The error message returned because the object that you want to delete is a directory within a bucket for which the hierarchical namespace feature is enabled. |
|FileImmutable|409|The error message returned because the data you want to delete or modify is protected by a retention policy.|

