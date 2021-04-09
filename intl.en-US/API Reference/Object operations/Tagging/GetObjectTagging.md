# GetObjectTagging

You can call this operation to query the tags of an object.

## Versioning

By default, when you call GetObjectTagging to query the tags of an object, only the tags of the current version of the object are returned. You can specify the versionId parameter in the request to query the tags of a specified version of an object. If the current version of the object is a delete marker, OSS returns 404 Not Found.

## Request structure

```
GET /objectname?tagging
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: Wed, 20 Mar 2019 02:02:36 GMT
Authorization: SignatureValue
```

## Request headers

A GetObjectTagging request contains only common request headers. For more information, see [Common request headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Response headers

The response to a GetObjectTagging request contains only common response headers. For more information, see [Common response headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Response elements

|Element|Type|Description|
|-------|----|-----------|
|Tagging|Container|The container used to store the collection of tags. Child nodes: TagSet |
|TagSet|Container|The collection of tags. Parent nodes: Tagging

Child nodes: Tag |
|Tag|Container|The collection of tags. Parent nodes: TagSet

Child nodes: Key and Value |
|Key|String|The key of the object tag. Parent nodes: Tag

Child nodes: none |
|Value|String|The value of the object tag. Parent nodes: Tag

Child nodes: none |

## Examples

-   Query the tags of an object in an unversioned bucket.

    In this example, an object named objectname is stored in an unversioned bucket named bucketname. A GetObjectTagging request is sent to query the \{a:1\} and \{b:2\} tags of objectname. After the tags of the object are obtained, 200 OK is returned.

    Sample requests

    ```
    GET /objectname?tagging
    Host: BucketName.oss-cn-hangzhou.aliyuncs.com
    Date: Wed, 20 Mar 2019 02:02:36 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:kZoYNv66bsmc10+dcGKw5x2P****
    ```

    Sample responses

    ```
    200 (OK)
    content-length: 209
    server: AliyunOSS
    x-oss-request-id: 5C919F38461FB4282600****
    date: Wed, 20 Mar 2019 02:02:32 GMT
    content-type: application/xml
    <?xml version="1.0" encoding="UTF-8"?>
    <Tagging>
      <TagSet>
        <Tag>
          <Key>a</Key>
          <Value>1</Value>
        </Tag>
        <Tag>
          <Key>b</Key>
          <Value>2</Value>
        </Tag>
      </TagSet>
    </Tagging>
    ```

-   Query the tags of an object in a versioned bucket.

    In this example, an object named objectname is stored in a versioned bucket named bucketname. A GetObjectTagging request is sent to query the \{age:18\} tags of the specified version of objectname. After the tag of the specified version of the object is obtained, 200 OK is returned.

    Sample requests

    ```
    GET /objectname?tagging&versionId=CAEQExiBgID.jImWlxciIDQ2ZjgwODIyNDk5MTRhNzBiYmQwYTZkMTYzZjM0****
    Host: BucketName.oss-cn-hangzhou.aliyuncs.com
    Date: Wed, 24 Jun 2020 08:50:28 GMT
    Authorization: OSS ************:********************
    ```

    Sample responses

    ```
    200 (OK)
    content-length: 161
    server: AliyunOSS
    x-oss-request-id: 5EF313D44506783438F3****
    date: Wed, 24 Jun 2020 08:50:28 GMT
    content-type: application/xml
    x-oss-version-id: CAEQExiBgID.jImWlxciIDQ2ZjgwODIyNDk5MTRhNzBiYmQwYTZkMTYzZjM0****
    <?xml version="1.0" encoding="UTF-8"?>
    <Tagging>
      <TagSet>
        <Tag>
          <Key>age</Key>
          <Value>18</Value>
        </Tag>
      </TagSet>
    </Tagging>
    ```


## SDK

You can use OSS SDKs for the following programming languages to call GetObjectTagging:

-   [Java](/intl.en-US/SDK Reference/Java/Object tagging/Obtain the tags added to an object.md)
-   [Python](/intl.en-US/SDK Reference/Python/Object tagging/Obtain the tags added to an object.md)
-   [Go](/intl.en-US/SDK Reference/Go/Object tagging/Obtain the tags added to an object.md)
-   [C++](/intl.en-US/SDK Reference/C++/Object tagging/Obtain the tags added to an object.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Object tagging/Query the tags added to an object.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Object tagging/Obtain the tags added to an object.md)

## Error codes

|Error code|HTTP status code|Description|
|----------|----------------|-----------|
|FileAlreadyExists|409|The error message returned because the object whose tagging configurations you want to query is a directory within a bucket with the hierarchical namespace feature enabled.|

