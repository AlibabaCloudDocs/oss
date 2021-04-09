# PutObjectACL

You can call this operation to modify the access control list \(ACL\) of an object. Only the bucket owner has permissions to perform this operation, and have the read and write permissions on the object.

## Versioning

By default, the PutObjectACL operation is called to configure ACL only for the current version of an object. You can set ACL for a specified version of the object by specifying the versionId parameter. If the corresponding version of the object is a delete marker, OSS returns 404 Not Found.

## ACL description

When you call the PutObjectACL operation, you can set the `x-oss-object-acl` header in the PUT request to configure ACL for an object. The following table describes the four ACLs that can be set for an object.

|Parameter|Description|
|:--------|:----------|
|private|The object is a private resource. Only the owner of this object has the permissions to read or write this object.|
|public-read|The objetc is a public-read resource. Only the owner of this object has the permissions to read and write this object. Other users have only the permissions to read this object.|
|public-read-write|The object is a public read/write resource. All users have the permissions to read and write this object.|
|default|The ACL of the object is the same as that of the bucket that stores the object.|

**Note:**

-   The object ACL takes precedence over its bucket ACL. For example, the bucket ACL is private, but the object ACL is public-read-write. All users can access the object, even if the bucket ACL is private. If an object has no ACL configured, its bucket ACL applies.
-   The read operations on an object include GetObject, HeadObject, and CopyObject and UploadPartCopy that read the source object. The write operations on an object include PutObject, PostObject, AppendObject, DeleteObject, DeleteMultipleObjects, and CompleteMultipartUpload and CopyObject that write the destination object.
-   When you write an object, you can also include the x-oss-object-acl header in the request to configure ACL for the object. For example, you can include the x-oss-object-acl header in the request to configure ACL for an object when you call PutObject.

## Request structure

```
PUT /ObjectName?acl HTTP/1.1
x-oss-object-acl: Permission
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Examples

-   Upload an object to a versioning disabled bucket

    Sample requests

    ```
    PUT /test-object?acl HTTP/1.1
    x-oss-object-acl: public-read
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Wed, 29 Apr 2015 05:21:12 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:KU5h8YMUC78M30dXqf3JxrTZ****
    ```

    Sample responses

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 559CC9BDC755F95A64485981
    Date: Wed, 29 Apr 2015 05:21:12 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   Upload an object to a versioning enabled bucket

    Sample requests

    ```
    PUT /example?acl&versionId=CAEQMhiBgIC3rpSD0BYiIDBjYTk5MmIzN2JlNjQxZTFiNGIzM2E3OTliODA0**** HTTP/1.1
    x-oss-object-acl: public-read
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 06:30:11 GMT
    Authorization: OSS qctg2ns3l8u51iu:UTsv3F7L34v+ECq52vURdCSv****
    ```

    Sample responses

    ```
    HTTP/1.1 200 OK
    x-oss-version-id: CAEQMhiBgIC3rpSD0BYiIDBjYTk5MmIzN2JlNjQxZTFiNGIzM2E3OTliODA0****
    x-oss-request-id: 5CAC3BF3B7AEADE017000624
    Date: Tue, 09 Apr 2019 06:30:11 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```


## SDK

You can use OSS SDKs for the following programming languages to call the PutObjectACL operation:

-   [Java](/intl.en-US/SDK Reference/Java/Manage objects/Manage object ACLs.md)
-   [Python](/intl.en-US/SDK Reference/Python/Manage objects/Manage ACL for an object.md)
-   [PHP](/intl.en-US/SDK Reference/PHP/Manage objects/Manage ACL for an object.md)
-   [Go](/intl.en-US/SDK Reference/Go/Manage objects/Manage ACL for an object.md)
-   [C](/intl.en-US/SDK Reference/C/Manage objects/Manage ACL for an object.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Manage objects/Manage object ACLs.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Manage objects/Manage the ACL of an object.md)
-   [Browser.js](/intl.en-US/SDK Reference/Browser.js/Set ACLs.md)

## Error codes

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|AccessDenied|403|The error message returned because the operation user is not the bucket owner or the bucket owner does not have the read or write permissions on the object.|
|InvalidArgument|400|The error message returned because the specified x-oss-object-acl value is invalid.|
|FileAlreadyExists|409|The error message returned because the hierarchical namespace feature is enabled for a bucket when you want to modify the ACL of an object within the bucket and the object is set to a directory.|

