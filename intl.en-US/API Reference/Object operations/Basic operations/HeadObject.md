# HeadObject

You can call this operation to query the metadata of an object. The object content is not included in the returned results.

## Versioning

By default, you can call the HeadObject operation to query the metadata of the current version of an object. If the current version of the object is a delete marker, OSS returns 404 Not Found. If you specify a version ID in the request, OSS returns the metadata of the object of the specified version.

## Request structure

```
HEAD /ObjectName HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Request headers

|Header|Type|Required|Description|
|:-----|:---|:-------|:----------|
|If-Modified-Since|String|No|The information return condition. If the specified time is earlier than the last modified time of the object, OSS returns the metadata of the object and 200 OK. If the specified time is equal to or later than the last modified time of the object, OSS returns 304 Not Modified. Default value: null |
|If-Unmodified-Since|String|No|The information return condition. If the specified time is equal to or later than the last modified time of the object, OSS returns the metadata of the object and 200 OK. If the specified time is earlier than the last modified time of the object, OSS returns 412 Precondition Failed. Default value: null |
|If-Match|String|No|The information return condition. If the input ETag value matches the ETag value of the object, OSS returns the metadata of the object and 200 OK. If the input ETag value does not match the ETag value of the object, OSS returns 412 Precondition Failed. Default value: null |
|If-None-Match|String|No|The information return condition. If the imported ETag value does not match the ETag value of the object, OSS returns the metadata of the object and 200 OK. If the imported ETag value matches the ETag value of the object, OSS returns 304 Not Modified. Default value: null |

## Response headers

If the requested object is a symbolic link, the following headers are included in the response:

-   The Content-Length, ETag, and Content-Md5 headers are the metadata of the object to which the symbolic link points.
-   The returned value of Last-Modified uses the last modified time of the symbolic link or the last modified time of the object to which the symbolic link points, whichever one is later.
-   Other response headers are the metadata of the symbolic link.

|Header|Type|Description|
|:-----|:---|:----------|
|x-oss-meta-\*|String|The user metadata that is prefixed with x-oss-meta-. If you specify headers prefixed with x-oss-meta- in the PutObject request, the user metadata of the object is included in the response.|
|Custom headers that are not prefixed with x-oss-meta-|String|If you specify headers that are not prefixed with x-oss-meta in the PutObject request to configure the user metadata of the object, the headers are included in the response. Example: x-oss-persistent-headers: key1:base64\_encode\(value1\),key2:base64\_encode\(value2\)....|
|x-oss-server-side-encryption|String|This header is included in the response if the object is encrypted by using the server-side encryption algorithm based on entropy encoding. The value of this header is the algorithm used to encrypt the object at the server side.|
|x-oss-server-side-encryption-key-id|String|This header is included in the response if the object is encrypted by using Key Management Service \(KMS\) at the server side. The value of this header is the customer master key \(CMK\) ID that is used to encrypt the object.|
|x-oss-storage-class|String|The storage class of the object. Valid values: -   Standard: provides highly reliable, highly available, and high-performance object storage services that can handle frequent data access.
-   IA: is suitable for long-term storage of data that is infrequently accessed. Data that is accessed once or twice per month on average falls into this category.
-   Archive: is suitable for long-term storage of data that is infrequently accessed. Data that you want to store for at least six months falls into this category. The data takes up to one minute to restore before the data can be read.
-   Cold Archive: is suitable for long-term storage of data that is rarely accessed. |
|x-oss-object-type|String|The type of the object. Valid values: -   Normal: The object is uploaded by using PutObject or created by using CreateDirectory.
-   Appendable: The object is uploaded by using AppendObject.
-   Multipart: The object is uploaded by using MultipartUpload. |
|x-oss-next-append-position|String|The position for the next append operation. If the type of the object is Appendable, this header is included in the response.|
|x-oss-hash-crc64ecma|String|The 64-bit CRC value of the object. This value is calculated based on the ECMA-182 standard. If an object is created before OSS supports CRC-64, this header may not be included in the response when you call HeadObject to query the metadata of the object. |
|x-oss-expiration|String|The lifecycle information about the object. If lifecycle rules are configured for the object, this header is included in the response . This header contains two parameters: expiry-date that indicates the expiration date of the object, and rule-id that indicates the ID of the matched lifecycle rule.|
|x-oss-restore|String|The status of the object when you restore an object. If the storage class of the bucket is Archive and a RestoreObject request is submitted, this header is included in the response. -   If the RestoreObject request is not submitted or expires, this header is excluded from the response.
-   If the RestoreObject request is submitted and the restoration is incomplete, the returned value of this header is ongoing-request="true".
-   If the RestoreObject request is submitted and the restoration is complete, the returned value of this header is in the following format: ongoing-request="false", expiry-date="Sun, 16 Apr 2017 08:12:33 GMT", in which the expiry-date parameter value is the date before which the restored object can be read. |
|x-oss-process-status|String|The result of an event notification that is performed for the object. If you create an OSS event notification rule by using Message Service \(MNS\) and an operation performed on the object triggers the rule, this header is included in the response. The returned value of this header is Base64-encoded and in the JSON format.|
|x-oss-request-charged|String|This header is included in the response if pay-by-requester is enabled for the bucket and the requester is not the bucket owner. The value of this header is requester.|
|Content-Md5|String|-   This header is included in the response if the object type is Normal. The value of this header is calculated based on the following process: 1. Calculate the MD5 hash of the message content based on RFC 1864 to obtain a 128-bit digit. Do not include headers to calculate the MD5 hash. 2. Encode the digit by using Base64.
-   If the object type is Multipart or Appendable, this header is not included in the response. |
|Last-Modified|String|The time when the object was last modified. The time is in GMT specified in HTTP/1.1.|
|Access-Control-Allow-Origin|String|The allowed origins in cross-origin resource sharing \(CORS\). If a CORS rule is configured for the bucket that stores the object and the Origin header in the request meets the CORS rule, this header is included in the response.|
|Access-Control-Allow-Methods|String|The allowed methods in CORS. If a CORS rule is configured for the bucket that stores the object and the Access-Control-Request-Method header in the request meets the CORS rule, this header is included in the response.|
|Access-Control-Max-Age|String|The maximum cache period in CORS. If a CORS rule is configured for the bucket that stores the object and the request meets the CORS rule, this header is included in the response.|
|Access-Control-Allow-Headers|String|The allowed headers in CORS. If a CORS rule is configured for the bucket that stores the object and the request meets the CORS rule, this header is included in the response.|
|Access-Control-Expose-Headers|String|The list of header fields that can be accessed by JavaScript applications on the client. If a CORS rule is configured for the bucket that stores the object and the request meets the CORS rule, this header is included in the response.|
|x-oss-tagging-count|String|The number of tags added to the object. This header is included in the response only when you have the read permissions on tags.|

## Examples

-   Perform HeadObject on an unversioned object

    Sample requests

    ```
    HEAD /oss.jpg HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 7 Aug 2020 07:32:52 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:JbzF2LxZUtanlJ5dLA092wpD****
    ```

    Sample success responses when HeadObject is performed on an object

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 559CC9BDC755F95A6448****
    x-oss-object-type: Normal
    x-oss-storage-class: Archive
    Date: Fri, 7 Aug 2020 07:32:52 GMT
    Last-Modified: Fri, 24 Feb 2012 06:07:48 GMT
    ETag: "fba9dede5f27731c9771645a3986****"
    Content-Length: 344606
    Content-Type: image/jpg
    Connection: keep-alive
    Server: AliyunOSS
    ```

    Sample success responses when HeadObject is performed on a directory

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 559CC9BDC755F95A6448****
    x-oss-object-type: Normal
    x-oss-storage-class: Standard
    Date: Wed, 31 Mar 2021 07:32:52 GMT
    Last-Modified: Tue, 30 Mar 2021 06:07:48 GMT
    ETag: "null"
    Content-Length: 0
    Content-Type: application/x-directory
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   Query the metadata of an object of the specified version in a versioned bucket

    Sample requests

    ```
    HEAD /example?versionId=CAEQNRiBgICb8o6D0BYiIDNlNzk5NGE2M2Y3ZjRhZTViYTAxZGE0ZTEyMWYy****
    Host: versioning-test.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 7 Aug 2020 06:27:12 GMT
    Authorization: OSS ryghu9rp3mqp1ya:RvyjGvKxaUhdF0ibyEwX5mOM****
    ```

    Sample responses

    ```
    HTTP/1.1 200 OK
    x-oss-versionId: CAEQNRiBgICb8o6D0BYiIDNlNzk5NGE2M2Y3ZjRhZTViYTAxZGE0ZTEyMWYy****
    x-oss-request-id: 5CAC3B40B7AEADE01700****
    x-oss-object-type: Normal
    x-oss-storage-class: Archive
    Date: Fri, 7 Aug 2020 06:27:12 GMT
    Last-Modified: Fri, 7 Aug 2020 06:27:12 GMT
    ETag: "A082B659EF78733A5A042FA253B1****"
    Content-Length: 481827
    Content-Type: text/html
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   Query the metadata of the current object version in a versioned bucket

    Sample requests

    ```
    HEAD /example HTTP/1.1    
    Host: versioning-test.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 7 Aug 2020 06:27:12 GMT
    Authorization: OSS ryghu9rp3mqp1ya:RvyjGvKxaUhdF0ibyEwX5mOM****
    ```

    Sample responses

    ```
    HTTP/1.1 200 OK
    x-oss-versionId: CAEQMxiBgMCZov2D0BYiIDY4MDllOTc2YmY5MjQxMzdiOGI3OTlhNTU0ODIx****
    x-oss-request-id: 5CAC3B40B7AEADE01700****
    x-oss-object-type: Normal
    x-oss-storage-class: Archive
    Date: Fri, 7 Aug 2020 06:27:12 GMT
    Last-Modified: Fri, 7 Aug 2020 06:27:12 GMT
    ETag: "3663F7B0B9D3153F884C821E7CF4****"
    Content-Length: 485859
    Content-Type: text/html
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   Query the metadata of an object for which restoration is in progress

    Sample requests

    ```
    HEAD /oss.jpg HTTP/1.1
    Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 7 Aug 2020 07:32:52 GMT
    Authorization: OSS e1Unnbm1rgdnpI:KKxkdNrUBu2t1kqlDh0MLbDb****
    ```

    Sample responses

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 58F71A164529F18D7F00****
    x-oss-object-type: Normal
    x-oss-storage-class: Archive
    x-oss-restore: ongoing-request="true"
    Date: Fri, 7 Aug 2020 07:32:52 GMT
    Last-Modified: Fri, 7 Aug 2020 06:07:48 GMT
    ETag: "fba9dede5f27731c9771645a3986****"
    Content-Length: 344606
    Content-Type: image/jpg
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   Query the metadata of an object for which the RestoreObject request is submitted and restoration is complete

    Sample requests

    ```
    HEAD /oss.jpg HTTP/1.1
    Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 7 Aug 2020 09:35:51 GMT
    Authorization: OSS e1Unnbm1rgdnpI:21qtGJ+ykDVmdu6O6FMJnn+W****
    ```

    Sample responses

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 58F725344529F18D7F00****
    x-oss-object-type: Normal
    x-oss-storage-class: Archive
    x-oss-restore: ongoing-request="false", expiry-date="Sun, 16 Apr 2017 08:12:33 GMT"
    Date: Fri, 7 Aug 2020 09:35:51 GMT
    Last-Modified: Fri, 7 Aug 2020 06:07:48 GMT
    ETag: "fba9dede5f27731c9771645a3986****"
    Content-Length: 344606
    ```

-   Query the metadata of an object that is encrypted by using SSE-OSS at the server side

    Sample requests

    ```
    HEAD /oss.jpg HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 7 Aug 2020 07:32:52 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:JbzF2LxZUtanlJ5dLA092wpD****
    ```

    Sample responses

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 559CC9BDC755F95A6448****
    x-oss-object-type: Normal
    x-oss-storage-class: Archive
    x-oss-server-side-encryption: AES256
    Date: Fri, 7 Aug 2020 07:32:52 GMT
    Last-Modified: Fri, 7 Aug 2020 06:07:48 GMT
    ETag: "fba9dede5f27731c9771645a3986****"
    Content-Length: 344606
    Content-Type: image/jpg
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   Query the metadata of an object that is encrypted by using server-side encryption method SSE-KMS

    Sample requests

    ```
    HEAD /oss.jpg HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 7 Aug 2020 07:32:52 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:JbzF2LxZUtanlJ5dLA092wpD****
    ```

    Sample responses

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 559CC9BDC755F95A64485981
    x-oss-object-type: Normal
    x-oss-storage-class: Archive
    x-oss-server-side-encryption: KMS
    x-oss-server-side-encryption-key-id: 9468da86-3509-4f8d-a61e-6eab1eac****
    Date: Fri, 7 Aug 2020 07:32:52 GMT
    Last-Modified: Fri, 7 Aug 2020 06:07:48 GMT
    ETag: "fba9dede5f27731c9771645a3986****"
    Content-Length: 344606
    Content-Type: image/jpg
    Connection: keep-alive
    Server: AliyunOSS
    ```


## Error codes

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|Not Found|404|The error message returned because the requested object does not exist.|
|SymlinkTargetNotExist|404|The error message returned because the requested object is a symbolic link.|
|InvalidTargetType|400|The error message returned because the requested object is a symbolic link that points to another symbolic link.|
|Not Modified|304|Possible causes:-   The If-Modified-Since header is specified in the request, but the requested object has not been modified since the time specified in the request.
-   The If-None-Match header is specified in the request, and the ETag value provided in the request is the same as the ETag value of the requested object. |
|Precondition Failed|412|Possible causes:-   The If-Unmodified-Since header is specified in the request, but the specified time is earlier than the modified time of the requested object.
-   The If-Match header is specified in the request, but the ETag value provided in the request is different from the ETag value of the requested object. |

