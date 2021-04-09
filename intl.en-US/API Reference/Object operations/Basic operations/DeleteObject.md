# DeleteObject

You can call this operation to delete an object.

## Usage notes

-   To delete an object, you must have write permissions on the object.
-   HTTP status code 204 is returned when the DeleteObject operation succeeds, regardless of whether the object exists.
-   If the object you want to delete is a symbolic link, the DeleteObject operation deletes only the symbolic link but not the content to which the link points.
-   The DeleteObject operation cannot be used to delete directories in a bucket for which the hierarchical namespace feature is enabled.

## Versioning

When you call DeleteObject to delete an object from a versioned bucket, you must determine whether to specify a version ID in the request.

-   Delete an object without specifying a version ID \(temporary deletion\)

    By default, if you do not specify the version ID of the object you want to delete in the request, OSS does not delete the current version of the object and instead add a delete marker to the object as the new current version. In this case, if you perform the GetObject operation on the object without specifying a version ID in the request, OSS identifies that the current version of the object is a delete marker and returns `404 Not Found`. In addition, the `x-oss-delete-marker=true` header and the `x-oss-version-id` header that indicates the version ID of the created delete marker are included in the response.

    The value of the `x-oss-delete-marker` header in the response is true, which indicates that the `x-oss-version-id` is the version ID of a delete marker.

-   Delete an object by specifying a version ID \(permanently deletion\)

    If you specify the version ID of the object you want to delete in the request, OSS permanently deletes the version specified by the `versionId` field in the `params` parameter. To delete a version whose ID is null, add `params['versionId'] = "null"` in `params` in the request. OSS identifies the string "null" as the ID of the version to delete and deletes the version whose ID is null.


## Request structure

```
DELETE /ObjectName HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: Wed, 02 Jan 2019 13:28:38 GMT
Authorization: SignatureValue
```

## Request headers

A DeleteObject request contains only common request headers. For more information, see [Common request headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Response headers

|Header|Type|Example|Description|
|:-----|:---|-------|:----------|
|x-oss-delete-marker|Boolean|true|Indicates whether the object is a delete marker. -   If you do not specify the version ID of the object that you want to delete in the DeleteObject request, OSS creates a delete marker as the current version of the object and includes this header with a value of true in the response.
-   If you specify the version ID of the object you want to delete in the DeleteObject request and the specified version is a delete marker, OSS includes this header in the response and the value of this header is true.

Valid value: true|
|x-oss-version-id|String|CAEQMxiBgIDh3ZCB0BYiIGE4YjIyMjExZDhhYjQxNzZiNGUyZTI4ZjljZDcz\*\*\*\*|The version ID of the deleted object. -   If you do not specify the version ID of the object that you want to delete in the DeleteObject request, OSS creates a delete marker as the current version of the object and includes this header in the response to indicate the version ID of the created delete marker.
-   If you specify the version ID of the object you want to delete in the DeleteObject request, OSS includes this header in the response to indicate the ID of the deleted object version. |

The response to a DeleteObject request contains only common response headers. For more information, see [Common response headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Examples

-   Delete an object from an unversioned bucket.

    Sample requests

    ```
    DELETE /AK.txt HTTP/1.1
    Host: test.oss-cn-zhangjiakou.aliyuncs.com
    Accept-Encoding: identity
    User-Agent: aliyun-sdk-python/2.6.0(Windows/7/AMD64;3.7.0)
    Accept: text/html
    Connection: keep-alive
    date: Wed, 02 Jan 2019 13:28:38 GMT
    authorization: OSS qn6qrrqxo2oawuk53otfjbyc:zUglwRPGkbByZxm1+y4eyu+NIUs=zV0****
    Content-Length: 0
    ```

    Sample responses

    ```
    HTTP/1.1 204 No Content
    Server: AliyunOSS
    Date: Wed, 02 Jan 2019 13:28:38 GMT
    Content-Length: 0
    Connection: keep-alive
    x-oss-request-id: 5C2CBC8653718B5511EF4535
    x-oss-server-time: 134
    ```

-   Delete an object from a versioned bucket without specifying a version ID.

    In this case, OSS adds a delete marker to the object and includes the `x-oss-delete-marker=true` header in the response.

    Sample requests

    ```
    DELETE /example HTTP/1.1
    Host: versioning-delete.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 04:08:23 GMT
    Authorization: OSS twnetzwjkqr9eq6:z73SSKA6t2tNTP4GuPjPiyV/****
    ```

    Sample responses

    ```
    HTTP/1.1 204 NoContent
    x-oss-delete-marker: true
    x-oss-version-id: CAEQMxiBgIDh3ZCB0BYiIGE4YjIyMjExZDhhYjQxNzZiNGUyZTI4ZjljZDcz****
    x-oss-request-id: 5CAC1AB7B7AEADE01700****
    Date: Tue, 09 Apr 2019 04:08:23 GMT
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   Delete a version of the object from a versioned bucket by specifying the version ID.

    In this case, the specified version of the object is permanently deleted.

    Sample requests

    ```
    DELETE /example?versionId=CAEQOBiBgIDNlJeB0BYiIDAwYjJlNDQ4YjJkMzQxMmY5NTM5N2UzZWNiZTQ2**** HTTP/1.1
    Host: versioning-delete.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 04:11:54 GMT
    Authorization: OSS gb3m2qiwirupd6v:UjOXBmIbJD3qXL+DP1EDNyCI****
    ```

    Sample responses

    ```
    HTTP/1.1 204 No Content
    x-oss-version-id: CAEQOBiBgIDNlJeB0BYiIDAwYjJlNDQ4YjJkMzQxMmY5NTM5N2UzZWNiZTQ2****
    x-oss-request-id: 5CAC1B8AB7AEADE01700****
    Date: Tue, 09 Apr 2019 04:11:54 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   Delete a delete marker from a versioned bucket by specifying the version ID of the delete marker.

    In the following example, the specified version is a delete marker. The `x-oss-delete-marker=true` header is included in the response.

    Sample requests

    ```
    DELETE /example?versionId=CAEQOBiBgIDNlJeB0BYiIDAwYjJlNDQ4YjJkMzQxMmY5NTM5N2UzZWNiZTQ2**** HTTP/1.1
    Host: versioning-delete.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 04:16:25 GMT
    Authorization: OSS jh475i54ffozhoy:4tX6Z+fnhtINhp0g+sRiLEQb****
    ```

    Sample responses

    ```
    HTTP/1.1 204 No Content
    x-oss-delete-marker: true
    x-oss-version-id: CAEQNhiBgIDFtp.B0BYiIDk4NzgwMmU4NDMyOTQyM2NiMDQxOTcxYWNhMjc1****
    x-oss-request-id: 5CAC1C99B7AEADE01700****
    Date: Tue, 09 Apr 2019 04:16:25 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```


## SDK

You can use OSS SDKs for the following programming languages to call DeleteObject:

-   [Java](/intl.en-US/SDK Reference/Java/Manage objects/Delete objects.md)
-   [Python](/intl.en-US/SDK Reference/Python/Manage objects/Delete objects.md)
-   [PHP](/intl.en-US/SDK Reference/PHP/Manage objects/Delete objects.md)
-   [Go](/intl.en-US/SDK Reference/Go/Manage objects/Delete objects.md)
-   [C](/intl.en-US/SDK Reference/C/Manage objects/Delete objects.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Manage objects/Delete objects.md)
-   [Android](/intl.en-US/SDK Reference/Android/Manage objects/Delete an object.md)
-   [iOS](/intl.en-US/SDK Reference/iOS/Manage objects/Delete objects.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Manage objects/Delete objects.md)
-   [Browser.js](/intl.en-US/SDK Reference/Browser.js/Manage objects.md)

## Error codes

|Error code|HTTP status code|Description|
|----------|----------------|-----------|
|FileImmutable|409|The error message returned because you attempt to delete or modify protected data. During the protection period, data in the bucket cannot be deleted or modified.|
|FileAlreadyExists|409|The error message returned because the object that you want to delete is a directory within a bucket for which the hierarchical namespace feature is enabled.|

