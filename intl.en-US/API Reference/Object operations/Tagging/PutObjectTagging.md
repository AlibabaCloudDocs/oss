# PutObjectTagging

You can call this operation to configure tagging or update tagging configurations for an object.

## Usage notes

The object tagging feature uses a key-value pair to tag an object. When you call this operation, take note of the following items:

-   A maximum of 10 tags can be set for an object. Tags added to an object must have unique tag keys.
-   A tag key can be a maximum of 128 bytes in length. A tag value can be a maximum of 256 bytes in length.
-   Tag keys and values are case-sensitive.
-   The key and value of the tag can contain letters, digits, spaces, and the following special characters:

    + - =. \_ : /

    If you configure the tags in the HTTP header and the tags contain any of the preceding characters, you can use URL encoding to encode the keys and values of the tags.

-   The Last-Modified value of an object is not updated when the object tags are changed.

For more information about object tags, see [Configure object tagging](/intl.en-US/Developer Guide/Objects/Manage files/Configure object tagging.md).

## Versioning

By default, the PutObjectTagging operation is called to configure tagging or update tagging configurations for the current version of an object. If the current version of the object is a delete marker, OSS returns 404 Not Found. You can configure tagging or update tagging configurations for a specified version of the object by specifying the versionId parameter.

## Request structure

```
PUT /objectname? tagging
Content-Length: 114
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
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
|Tagging|Container|Yes|The collection of tags. Child nodes: TagSet |
|TagSet|Container|Yes|The collection of tags. Parent nodes: Tagging

Child nodes: Tag |
|Tag|Container|No|The collection of tags. Parent nodes: TagSet

Child nodes: Key and Value |
|Key|String|No|The key of the object tag. Parent nodes: Tag

Child nodes: none |
|Value|String|No|The value of the object tag. Parent nodes: Tag

Child nodes: none |

## Examples

-   Sample requests for objects in an unversioned bucket

    objectname is contained in bucketname that is unversioned. You can send a PUT request to add the \{a:1\} and \{b:2\} tags to objectname. After the two tags are added to the object, 200 OK is returned.

    ```
    PUT /objectname? tagging
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

    Sample success responses

    ```
    200 (OK)
    content-length: 0
    server: AliyunOSS
    x-oss-request-id: 5C8F55ED461FB4A64C00****
    date: Mon, 18 Mar 2019 08:25:17 GMT
    ```

-   Sample requests for objects in a versioned bucket

    objectname is contained in bucketname that is versioned. You can specify versionId in a PUT request to add the \{age:18\} tag to the specified version of objectname. After the tag is added to the specified version of the object, 200 OK is returned.

    ```
    PUT /objectname? tagging&versionId=CAEQExiBgID.jImWlxciIDQ2ZjgwODIyNDk5MTRhNzBiYmQwYTZkMTYzZjM0****
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

    Sample success responses

    ```
    200 (OK)
    content-length: 0
    server: AliyunOSS
    x-oss-request-id: 5EF315A7FBD3EC3232B4****
    date: Wed, 24 Jun 2020 08:58:15 GMT
    x-oss-version-id: CAEQExiBgID.jImWlxciIDQ2ZjgwODIyNDk5MTRhNzBiYmQwYTZkMTYzZjM0****
    ```


## SDKs

You can call PutObjectTagging by using the following OSS SDKs for different programming languages:

-   [Java](/intl.en-US/SDK Reference/Java/Object tagging/Configure object tagging.md)
-   [Python](/intl.en-US/SDK Reference/Python/Object tagging/Configure object tagging.md)
-   [Go](/intl.en-US/SDK Reference/Go/Object tagging/Configure object tagging.md)
-   [C++](/intl.en-US/SDK Reference/C++/Object tagging/Configure object tagging.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Object tagging/Configure object tagging.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Object tagging/Configure object tagging.md)

