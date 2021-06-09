# PutObjectACL

You can call this operation to modify the access control list \(ACL\) of an object. Only the bucket owner that has permissions to read and write objects in the bucket can call this operation to modify object ACLs.

## Versioning

By default, the PutObjectACL operation is called to configure the ACL of the current version of an object. You can specify a version ID in the request to configure the ACL of the specified version of an object. If the specified version is a delete marker, OSS returns 404 Not Found.

## ACL overview

When you call the PutObjectACL operation, you can set the `x-oss-object-acl` header in request to configure the ACL of an object. The following table describes the ACLs that you can configure for an object.

|ACL|Description|
|:--|:----------|
|private|The object is a private resource. Only the owner of this object has permissions to read and write this object. Other users cannot access the object.|
|public-read|The object is a public-read resource. Only the owner of this object has permissions to write this object. Other users can only read the object.|
|public-read-write|The object is a public-read-write resource. All users have permissions to read and write this object.|
|default|The ACL of the object is the same as that of the bucket in which the object is stored.|

**Note:**

-   The ACL of an object takes precedence over the ACL of the bucket in which the object is stored. For example, if an object whose ACL is public-read-write is stored in a bucket whose ACL is private, all users can read and write the object. By default, if you do not configure the ACL of an object, the ACL of the object is the same as that of the bucket in which the object is stored.
-   Operations that read objects include GetObject, HeadObject, CopyObject, and UploadPartCopy, in which CopyObject and UploadPartCopy read the source object. Operations that write objects include PutObject, PostObject, AppendObject, DeleteObject, DeleteMultipleObjects, CompleteMultipartUpload, and CopyObject, in which CopyObject writes the destination object.
-   When you call operations to write an object, you can also include the x-oss-object-acl header in the request to configure the ACL of the object. For example, you can include the x-oss-object-acl header in a PutObject request to configure the ACL of the object to upload.

## Request structure

```
PUT /ObjectName?acl HTTP/1.1
x-oss-object-acl: Permission
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Request headers

|Header|Type|Required|Example|Description|
|------|----|--------|-------|-----------|
|x-oss-object-acl|String|No|public-read|The ACL of the object when the object is created. Default value: default. Valid values:

-   default: The ACL of the object is the same as that of the bucket in which the object is stored.
-   private: The object is a private resource. Only the owner of this object and authorized users have permissions to read and write this object.
-   public-read: The object is a public-read resource. Only the owner of this object and authorized users have permissions to write this object. Other users can only read the object. Exercise caution when you set the ACL of the object to this value.
-   public-read-write: The object is a public-read-write resource. All users have permissions to read and write the object. Exercise caution when you set the ACL of the object to this value.

For more information about ACLs, see [ACL](/intl.en-US/Developer Guide/Data security/Access and control/ACL.md). |

For more information about the common headers included in PutObjectACL requests such as Host and Date, see [Common request headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Examples

-   Modify the ACL of an object in an unversioned bucket

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

-   Modify the ACL of an object in a versioned bucket

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
|AccessDenied|403|The error message returned because you are not the bucket owner or do not have permissions to read and write the object whose ACL you want to modify.|
|InvalidArgument|400|The error message returned because the specified x-oss-object-acl value is invalid.|
|FileAlreadyExists|409|The error message returned because the object whose ACL you want to modify is a directory in a bucket for which the hierarchical namespace feature is enabled.|

