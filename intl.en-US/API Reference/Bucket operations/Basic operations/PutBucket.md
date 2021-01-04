# PutBucket

You can call this operation to create a bucket.

## Usage notes

When you call the PutBucket operation, take note of the following items:

-   Anonymous access is not supported.
-   An Alibaba Cloud account can create up to 100 buckets in the same region.
-   Each region has corresponding endpoints. For more information about the mappings between regions and endpoints, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).

## Request structure

```
PUT / HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
x-oss-acl: Permission
Authorization: SignatureValue
<? xml version="1.0" encoding="UTF-8"? >
<CreateBucketConfiguration>
    <StorageClass>Standard</StorageClass>
</CreateBucketConfiguration>
```

## Request headers

|Header|Type|Required|Example|Description|
|:-----|:---|:-------|-------|:----------|
|x-oss-acl|String|No|private|The access control list \(ACL\) for a bucket. Default value: private. Valid values:

-   public-read-write
-   public-read
-   private

For more information about bucket ACLs, see [Set the ACL for a bucket](/intl.en-US/Developer Guide/Buckets/Set the ACL for a bucket.md). |

For more information about the common request headers included in the PutBucket request, see [Common request headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Request elements

|Element|Type|Required|Example|Description|
|-------|----|:-------|-------|-----------|
|StorageClass|String|No|Standard|The storage class of the bucket. Valid values:

-   Standard \(default value\)
-   IA \(Infrequent Access\)
-   Archive
-   ColdArchive |
|DataRedundancyType|String|No|LRS|The disaster recovery type of a bucket.

Valid values:

-   LRS \(default value\)

If you specify LRS, OSS stores the copies of each object across different devices within the same zone. This way, OSS ensures data reliability and availability when hardware failures occur.

-   ZRS

Zone-redundant storage \(ZRS\) uses the multi-zone\(AZ\) mechanism to distribute user data across three zones within the same region. Even if one zone becomes unavailable due to failures such as power outages and fires, the data is still accessible. |

For more information about the storage classes and disaster recovery types of buckets, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md).

## Response headers

The response to a PutBucket request contains only common response headers. For more information, see [Common response headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Examples

Sample requests

```
PUT / HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2017 03:15:40 GMT
x-oss-acl: private
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:77Dvh5wQgIjWjwO/KyRt8dOP****
<? xml version="1.0" encoding="UTF-8"? >
<CreateBucketConfiguration>
    <StorageClass>Standard</StorageClass>
</CreateBucketConfiguration>
```

Sample responses

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906****
Date: Fri, 24 Feb 2017 03:15:40 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

## SDK

You can use OSS SDKs for the following programming languages to call the PutBucket operation:

-   [Java](/intl.en-US/SDK Reference/Java/Buckets/Create buckets.md)
-   [Python](/intl.en-US/SDK Reference/Python/Buckets/Create a bucket.md)
-   [PHP](/intl.en-US/SDK Reference/PHP/Buckets/Create buckets.md)
-   [Go](/intl.en-US/SDK Reference/Go/Buckets/Create buckets.md)
-   [C](/intl.en-US/SDK Reference/C/Buckets/Create buckets.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Buckets/Create buckets.md)
-   [Android](/intl.en-US/SDK Reference/Android/Buckets/Create buckets.md)
-   [iOS](/intl.en-US/SDK Reference/iOS/Manage buckets.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Buckets/Create buckets.md)
-   [Ruby](/intl.en-US/SDK Reference/Ruby/Buckets/Create buckets.md)

## Errors codes

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|InvalidBucketName|400|The error message returned because the bucket name does not comply with the naming conventions.|
|AccessDenied|403|-   The error message returned because the information for user authentication is not imported when you initiate a PutBucket request.
-   The error message returned because you are not authorized to perform the PutBucket operation. |
|TooManyBuckets|400|The error message returned because the number of buckets you want to create exceeds the upper limit. An Alibaba Cloud account can create up to 100 buckets in the same region.|
|BucketAlreadyExists|409|The error message returned because the specified bucket already exists or is owned by another user. Specify a new bucket name that conforms to the naming conventions when you create the bucket.|

