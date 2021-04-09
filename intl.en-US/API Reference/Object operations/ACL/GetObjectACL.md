# GetObjectACL

You can call this operation to query the ACL of an object in a bucket.

## Versioning

By default, when you call GetObjectACL to query the ACL of an object, only the ACL of the current version of the object is returned. You can specify the versionId parameter in the request to query the ACL of a specified version of an object. If the current version of the object is a delete marker, OSS returns 404 Not Found.

**Note:** If a GetObjectACL request is sent to query the ACL of an object for which ACL is not configured, OSS returns the default ACL for this object. In this case, the ACL of this object is the same as that of the bucket in which the object is stored. For example, if the ACL of the bucket in which the object is stored is private, the ACL of the object is also private.

## Request structure

```
GET /ObjectName?acl HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Response elements

|Element|Type|Description|
|:------|:---|:----------|
|AccessControlList|Container|The container used to store the ACL information. Parent nodes: AccessControlPolicy |
|AccessControlPolicy|Container|The container used to store results of the GetObjectACL request. Parent nodes: none |
|DisplayName|String|The name of the bucket owner, which is the same as the user ID. Parent nodes: AccessControlPolicy.Owner |
|Grant|Enumerated string|The ACL of the object. Valid values: private, public-read, and public-read-write.

Parent nodes: AccessControlPolicy.AccessControlList |
|ID|String|The user ID of the bucket owner. Parent nodes: AccessControlPolicy.Owner |
|Owner|Container|The container used to store the information about the bucket owner. Parent nodes: AccessControlPolicy |

## Examples

-   Query the ACL of an object in an unversioned bucket.

    Sample requests

    ```
    GET /test-object?acl HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Wed, 29 Apr 2015 05:21:12 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:CTkuxpLAi4XZ+WwIfNm0Fmgb****
    ```

    Sample responses

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 559CC9BDC755F95A64485981
    Date: Wed, 29 Apr 2015 05:21:12 GMT
    Content-Length: 253
    Content-Tupe: application/xml
    Connection: keep-alive
    Server: AliyunOSS
    <?xml version="1.0" ?>
    <AccessControlPolicy>
        <Owner>
            <ID>00220120222</ID>
            <DisplayName>00220120222</DisplayName>
        </Owner>
        <AccessControlList>
            <Grant>public-read </Grant>
        </AccessControlList>
    </AccessControlPolicy>
    ```

-   Query the ACL of an object in a versioned bucket.

    Sample requests

    ```
    GET /example?acl&versionId=CAEQMhiBgMC1qpSD0BYiIGQ0ZmI5ZDEyYWVkNTQwMjBiNTliY2NjNmY3ZTVk**** HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 06:30:10 GMT
    Authorization: OSS qctg2ns3l8u51iu:w4DK66Kb/0M9GJKdsrpNs8l1****
    ```

    Sample responses

    ```
    HTTP/1.1 200 OK
    x-oss-version-id: CAEQMhiBgMC1qpSD0BYiIGQ0ZmI5ZDEyYWVkNTQwMjBiNTliY2NjNmY3ZTVk****
    x-oss-request-id: 5CAC3BF2B7AEADE017000621
    Date: Tue, 09 Apr 2019 06:30:10 GMT
    Content-Length: 261
    Content-Tupe: application/xml
    Connection: keep-alive
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <AccessControlPolicy>
      <Owner>
        <ID>1234513715092****</ID>
        <DisplayName>1234513715092****</DisplayName>
      </Owner>
      <AccessControlList>
        <Grant>public-read</Grant>
      </AccessControlList>
    </AccessControlPolicy>
    ```


## SDK

You can use OSS SDKs for the following programming languages to call the GetObjectACL operation:

-   [Java](/intl.en-US/SDK Reference/Java/Manage objects/Manage object ACLs.md)
-   [Python](/intl.en-US/SDK Reference/Python/Manage objects/Manage ACL for an object.md)
-   [PHP](/intl.en-US/SDK Reference/PHP/Manage objects/Manage ACL for an object.md)
-   [Go](/intl.en-US/SDK Reference/Go/Manage objects/Manage ACL for an object.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Manage objects/Manage object ACLs.md)

## Error codes

|Error code|HTTP status code|Error message|Description|
|:---------|:---------------|:------------|:----------|
|AccessDenied|403|You do not have read acl permission on this object.|The error message returned because you are not authorized to perform the GetObjectACL operation. Only the bucket owner has permissions to call GetObjectACL to query the ACL of an object in the bucket.|
|FileAlreadyExists|409|The object you specified already exists and is a directory.|The error message returned because the object whose ACL you want to query is a directory within a bucket with the hierarchical namespace feature enabled.|

