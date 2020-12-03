# PutBucketEncryption

You can call this operation to configure encryption rules for a bucket.

**Note:** Only the bucket owner or authorized RAM users can configure encryption rules for a bucket. Otherwise, OSS returns the 403 error. For more information about bucket encryption, see [Server-side encryption](/intl.en-US/Developer Guide/Data security/Data encryption/Server-side encryption.md).

## Request structure

```
PUT /? encryption HTTP/1.1
Date: GMT Date
Content-Length: ContentLength
Content-Type: application/xml
Host: BucketName.oss.aliyuncs.com
Authorization: SignatureValue
<? xml version="1.0" encoding="UTF-8"? >
<ServerSideEncryptionRule>
  <ApplyServerSideEncryptionByDefault>
    <SSEAlgorithm>AES256</SSEAlgorithm>
    <KMSMasterKeyID></KMSMasterKeyID>
  </ApplyServerSideEncryptionByDefault>
</ServerSideEncryptionRule>
```

## Request elements

|Element|Type|Required|Description|
|-------|----|--------|-----------|
|ServerSideEncryptionRule|Container|Yes|The container that stores server-side encryption rules. Child node: ApplyServerSideEncryptionByDefault |
|ApplyServerSideEncryptionByDefault|Container|Yes|The container that stores the default server-side encryption method. Child nodes: SSEAlgorithm and KMSMasterKeyID |
|SSEAlgorithm|String|Yes|The default server-side encryption method. Valid values: KMS, AES256.

You are charged for calling API operations when you use CMKs to encrypt or decrypt data. For more information about the fees, see [KMS pricing](/intl.en-US/Pricing/Billing.md).

In cross-region replications, if the default server-side encryption method is configured for the destination bucket and ReplicaCMKID is configured in the replication rule:

-   If objects in the source bucket are not encrypted, they are encrypted using the default encryption method of the destination bucket after they are replicated.
-   If objects in the source bucket are encrypted using SSE-KMS or SSE-OSS, they are encrypted using the same method after they are replicated.

For more information, see [Use cross-region replication with server-side encryption](/intl.en-US/Developer Guide/Data security/Disaster recovery/Cross-region replication in specific scenarios.md). |
|KMSDataEncryption|String|No|The algorithm used to encrypt objects. If this element is not specified, objects are encrypted by using AES256. This element is valid only when the value of SSEAlgorithm is set to KMS.Valid value: SM4. |
|KMSMasterKeyID|String|No|The CMK ID that must be specified when SSEAlgorithm is set to KMS and a specified CMK is used for encryption. In other cases, this element must be set to null.|

## Examples

-   Sample requests
    -   Set the encryption method to SSE-KMS

        The following sample request can be sent to configure the encryption method of the bucket named oss-example to SSE-KMS:

        ```
        PUT /? encryption HTTP/1.1
        Date: Thur, 5 Nov 2020 11:09:13 GMT
        Content-Length: ContentLength
        Content-Type: application/xml
        Host: oss-example.oss-cn-hangzhou.aliyuncs.com
        Authorization: OSS qn6qrrqxo2oawuk53otf****:ceOEyZavKY4QcjoUWYSpYbJ3****
        <? xml version="1.0" encoding="UTF-8"? >
        <ServerSideEncryptionRule>
          <ApplyServerSideEncryptionByDefault>
            <SSEAlgorithm>KMS</SSEAlgorithm>
            <KMSMasterKeyID>9468da86-3509-4f8d-a61e-6eab1eac****</KMSMasterKeyID>
          </ApplyServerSideEncryptionByDefault>
        </ServerSideEncryptionRule>
        ```

-   Sample response

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 5C1B138A109F4E405B2D****
    Date: Thur, 5 Nov 2020 11:09:13 GMT
    ```


## SDK

You can use OSS SDKs for the following programming languages to call the PutBucketEncryption operation:

-   [Java](/intl.en-US/SDK Reference/Java/Data encryption/Server-side encryption.md)
-   [Python](/intl.en-US/SDK Reference/Python/Data encryption/Server-side encryption.md)
-   [Go](/intl.en-US/SDK Reference/Go/Data encryption/Server-side encryption.md)
-   [C++](/intl.en-US/SDK Reference/C++/Data encryption/Server-side encryption.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Server-side encryption.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Server-side encryption.md)

## Errors codes

|Error code|HTTP status code|Description|
|----------|----------------|-----------|
|InvalidEncryptionAlgorithmError|400|The error returned because the value of SSEAlgorithm is not KMS or AES256. The following error message is returned: `The Encryption request you specified is not valid. Supported value: AES256/KMS`.|
|InvalidArgument|400|The error returned because the value of SSEAlgorithm is AES256 but KMSMasterKeyID is specified. The following error message is returned: `KMSMasterKeyID is not applicable if the default sse algorithm is not KMS`.|

