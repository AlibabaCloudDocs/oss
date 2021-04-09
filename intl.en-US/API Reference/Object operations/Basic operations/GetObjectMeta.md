# GetObjectMeta

You can call this operation to query the metadata information of an object, which includes ETag, Size, and LastModified. The content of the object is not returned.

**Note:** If the type of the object is a symbolic link, the information about the symbolic link is returned.

## Versioning

By default, GetObjectMeta obtains the metadata of the current version of an object. If the current version of the object is a delete marker, OSS returns 404 Not Found. If you specify a version ID in the request, OSS returns the metadata of the object of the specified version.

## Request structure

```
HEAD /ObjectName?objectMeta HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Response headers

|Header|Type|Description|
|:-----|----|:----------|
|Content-Length|String|The size of the object. |
|ETag|String|The ETag that is created to identify the content of the object when the object is created.

If an object is created by using a PutObject request, the ETag of the object is the MD5 hash of the object content. If an object is created by using another method, the ETag value is the universally unique identifier \(UUID\) of the object content. The ETag value of the object can be used to check whether the object content is modified. However, we recommend that you do not use the ETag of an object as the MD5 hash of the object to check data integrity.

Default value: null |

## Examples

-   Perform GetObjectMeta on an unversioned object

    Sample requests

    ```
    HEAD /oss.jpg?objectMeta HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Wed, 29 Apr 2015 05:21:12 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:CTkuxpLAi4XZ+WwIfNm0Fmgb****
    ```

    Sample success responses when GetObjectMeta is performed on an object

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 559CC9BDC755F95A6448****
    Date: Wed, 29 Apr 2015 05:21:12 GMT
    ETag: "5B3C1A2E053D763E1B002CC607C5****"
    Last-Modified: Fri, 24 Feb 2012 06:07:48 GMT
    Content-Length: 344606
    Connection: keep-alive
    Server: AliyunOSS
    ```

    Sample success responses when GetObjectMeta is performed on a directory

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 559CC9BDC755F95A6448****
    Date: Wed, 31 Mar 2021 05:21:12 GMT
    ETag: "null"
    Last-Modified: Tue, 30 Mar 2021 06:07:48 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   Perform GetObjectMeta on an unversioned object

    Sample requests

    ```
    GET /example?objectMeta&versionId=CAEQNRiBgIDMh4mD0BYiIDUzNDA4OGNmZjBjYTQ0YmI4Y2I4ZmVlYzJlNGVk**** HTTP/1.1
    Host: versioning-test.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 06:24:00 GMT
    Authorization: OSS 5n4nrhyqrcs6ngn:i/M/c36KzrOEA/bBSHLllIAt****
    ```

    Sample success responses

    ```
    HTTP/1.1 200 OK
    x-oss-version-id: CAEQNRiBgIDMh4mD0BYiIDUzNDA4OGNmZjBjYTQ0YmI4Y2I4ZmVlYzJlNGVk****
    x-oss-request-id: 5CAC3A80B7AEADE0170005F6
    Date: Tue, 09 Apr 2019 06:24:00 GMT
    ETag: "1CF5A685959CA2ED8DE6E5F8ACC2****"
    Last-Modified: Tue, 09 Apr 2019 06:24:00 GMT
    Content-Length: 119914
    Connection: keep-alive
    Server: AliyunOSS
    ```


## SDK

You can use OSS SDKs for the following programming languages to call the GetObjectMeta operation:

-   [Java](/intl.en-US/SDK Reference/Java/Manage objects/Manage Object Meta.md)
-   [Python](/intl.en-US/SDK Reference/Python/Manage objects/Manage object metadata.md)
-   [PHP](/intl.en-US/SDK Reference/PHP/Manage objects/Manage object metadata.md)
-   [Go](/intl.en-US/SDK Reference/Go/Manage objects/Manage ACL for an object.md)
-   [C](/intl.en-US/SDK Reference/C/Manage objects/Manage ACL for an object.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Manage objects/Manage Object Meta.md)
-   [iOS](/intl.en-US/SDK Reference/iOS/Manage objects/Overview.md)

## Error codes

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|Not Found|404|The error message returned because the specified object does not exist.|

