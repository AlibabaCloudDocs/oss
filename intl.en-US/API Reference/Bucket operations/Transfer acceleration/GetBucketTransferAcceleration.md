# GetBucketTransferAcceleration

You can call this operation to query the transfer acceleration configurations of a bucket.

## Usage notes

-   Only the owner of a bucket or RAM users granted with the oss:PutBucketTransferAcceleration permission can initiate requests to query the transfer acceleration configurations of a bucket.
-   If transfer acceleration is not configured for the bucket to which you send the GetBucketTransferAcceleration request, no configurations are returned.

For more information about transfer acceleration, see [Transfer acceleration](/intl.en-US/Developer Guide/Buckets/Transfer acceleration.md) in the Developer Guide.

## Request structure

```
GET /?transferAcceleration HTTP/1.1
Date: GMT Date
Content-Length: ContentLength
Content-Type: application/xml
Host: BucketName.oss.aliyuncs.com
Authorization: SignatureValue
```

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|TransferAccelerationConfiguration|Container|N/A|The container used to store transfer acceleration configurations.|
|Enabled|String|true|The status of transfer acceleration Valid values:-   true: indicates that transfer acceleration is enabled for the bucket.
-   false: indicates that transfer acceleration is disabled for the bucket. |

For more information about other common response headers, such as x-oss-request-id and Date, contained in the response to a GetBucketTransferAcceleration request, see [Common response headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Examples

-   Sample requests

    The following sample request is used to query the status of transfer acceleration of a bucket named examplebucket:

    ```
    GET /?transferAcceleration HTTP/1.1
    Date: Fri , 30 Apr 2021 13:08:38 GMT
    Content-Length: 443
    Content-Type: application/xml
    Host: examplebucket.oss.aliyuncs.com
    Authorization: OSS qn6qrrqxo2oawuk53otf****:PYbzsdWAIWAlMW8luk****
    ```

-   Sample responses

    The following response indicates that transfer acceleration is enabled for examplebucket:

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Fri , 30 Apr 2021 13:08:38 GMT
    <TransferAccelerationConfiguration>
     <Enabled>true</Enabled>
    </TransferAccelerationConfiguration>
    ```


## Error codes

|Error code|HTTP status code|Description|
|----------|----------------|-----------|
|NoSuchTransferAccelerationConfiguration|404|The error message returned because transfer acceleration is not configured for the bucket.|

