# DeleteObjectTagging

You can call this operation to delete the tags of a specified object.

## Versioning

By default, when you call DeleteObjectTagging to delete the tags of an object, the tags of the current version of the object are deleted. You can specify the versionId parameter in the request to delete the tags of a specified version of an object. If the current version of the object is a delete marker, OSS returns 404 Not Found.

## Request structure

```
DELETE /objectname?tagging
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: Mon, 18 Mar 2019 08:25:17 GMT
Authorization: SignatureValue
```

## Request headers

A DeleteObjectTagging request contains only common request headers. For more information, see [Common request headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Response headers

The response to a DeleteObjectTagging request contains only common response headers. For more information, see [Common response headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Examples

-   Delete the tags of an object in an unversioned bucket.

    In this example, an object named objectname is stored in an unversioned bucket named bucketname. A DeleteObjectTagging request is sent to delete all tags of objectname.

    Sample requests

    ```
    DELETE /objectname?tagging
    Host: BucketName.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 03:00:33 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:kZoYNv66bsmc10+dcGKw5x2P****
    ```

    Sample responses

    ```
    204 (No Content)
    content-length: 0
    server: AliyunOSS
    x-oss-request-id: 5CAC0AD16D0232E2051B****
    date: Tue, 09 Apr 2019 03:00:33 GMT
    ```

-   Delete the tags of an object in a versioned bucket.

    In this example, an object named objectname is stored in a versioned bucket named bucketname. A DeleteObjectTagging request is sent to delete all tags of a specified version of objectname.

    Sample requests

    ```
    DELETE /objectname?tagging&versionId=CAEQExiBgID.jImWlxciIDQ2ZjgwODIyNDk5MTRhNzBiYmQwYTZkMTYzZjM0****
    Host: BucketName.oss-cn-hangzhou.aliyuncs.com
    Date: Wed, 24 Jun 2020 09:01:09 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:kZoYNv66bsmc10+dcGKw5x2P****
    ```

    Sample responses

    ```
    204 (No Content)
    content-length: 0
    server: AliyunOSS
    x-oss-request-id: 5EF3165525D95C3338E8****
    date: Wed, 24 Jun 2020 09:01:09 GMT
    x-oss-version-id: CAEQExiBgID.jImWlxciIDQ2ZjgwODIyNDk5MTRhNzBiYmQwYTZkMTYzZjM0****
    ```


## SDK

You can use OSS SDKs for the following programming languages to call DeleteObjectTagging:

-   [Java](/intl.en-US/SDK Reference/Java/Object tagging/Delete the tags added to an object.md)
-   [Python](/intl.en-US/SDK Reference/Python/Object tagging/Delete the tags added to an object.md)
-   [Go](/intl.en-US/SDK Reference/Go/Object tagging/Delete the tags added to an object.md)
-   [C++](/intl.en-US/SDK Reference/C++/Object tagging/Delete the tags added to an object.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Object tagging/Delete the tags added to an object.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Object tagging/Delete the tags added to an object.md)

## Error codes

|Error code|HTTP status code|Description|
|----------|----------------|-----------|
|FileAlreadyExists|409|The error message returned because the object whose tags you want to delete is a directory in a bucket with the hierarchical namespace feature enabled.|

