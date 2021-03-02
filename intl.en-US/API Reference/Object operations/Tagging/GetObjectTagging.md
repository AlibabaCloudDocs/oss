# GetObjectTagging

You can call this operation to query the tags of an object.

## Versioning

By default, the GetObjectTagging operation is called to query the tags of the current version of an object. If the current version of the object is a delete marker, OSS returns 404 Not Found. You can query the tags of a specified version of an object by specifying the versionId parameter.

## Request structure

```
GET /objectname? tagging
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
|Tagging|Container|The collection of tags. Child node: TagSet |
|TagSet|Container|The collection of tags. Parent node: Tagging

Child node: Tag |
|Tag|Container|The collection of tags. Parent nodes: TagSet

Child nodes: Key and Value |
|Key|String|The key of the object tag. Parent nodes: Tag

Child node: none |
|Value|String|The value of the object tag. Parent nodes: Tag

Child node: none |

## Examples

-   Sample request for querying the tags of an object in an unversioned bucket

    You can send this request to query the tags \{a:1\} and \{b:2\} of objectname in bucketname that is unversioned. After the two object tags are obtained, 200 OK is returned.

    ```
    GET /objectname? tagging
    Host: BucketName.oss-cn-hangzhou.aliyuncs.com
    Date: Wed, 20 Mar 2019 02:02:36 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:kZoYNv66bsmc10+dcGKw5x2P****
    ```

    Sample response

    ```
    200 (OK)
    content-length: 209
    server: AliyunOSS
    x-oss-request-id: 5C8F55ED461FB4A64C00****
    date: Wed, 20 Mar 2019 02:02:32 GMT
    content-type: application/xml
    <? xml version="1.0" encoding="UTF-8"? >
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

-   Sample requests for querying the tags of an object in a versioned bucket

    You can send this request to query the tag \{age:18\} of the specified version of objectname in bucketname that is versioned. The version is specified by versionId. After the tag of the specified version of the object is obtained, 200 OK is returned.

    ```
    GET /objectname? tagging&versionId=CAEQExiBgID.jImWlxciIDQ2ZjgwODIyNDk5MTRhNzBiYmQwYTZkMTYzZjM0****
    Host: BucketName.oss-cn-hangzhou.aliyuncs.com
    Date: Wed, 24 Jun 2020 08:50:28 GMT
    Authorization: OSS ************:********************
    ```

    Sample response

    ```
    200 (OK)
    content-length: 161
    server: AliyunOSS
    x-oss-request-id: 5EF313D44506783438F3****
    date: Wed, 24 Jun 2020 08:50:28 GMT
    content-type: application/xml
    x-oss-version-id: CAEQExiBgID.jImWlxciIDQ2ZjgwODIyNDk5MTRhNzBiYmQwYTZkMTYzZjM0****
    <? xml version="1.0" encoding="UTF-8"? >
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

You can call GetObjectTagging by using OSS SDKs for the following programming languages:

-   [Java](/intl.en-US/SDK Reference/Java/Object tagging/Obtain the tags added to an object.md)
-   [Python](/intl.en-US/SDK Reference/Python/Object tagging/Obtain the tags added to an object.md)
-   [Go](/intl.en-US/SDK Reference/Go/Object tagging/Obtain the tags added to an object.md)
-   [C++](/intl.en-US/SDK Reference/C++/Object tagging/Obtain the tags added to an object.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Object tagging/Query the tags added to an object.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Object tagging/Obtain the tags added to an object.md)

