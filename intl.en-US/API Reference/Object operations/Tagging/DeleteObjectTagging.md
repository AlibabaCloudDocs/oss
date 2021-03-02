# DeleteObjectTagging

You can call this operation to delete the tags of a specified object.

## Versioning

By default, the DeleteObjectTagging operation is called to delete the tags of the current version of an object. If the current version of the object is a delete marker, OSS returns 404 Not Found. You can delete the tags of a specified version of an object by specifying the versionId parameter.

## Request structure

```
DELETE /objectname? tagging
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: Mon, 18 Mar 2019 08:25:17 GMT
Authorization: SignatureValue
```

## Request headers

A DeleteObjectTagging request contains only common request headers. For more information, see [Common request headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Response headers

The response to a DeleteObjectTagging request contains only common response headers. For more information, see [Common response headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Examples

-   Sample request for deleting the tags of an object in an unversioned bucket

    You can send this request to delete all tags of objectname in bucketname that is unversioned.

    ```
    DELETE /objectname? tagging
    Host: BucketName.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 03:00:33 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:kZoYNv66bsmc10+dcGKw5x2P****
    ```

    Sample response

    ```
    204 (No Content)
    content-length: 0
    server: AliyunOSS
    x-oss-request-id: 5CAC0AD16D0232E2051B****
    date: Tue, 09 Apr 2019 03:00:33 GMT
    ```

-   Sample request for deleting the tags of an object in a versioned bucket

    You can send this request to delete all tags of the specified version of objectname in bucketname that is versioned. The version is specified by versionId.

    ```
    DELETE /objectname? tagging&versionId=CAEQExiBgID.jImWlxciIDQ2ZjgwODIyNDk5MTRhNzBiYmQwYTZkMTYzZjM0****
    Host: BucketName.oss-cn-hangzhou.aliyuncs.com
    Date: Wed, 24 Jun 2020 09:01:09 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:kZoYNv66bsmc10+dcGKw5x2P****
    ```

    Sample response

    ```
    204 (No Content)
    content-length: 0
    server: AliyunOSS
    x-oss-request-id: 5EF3165525D95C3338E8****
    date: Wed, 24 Jun 2020 09:01:09 GMT
    x-oss-version-id: CAEQExiBgID.jImWlxciIDQ2ZjgwODIyNDk5MTRhNzBiYmQwYTZkMTYzZjM0****
    ```


## SDK

You can call DeleteObjectTagging by using OSS SDKs for the following programming languages:

-   [Java](/intl.en-US/SDK Reference/Java/Object tagging/Delete the tags added to an object.md)
-   [Python](/intl.en-US/SDK Reference/Python/Object tagging/Delete the tags added to an object.md)
-   [Go](/intl.en-US/SDK Reference/Go/Object tagging/Delete the tags added to an object.md)
-   [C++](/intl.en-US/SDK Reference/C++/Object tagging/Delete the tags added to an object.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Object tagging/Delete the tags added to an object.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Object tagging/Delete the tags added to an object.md)

