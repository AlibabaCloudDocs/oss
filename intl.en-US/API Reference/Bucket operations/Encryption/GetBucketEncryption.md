# GetBucketEncryption

You can call this operation to query the encryption rules configured for a bucket.

**Note:** Only the bucket owner or authorized RAM users can query the encryption rules configured for a bucket. If other users try to query the encryption rules configured for the bucket, OSS returns 403. For more information about bucket encryption, see [Server-side encryption](/intl.en-US/Developer Guide/Data security/Data encryption/Server-side encryption.md).

## Request structure

```
Get /? encryption HTTP/1.1
Date: GMT Date
Host: BucketName.oss.aliyuncs.com
Authorization: SignatureValue
```

## Request headers

A GetBucketEncryption request contains only common request headers. For more information, see [Common request headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Response headers

The response to a GetBucketEncryption request contains only common response headers. For more information, see [Common response headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Response elements

|Element|Type|Example|Description|
|-------|----|-------|-----------|
|ServerSideEncryptionRule|Container|N/A|The container that stores server-side encryption rules. Child node: ApplyServerSideEncryptionByDefault |
|ApplyServerSideEncryptionByDefault|Container|N/A|The container that stores the default server-side encryption method. Child nodes: SSEAlgorithm and KMSMasterKeyID |
|SSEAlgorithm|String|KMS|The default server-side encryption method. Valid values: KMS and AES256 |
|KMSMasterKeyID|String|9468da86-3509-4f8d-a61e-6eab1eac\*\*\*\*|The CMK ID that is used for encryption. This parameter is returned only when the value of SSEAlgorithm is KMS and a CMK ID is specified in the request. In other cases, this parameter is null. |

## Examples

-   Sample request

    ```
    Get /? encryption HTTP/1.1
    Date: Tue, 20 Dec 2018 11:20:10 GMT
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Authorization: OSS qn6qrrqxo2oawuk53otf****:ceOEyZavKY4QcjoUWYSpYbJ3****
    ```

-   Sample response

    The following response indicates that SSE-KMS is configured for the bucket.

    ```
    HTTP/1.1 204 NoContent
    x-oss-request-id: 5C1B138A109F4E405B2D8AEF
    Date: Tue, 20 Dec 2018 11:22:05 GMT
    <? xml version="1.0" encoding="UTF-8"? >
    <ServerSideEncryptionRule>
      <ApplyServerSideEncryptionByDefault>
        <SSEAlgorithm>KMS</SSEAlgorithm>
        <KMSMasterKeyID>9468da86-3509-4f8d-a61e-6eab1eac****</KMSMasterKeyID>
      </ApplyServerSideEncryptionByDefault>
    </ServerSideEncryptionRule>
    ```


## SDK

You can use OSS SDKs for the following programming languages to call GetBucketEncryption:

-   [Java](/intl.en-US/SDK Reference/Java/Data encryption/Server-side encryption.md)
-   [Python](/intl.en-US/SDK Reference/Python/Data encryption/Server-side encryption.md)
-   [Go](/intl.en-US/SDK Reference/Go/Data encryption/Server-side encryption.md)
-   [C++](/intl.en-US/SDK Reference/C++/Data encryption/Server-side encryption.md)
-   [PHP](/intl.en-US/SDK Reference/PHP/Data encryption/Server-side encryption.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Server-side encryption.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Server-side encryption.md)

## Errors codes

|Error code|HTTP status code|Description|
|----------|----------------|-----------|
|AccessDenied|403|The error message returned because you do not have permissions to query the encryption rules configured for the bucket.|
|NoSuchBucket|400|The error message returned because the bucket whose encryption rules you want to query does not exist.|
|NoSuchServerSideEncryptionRule|400|The error message returned because the no encryption rules are configured for the bucket.|

