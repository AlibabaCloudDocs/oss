# GetObject

You can call this operation to query an object. To perform the GetObject operation, you must have the read permissions on the object.

## Usage notes

-   By default, the GetObject operation supports access over both HTTP and HTTPS. To allow access to a bucket only over HTTPS, set the access method in a bucket policy. For more information, see [Add bucket policies](/intl.en-US/Console User Guide/Upload, download, and manage objects/Add bucket policies.md).
-   If the storage class of the object to obtain is Archive, you must send a RestoreObject request to restore the object before you call the GetObject operation.

## Versioning

By default, only the current version of an object is returned after GetObject is called.

If the version ID of the object is specified in the request, OSS returns the specified version of the object. If the version ID is set to null in the request, OSS returns the object whose version ID is null.

## Request structure

```
GET /ObjectName HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
Range: bytes=ByteRange (optional)
```

If you download an object larger than 100 MB from OSS, transmission may fail due to network errors. You can specify the HTTP Range header to query part of data of the large object.

OSS does not support multiple range parameters. You can query only one range at a time. ByteRange specifies the range of data to query. Unit: bytes. The valid values of ByteRange are from 0 to `object size - 1`. Examples:

-   `Range: bytes=0-499` specifies the first 500 bytes.
-   `Range: bytes=500-999` specifies the second 500 bytes.
-   `Range: bytes=-500` specifies the last 500 bytes.
-   `Range: bytes=500-` specifies data from the 500th byte to the end of the object.
-   `Range: bytes=0-` specifies data from the first byte to the last byte, which is the entire object.

## Request headers

When you initiate a GET request in OSS, you can set headers in the request to customize headers in the response. However, the headers in the response are set to the values specified in the headers in the request only when the request is successful. When the request is successful, 200 OK is returned.

**Note:** When you initiate a GET request in OSS as an anonymous user, you cannot set headers in the request to customize headers in the response.

|Header|Type|Required|Description|
|:-----|:---|:-------|:----------|
|response-content-type|String|No|The content-type header in the response that OSS returns. Default value: null |
|response-content-language|String|No|The content-language header in the response that OSS returns. Default value: null |
|response-expires|String|No|The expires header in the response that OSS returns. Default value: null |
|response-cache-control|String|No|The cache-control header in the response that OSS returns. Default value: null |
|response-content-disposition|String|No|The content-disposition header in the response that OSS returns. Default value: null |
|response-content-encoding|String|No|The content-encoding header in the response that OSS returns. Default value: null |
|Range|String|No|The range of data to be returned. -   If the value of Range is valid, OSS returns the response that includes the total size of the object and the range of data returned. For example, Content-Range: bytes 0~9/44 indicates that the total size of the object is 44 bytes, and the range of data returned is the first 10 bytes.
-   However, if the value of Range is invalid, the entire object is returned, and the response returned by OSS excludes Content-Range.

Default value: null |
|If-Modified-Since|String|No|If the time specified in this header is earlier than the object modified time or does not conform to the standards, OSS returns the object and 200 OK. If the time specified in this header is later than or the same as the object modified time, OSS returns 304 Not Modified. The time must be in GMT. Example: `Fri, 13 Nov 2015 14:47:53 GMT`.

Default value: null |
|If-Unmodified-Since|String|No|If the time specified in this header is the same as or later than the object modified time, OSS returns the object and 200 OK. If the time specified in this header is earlier than the object modified time, OSS returns 412 Precondition Failed. The time is displayed in GMT. Example: `Fri, 13 Nov 2015 14:47:53 GMT`.

You can specify both the If-Modified-Since and If-Unmodified-Since headers in a request.

Default value: null |
|If-Match|String|No|If the ETag specified in the request matches the ETag value of the object, OSS transmits the object and returns 200 OK. If the ETag specified in the request does not match the ETag value of the object, OSS returns 412 Precondition Failed. The ETag value of an object is used to check whether the content of the object has changed. You can check data integrity by using the ETag value.

Default value: null |
|If-None-Match|String|No|If the ETag specified in the request does not match the ETag value of the object, OSS transmits the object and returns 200 OK. If the ETag specified in the request matches the ETag value of the object, OSS returns 304 Not Modified. You can specify both the If-Match and If-None-Match headers in a request.

Default value: null |
|Accept-Encoding|String|No|The encoding type at the client side. If you want an object to be returned in the GZIP format, you must include the Accept-Encoding:gzip header in your request. OSS determines whether to return the object compressed in the GZIP format. OSS evaluates the decision based on the Content-Type header and whether the size of the object is larger than or equal to 1 KB.

**Note:**

-   If an object is compressed in the GZIP format, the response OSS returns does not include the ETag value of the object.
-   OSS supports the following Content-Type values to compress the object in the GZIP format: text/cache-manifest, text/xml, text/plain, text/css, application/javascript, application/x-javascript, application/rss+xml, application/json, and text/json.

Default value: null |

## Response headers

If the requested object is a symbolic link, the content of the object to which the symbolic points is returned. Response headers `Content-Length`, `ETag`, and `Content-Md5` indicate the metadata of the returned object. The returned value of the `Last-Modified` header uses the last modified time of the symbolic link or the last modified time of the object to which the symbolic link points, whichever one is later. Other headers indicate the metadata of the symbolic link.

|Header|Type|Description|
|------|----|-----------|
|x-oss-server-side-encryption|String|If the requested object is encrypted by using a server-side encryption algorithm based on entropy encoding, OSS automatically decrypts the object and returns the decrypted object after OSS receives the GetObject request. OSS includes x-oss-server-side-encryption in the response to indicate the encryption algorithm that is used to encrypt the object on the server.|
|x-oss-tagging-count|String|The number of tags associated with the object. This header is returned only if you have the read permissions on tags.|

## Examples

-   Sample requests for simple GetObject requests

    Sample requests

    ```
    GET /oss.jpg HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 24 Feb 2012 06:38:30 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:UNQDb7GapEgJkcde6OhZ9J*****
    ```

    Sample success responses when GetObject is performed on an object

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 3a8f-2e2d-7965-3ff9-51c875b*****
    x-oss-object-type: Normal
    Date: Fri, 24 Feb 2012 06:38:30 GMT
    Last-Modified: Fri, 24 Feb 2012 06:07:48 GMT
    ETag: "5B3C1A2E0563E1B002CC607C*****"
    Content-Type: image/jpg
    Content-Length: 344606
    Server: AliyunOSS
    [344606 bytes of object data]
    ```

    Sample success responses when GetObject is performed on a directory

    **Note:** When GetObject is performed on a directory, custom response headers such as response-content-type and Range that are included in a GetObject request become invalid.

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 3a8f-2e2d-7965-3ff9-51c875b*****
    x-oss-object-type: Normal
    Date: Wed, 31 Mar 2021 06:38:30 GMT
    Last-Modified: Tue, 30 Mar 2021 06:07:48 GMT
    ETag: "null"
    Content-Type: application/x-directory
    Content-Length: 0
    Server: AliyunOSS
    ```

-   Sample requests that include the Range parameter

    Sample requests

    ```
    GET /oss.jpg HTTP/1.1
    Host:oss-example. oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 28 Feb 2012 05:38:42 GMT
    Range: bytes=100-900
    Authorization: OSS qn6qrrqxo2oawuk5jbyc:qZzjF3DUtd+yK16BdhGtFcC*****
    ```

    Sample success responses

    ```
    HTTP/1.1 206 Partial Content
    x-oss-request-id: 28f6-15ea-8224-234e-c0ce407*****
    x-oss-object-type: Normal
    Date: Fri, 28 Feb 2012 05:38:42 GMT
    Last-Modified: Fri, 24 Feb 2012 06:07:48 GMT
    ETag: "5B3C1A2E05E1B002CC607C*****"
    Accept-Ranges: bytes
    Content-Range: bytes 100-900/344606
    Content-Type: image/jpg
    Content-Length: 801
    Server: AliyunOSS
    [801 bytes of object data]
    ```

-   Sample requests that include custom response headers

    Sample requests

    ```
    GET /oss.jpg?response-expires=Thu%2C%2001%20Feb%202012%2017%3A00%3A00%20GMT& response-content-type=text&response-cache-control=No-cache&response-content-disposition=attachment%253B%2520filename%253Dtesting.txt&response-content-encoding=utf-8&response-content-language=%E4%B8%AD%E6%96%87 HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com:
    Date: Fri, 24 Feb 2012 06:09:48 GMT
    ```

    Sample success responses

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 559CC9BDC75A644*****
    x-oss-object-type: Normal
    Date: Fri, 24 Feb 2012 06:09:48 GMT 
    Last-Modified: Fri, 24 Feb 2012 06:07:48 GMT
    ETag: "5B3C1A2E053D1B002CC607*****"
    Content-Length: 344606
    Connection: keep-alive
    Content-disposition: attachment; filename:testing.txt
    Content-language: Chinese
    Content-encoding: utf-8
    Content-type: text
    Cache-control: no-cache
    Expires: Fri, 24 Feb 2012 17:00:00 GMT
    Server: AliyunOSS
    [344606 bytes of object data]
    ```

-   Sample requests where the requested object is a symbolic link

    Sample requests

    ```
    GET /link-to-oss.jpg HTTP/1.1
    Accept-Encoding: identity
    Date: Tue, 08 Nov 2016 03:17:58 GMT
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Authorization: OSS qn6qrrqxok53otfjbyc:qZzjF3DUtd+yK16BdhGtFc*****
    ```

    Sample success responses

    ```
    HTTP/1.1 200 OK
    Server: AliyunOSS
    Date: Tue, 08 Nov 2016 03:17:58 GMT
    Content-Type: application/octet-stream
    Content-Length: 20
    Connection: keep-alive
    x-oss-request-id: 582143E6A212AD*****
    Accept-Ranges: bytes
    ETag: "8086265EFC021F9A2F09BF4****"
    Last-Modified: Tue, 08 Nov 2016 03:17:58 GMT
    x-oss-object-type: Symlink
    Content-MD5: gIYmXvwCEe0fmi8Jv0Y****
    ```

-   Sample requests where the requested object is restored

    Sample requests

    ```
    GET /oss.jpg HTTP/1.1
    Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
    Date: Sat, 15 Apr 2017 09:38:30 GMT
    Authorization: OSS qn6qrrqxo2o***k53otfjbyc:zUglwRPGkbByZxm1+y4eyu+*****
    ```

    Sample success responses

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 58F723829F29F18D7F00*****
    x-oss-object-type: Normal
    x-oss-restore: ongoing-request="false", expiry-date="Sun, 16 Apr 2017 08:12:33 GMT"
    Date: Sat, 15 Apr 2017 09:38:30 GMT
    Last-Modified: Sat, 15 Apr 2017 06:07:48 GMT
    ETag: "5B3C1A2E0763E1B002CC607C*****"
    Content-Type: image/jpg
    Content-Length: 344606
    Server: AliyunOSS
    [354606 bytes of object data]
    ```

-   Sample requests where the version ID of the requested object is specified

    Sample requests

    ```
    GET /example?versionId=CAEQNhiBgMDJgZCA0BYiIDc4MGZjZGI2OTBjOTRmNTE5NmU5NmFhZjhjYmY0**** HTTP/1.1
    Host: versioning-get.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 02:58:06 GMT
    Authorization: OSS lkojgxic6e:8wcOrEDt4iSxpBPfQW9OJNw*****
    ```

    Sample success responses

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 5CAC0A3EDE0170*****
    x-oss-version-id: CAEQNhiBgM0BYiIDc4MGZjZGI2OTBjOTRmNTE5NmU5NmFhZjhjYmY*****
    x-oss-object-type: Normal
    Date: Tue, 09 Apr 2019 02:58:06 GMT
    Last-Modified: Fri, 22 Mar 2018 08:07:50 GMT
    ETag: "5B3C1A2E053D7002CC607C5A*****"
    Content-Type: text/html
    Content-Length: 362149
    Server: AliyunOSS
    [362149 bytes of object data]
    ```

-   Sample requests where the version ID of the requested object is not specified and the current version is a delete marker

    Sample requests

    ```
    GET /example HTTP/1.1
    Host: versioning-get.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 03:22:33 GMT
    Authorization: OSS duagpvtn35:taVlDvAJMhEumrR+oLMWtQp*****
    ```

    Sample success responses

    ```
    HTTP/1.1 404 Not Found
    x-oss-request-id: 5CAC0FEADE0170*****
    x-oss-delete-marker: true
    x-oss-version-id: CAEQNxiBgyA0BYiIDc4ZDdmNTA2MGViZTRiNjE5NzZlZWM4OWM5OT*****
    Date: Tue, 09 Apr 2019 03:22:33 GMT
    Content-Type: application/xml
    Connection: keep-alive
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <Error>
      <Code>NoSuchKey</Code>
      <Message>The specified key does not exist.</Message>
      <RequestId>5CAC0FEADE0170*****</RequestId>
      <HostId>versioning-get.oss-cn-hangzhou.aliyun*****</HostId>
      <Key>example</Key>
    </Error>
    ```

-   Sample requests where the version ID is set to a delete marker

    Sample requests

    ```
    GET /example?versionId=CAEQMxiBgMCfqaWA0BYiIDliMWI4MGQ0MTVmMjQ3MmE5MDNlMmY4YmFkYTk3**** HTTP/1.1
    Host: versioning-get.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 03:09:44 GMT
    Authorization: OSS tvqm50uz4y:51UaP+wQt5k1RQang/U6Eeq*****
    ```

    Sample success responses

    ```
    HTTP/1.1 405 Method Not Allowed
    x-oss-request-id: 5CAC0CF8DE01700*****
    x-oss-delete-marker: true
    x-oss-version-id: CAEQMxiBgMCfqaWADliMWI4MGQ0MTVmMjQ3MmE5MDNlMmY4YmFkYTk*****
    Allow: DELETE
    Date: Tue, 09 Apr 2019 03:09:44 GMT
    Content-Type: application/xml
    Content-Length: 318
    Connection: keep-alive
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <Error>
      <Code>MethodNotAllowed</Code>
      <Message>The specified method is not allowed against this resource.</Message>
      <RequestId>5CAC0CF8DE0170*****</RequestId>
      <HostId>versioning-get.oss-cn-hangzhou.aliyunc*****</HostId>
      <Method>GET</Method>
      <ResourceType>DeleteMarker</ResourceType>
    </Error>
    ```


## SDK

You can use OSS SDKs for the following programming languages to call the GetObject operation:

-   [Java](/intl.en-US/SDK Reference/Java/Upload objects/Overview.md)
-   [Python](/intl.en-US/SDK Reference/Python/Upload objects/Overview.md)
-   [PHP](/intl.en-US/SDK Reference/PHP/Upload objects/Overview .md)
-   [Go](/intl.en-US/SDK Reference/Go/Upload objects/Overview.md)
-   [C](/intl.en-US/SDK Reference/C/Upload objects/Overview.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Upload objects/Overview.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Upload objects/Overview.md)
-   [Browser.js](/intl.en-US/SDK Reference/Browser.js/Upload objects.md)
-   [Ruby](/intl.en-US/SDK Reference/Ruby/Upload objects.md)

## Error codes

|Error code|HTTP status code|Description|
|----------|----------------|-----------|
|NoSuchKey|404|The error message returned because the specified object does not exist.|
|SymlinkTargetNotExist|404|The error message returned because the requested object is a symbolic link, and the object to which the symbolic link points does not exist.|
|InvalidTargetType|400|The error message returned because the requested object is a symbolic link, and the object to which the symbolic link points is another symbolic link.|
|InvalidObjectState|403|The error message returned because the following scenarios occur when the storage class of the object you want to download is Archive: -   The RestoreObject request for the object was not initiated or timed out.
-   The RestoreObject request for the object has been initiated but the object is not restored. |
|Not Modified|304|Possible causes:-   The If-Modified-Since header is specified in the request, but the requested object has not been modified since the time specified in the request.
-   The If-None-Match header is specified in the request, and the ETag value provided in the request is the same as the ETag value of the requested object. |
|Precondition Failed|412|Possible causes:-   The If-Unmodified-Since header is specified in the request, but the specified time is earlier than the modified time of the requested object.
-   The If-Match header is specified in the request, but the ETag value provided in the request is different from the ETag value of the requested object. |
|Not Found|404|The error message returned because the version ID of the object is not specified in the request and the current version of the object is a delete marker.|
|Method Not Allowed|405|The error message returned because the version ID of the object is specified in the request and the object that has the version ID is a delete marker.|

