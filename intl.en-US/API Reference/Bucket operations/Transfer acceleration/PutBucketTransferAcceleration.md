# PutBucketTransferAcceleration

You can call this operation to configure transfer acceleration for a bucket. After you enable transfer acceleration for a bucket, users worldwide can access objects more quickly. The transfer acceleration feature is applicable to scenarios where data needs to be transferred over long geographical distances. This feature can also be used to download or upload objects that are gigabytes or terabytes in size.

## Usage notes

-   Only the owner of a bucket or RAM users to which the oss:PutBucketTransferAcceleration permission is granted can initiate requests to configure transfer acceleration for the bucket.
-   After you enable transfer acceleration for a bucket, you can use an accelerate endpoint in addition to the default endpoint to access the bucket. The access speed is accelerated only when you use the accelerate endpoint.
-   You are charged for the transfer acceleration fees when you use the accelerate endpoint to access a bucket. For more information, see [Transfer acceleration fees](/intl.en-US/Pricing/Billing items and methods/Transfer acceleration fees.md).

For more information about transfer acceleration, see [Transfer acceleration](/intl.en-US/Developer Guide/Buckets/Transfer acceleration.md) in the Developer Guide.

## Request structure

```
PUT /?transferAcceleration HTTP/1.1
Date: GMT Date
Content-Length: ContentLength
Content-Type: application/xml
Host: BucketName.oss.aliyuncs.com
Authorization: SignatureValue
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|TransferAccelerationConfiguration|Container|Yes|N/A|The container used to store transfer acceleration configurations.|
|Enabled|String|Yes|true|Specifies whether to enable transfer acceleration for the bucket. Valid values:-   true: indicates that the request is sent to enable transfer acceleration for the bucket.
-   false: indicates that the request is sent to disable transfer acceleration for the bucket.

**Note:** Transfer acceleration takes effect within 30 minutes after it is enabled. |

For more information about the common request headers contained in a PutBucketTransferAcceleration request, such as Authorization and Content-Length, see [Common request headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Response headers

All deaders contained in the response to a PutBucketTransferAcceleration request are common response headers, such as x-oss-request-id and Date. For more information about the common response headers, see [Common response headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Examples

-   Sample requests

    The following sample request is used to enable transfer acceleration for a bucket named examplebucket:

    ```
    PUT /?transferAcceleration HTTP/1.1
    Date: Fri , 30 Apr 2021 13:08:38 GMT
    Content-Length: 443
    Content-Type: application/xml
    Host: examplebucket.oss.aliyuncs.com
    Authorization: OSS qn6qrrqxo2oawuk53otf****:PYbzsdWAIWAlMW8luk****
    <TransferAccelerationConfiguration>
      <Enabled>true</Enabled>
    </TransferAccelerationConfiguration>
    ```

-   Sample responses

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674A4D890****
    Date: Fri , 30 Apr 2021 13:08:38 GMT
    Content-Length: 443
    Connection: keep-alive
    Server: AliyunOSS
    ```


## Error codes

|Error code|HTTP status code|Description|
|----------|----------------|-----------|
|AccessDenied|404|The error message returned because you are not authorized to perform the PutBucketTransferAcceleration operation. Only users granted with the oss:PutBucketTransferAcceleration permission can configure transfer acceleration for the bucket.|
|MalformedXML|400|The error message returned because the request is not in a valid XML format. For example, the Enabled field in the request is set to a value other than true or false.|

