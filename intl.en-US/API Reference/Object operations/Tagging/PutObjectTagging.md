# PutObjectTagging

You can call this operation to add tags to an object or update the tags added to the bucket. The object tagging feature uses a key-value pair to tag an object.

## Usage notes

-   You can add up to 10 tags to an object. The tags added to an object must have unique tag keys.
-   A tag key can be up to 128 bytes in length. A tag value can be up to 256 bytes in length.
-   Tag keys and values are case-sensitive.
-   The key and value of a tag can contain letters, digits, spaces, and the following special characters:

    + - = . \_ : /

    If you configure the tags in the HTTP header and the tags contain any of the preceding characters, you can use URL encoding to encode the keys and values of the tags.

-   The Last-Modified value of an object is not updated when the object tags are changed.

For more information about object tags, see [Configure object tagging](/intl.en-US/Developer Guide/Objects/Manage files/Configure object tagging.md).

## Versioning

By default, when you call PutObjectTagging to add tags to an object or update the tags configured for an object, the tags are added to the current version of the object or the tags configured for the current version of the object are updated. You can specify the versionId parameter in the request to add tags to a specified version of an object or update the tags configured for a specified version of an object. If the current version of the object is a delete marker, OSS returns 404 Not Found.

## Request structure

```
PUT /objectname?tagging
Content-Length: 114
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: Mon, 18 Mar 2019 08:25:17 GMT
Authorization: SignatureValue
<Tagging>
  <TagSet>
    <Tag>
      <Key>Key</Key>
      <Value>Value</Value>
    </Tag>
  </TagSet>
</Tagging>            
```

## Request elements

|Element|Type|Required|Description|
|-------|----|--------|-----------|
|Tagging|Container|Yes|The container used to store the collection of tags. Child nodes: TagSet |
|TagSet|Container|Yes|The collection of tags. Parent nodes: Tagging

Child nodes: Tag |
|Tag|Container|No|The collection of tags. Parent nodes: TagSet

Child nodes: Key and Value |
|Key|String|No|The key of the object tag. Parent nodes: Tag

Child nodes: none |
|Value|String|No|The value of the object tag. Parent nodes: Tag

Child nodes: none |

## Examples

-   Add tags to an object in an unversioned bucket.

    In this example, an object named objectname is stored in an unversioned bucket named bucketname. A PutObjectTagging request is sent to add the \{a:1\} and \{b:2\} tags to objectname. After the two tags are added to the object, 200 OK is returned.

    Sample requests

    ```
    PUT /objectname?tagging
    Content-Length: 114
    Host: BucketName.oss-cn-hangzhou.aliyuncs.com
    Date: Mon, 18 Mar 2019 08:25:17 GMT
    Authorization: OSS ***********:*********************
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

    Sample responses

    ```
    200 (OK)
    content-length: 0
    server: AliyunOSS
    x-oss-request-id: 5C8F55ED461FB4A64C00****
    date: Mon, 18 Mar 2019 08:25:17 GMT
    ```

-   Add tags to an object in a versioned bucket.

    In this example, an object named objectname is stored in a versioned bucket named bucketname. A PutObjectTagging request is sent to add the \{age:18\} tags to the specified version of objectname. After the tag is added to the specified version of the object, 200 OK is returned.

    Sample requests

    ```
    PUT /objectname?tagging&versionId=CAEQExiBgID.jImWlxciIDQ2ZjgwODIyNDk5MTRhNzBiYmQwYTZkMTYzZjM0****
    Content-Length: 90
    Host: BucketName.oss-cn-hangzhou.aliyuncs.com
    Date: Wed, 24 Jun 2020 08:58:15 GMT
    Authorization: OSS ************:********************
    <Tagging>
      <TagSet>
        <Tag>
          <Key>age</Key>
          <Value>18</Value>
        </Tag>
      </TagSet>
    </Tagging>
    ```

    Sample responses

    ```
    200 (OK)
    content-length: 0
    server: AliyunOSS
    x-oss-request-id: 5EF315A7FBD3EC3232B4****
    date: Wed, 24 Jun 2020 08:58:15 GMT
    x-oss-version-id: CAEQExiBgID.jImWlxciIDQ2ZjgwODIyNDk5MTRhNzBiYmQwYTZkMTYzZjM0****
    ```


## SDK

You can use OSS SDKs for the following programming languages to call the PutObjectTagging operation:

-   [Java](/intl.en-US/SDK Reference/Java/Object tagging/Configure object tagging.md)
-   [Python](/intl.en-US/SDK Reference/Python/Object tagging/Configure object tagging.md)
-   [Go](/intl.en-US/SDK Reference/Go/Object tagging/Configure object tagging.md)
-   [C++](/intl.en-US/SDK Reference/C++/Object tagging/Configure object tagging.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Object tagging/Configure object tagging.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Object tagging/Configure object tagging.md)

## Error codes

|Error code|HTTP status code|Description|
|----------|----------------|-----------|
|FileAlreadyExists|409|The error message returned because the object whose tagging configurations you want to configure or update is a directory within a bucket with the hierarchical namespace feature enabled.|

