# GetBucketInfo

You can call this operation to view the information about a bucket. Only the bucket owner can view the information about a bucket.

**Note:** The request can be initiated from an OSS endpoint.

## Request structure

```
GET /?bucketInfo HTTP/1.1
Host: BucketName.oss.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Response elements

|Element|Type|Description|
|:------|:---|:----------|
|BucketInfo|Container|The container that stores the bucket information. Child nodes: Bucket

Parent nodes: none |
|Bucket|Container|The container that stores the bucket information. Parent nodes: BucketInfo |
|CreationDate|Date|The time when the bucket was created. The time follows the ISO 8601 standard in the YYYY-MM-DDTHH:mm:ss.sssZ format. Example: 2013-07-31T10:56:21.000Z.

Parent nodes: BucketInfo and Bucket |
|ExtranetEndpoint|String|The public endpoint that is used to access the bucket over the Internet. Parent nodes: BucketInfo and Bucket |
|IntranetEndpoint|String|The internal endpoint that is used to access the bucket from Elastic Compute Service \(ECS\) instances in the same region. Parent nodes: BucketInfo and Bucket |
|Location|String|The region where the bucket is located. Parent nodes: BucketInfo and Bucket |
|Name|String|The name of the bucket. Parent nodes: BucketInfo and Bucket |
|Owner|Container|The container that stores the information about the bucket owner. Parent nodes: BucketInfo and Bucket |
|ID|String|The user ID of the bucket owner. Parent nodes: BucketInfo, Bucket, and Owner |
|DisplayName|String|The name of the bucket owner, which is the same as the user ID. Parent nodes: BucketInfo, Bucket, and Owner |
|AccessControlList|Container|The container that stores the access control list \(ACL\) information. For more information about bucket ACLs, see [Set the ACL for a bucket](/intl.en-US/Developer Guide/Buckets/Set the ACL for a bucket.md).

Parent nodes: BucketInfo and Bucket |
|Grant|Enumerated string|The ACL for the bucket. Valid values: private, public-read, and public-read-write

Parent nodes: BucketInfo, Bucket, and AccessControlList |
|DataRedundancyType|Enumerated string|The redundancy option for the bucket. Valid values: LRS and ZRS

Parent nodes: BucketInfo and Bucket |
|StorageClass|String|The storage class of the bucket. Valid values: Standard, IA, Archive, and ColdArchive

For more information about storage classes, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md). |
|Versioning|String|The versioning status of the bucket. Valid values: Enabled and Suspended

For more information about versioning, see [PutBucketVersioning](/intl.en-US/API Reference/Bucket operations/Versioning/PutBucketVersioning.md).

Parent nodes: BucketInfo and Bucket |
|ServerSideEncryptionRule|Container|The container that stores server-side encryption rules. For more information about server-side encryption, see [A](/intl.en-US/Developer Guide/Data security/Data encryption/Server-side encryption.md).

Parent nodes: BucketInfo and Bucket |
|ApplyServerSideEncryptionByDefault|Container|The container that stores the default server-side encryption method. Parent nodes: BucketInfo and Bucket |
|SSEAlgorithm|String|The default server-side encryption method. Valid values: KMS and AES256 |
|KMSMasterKeyID|String|The customer master key \(CMK\) ID in use. A valid value is returned only when you set SSEAlgorithm to KMS and specify the CMK ID. In other cases, null is returned.|
|HierarchicalNamespace|String|The status of the hierarchical namespace feature for the bucket. Set the value to Enabled. Parent nodes: BucketInfo and Bucket |

## Examples

Sample requests

```
Get /?bucketInfo HTTP/1.1
Host: oss-example.oss.aliyuncs.com  
Date: Sat, 12 Sep 2015 07:51:28 GMT
Authorization: OSS qn6qrrqxo2oawuk53otf****: BuG4rRK+zNhH1AcF51NNHD39****
                
```

Sample responses

-   Sample success responses when information about the specified bucket is obtained

    ```
    HTTP/1.1 200
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Sat, 12 Sep 2015 07:51:28 GMT
    Connection: keep-alive
    Content-Length: 531  
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <BucketInfo>
      <Bucket>
        <CreationDate>2013-07-31T10:56:21.000Z</CreationDate>
        <ExtranetEndpoint>oss-cn-hangzhou.aliyuncs.com</ExtranetEndpoint>
        <IntranetEndpoint>oss-cn-hangzhou-internal.aliyuncs.com</IntranetEndpoint>
        <Location>oss-cn-hangzhou</Location>
        <StorageClass>Standard</StorageClass>
        <HierarchicalNamespace>Enabled</HierarchicalNamespace>
        <Name>oss-example</Name>
        <Owner>
          <DisplayName>username</DisplayName>
          <ID>27183473914****</ID>
        </Owner>
        <AccessControlList>
          <Grant>private</Grant>
        </AccessControlList>  
        <Comment>test</Comment>
      </Bucket>
    </BucketInfo>
    ```

-   Sample error responses when the specified bucket does not exist

    ```
    HTTP/1.1 404 
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Sat, 12 Sep 2015 07:51:28 GMT
    Connection: keep-alive
    Content-Length: 308  
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <Error>
      <Code>NoSuchBucket</Code>
      <Message>The specified bucket does not exist.</Message>
      <RequestId>568D547F31243C673BA1****</RequestId>
      <HostId>nosuchbucket.oss.aliyuncs.com</HostId>
      <BucketName>nosuchbucket</BucketName>
    </Error>
    ```

-   Sample error responses when you are not authorized to access information about the bucket

    ```
    HTTP/1.1 403
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Sat, 12 Sep 2015 07:51:28 GMT
    Connection: keep-alive
    Content-Length: 209  
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <Error>
      <Code>AccessDenied</Code>
      <Message>AccessDenied</Message>
      <RequestId>568D5566F2D0F89F5C0E****</RequestId>
      <HostId>test.oss.aliyuncs.com</HostId>
    </Error>
    ```


## SDK

You can use OSS SDKs for the following programming languages to call the GetBucketInfo operation:

-   [Java](/intl.en-US/SDK Reference/Java/Buckets/Query bucket information.md)
-   [Python](/intl.en-US/SDK Reference/Python/Buckets/Obtain bucket information.md)
-   [Go](/intl.en-US/SDK Reference/Go/Buckets/Query bucket information.md)
-   [C++](/intl.en-US/SDK Reference/C++/Buckets/Query bucket information.md)
-   [C](/intl.en-US/SDK Reference/C/Buckets/Query bucket information.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Buckets/Query bucket information.md)
-   [Android](/intl.en-US/SDK Reference/Android/Buckets/Query bucket information.md)

## Error codes

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|NoSuchBucket|404|The error message returned because the requested bucket does not exist.|
|AccessDenied|403|The error message returned because you are not authorized to view the information about the bucket. Only the bucket owner can view the information about a bucket.|

