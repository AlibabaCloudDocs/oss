# PutBucketLogging

You can call this operation to enable logging for a bucket. After you enable and configure logging for a bucket, OSS generates log objects based on a predefined naming convention. This way, access logs are generated and stored in the specified bucket on an hourly basis.

## Usage notes

-   The source bucket for which logs are generated and the destination bucket where the generated logs are stored can be the same or different buckets. However, the destination bucket must belong to the same account in the same region as the source bucket.
-   OSS generates bucket access logs on an hourly basis. However, requests in the previous hour may be recorded in the log generated for the subsequent hour.

    For more information about the log file naming conventions and log format, see [Logging](/intl.en-US/Developer Guide/Manage logs/Logging.md).

-   Before you disable logging, OSS keeps generating log objects. Delete log objects you no longer need to reduce storage costs.

    You can configure lifecycle rules to delete log objects at regular intervals. For more information, see [Lifecycle rules](/intl.en-US/Developer Guide/Objects/Object lifecycle/Lifecycle rules.md).

-   More fields may be added to OSS logs. We recommend that developers consider potential compatibility issues when they develop log processing tools.

## Request structure

```
PUT /? logging HTTP/1.1
Date: GMT Date
Content-Length: ContentLength
Content-Type: application/xml
Authorization: SignatureValue 
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
<? xml version="1.0" encoding="UTF-8"? >
<BucketLoggingStatus>
    <LoggingEnabled>
        <TargetBucket>TargetBucket</TargetBucket>
        <TargetPrefix>TargetPrefix</TargetPrefix>
    </LoggingEnabled>
</BucketLoggingStatus>
```

## Request headers

A PutBucketLogging request contains only common request headers. For more information, see [Common request headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Request elements

|Element|Type|Required|Example|Description|
|-------|----|--------|-------|-----------|
|BucketLoggingStatus|Container|Yes|N/A|The container that stores the logging status information.Child nodes: LoggingEnabled

Parent nodes: none |
|LoggingEnabled|Container|This parameter is required when you enable logging.|N/A|The container that stores the access log information.Child nodes: TargetBucket and TargetPrefix

Parent nodes: BucketLoggingStatus |
|TargetBucket|String|This parameter is required when you enable logging.|examplebucket|The bucket that stores access logs.Child nodes: none

Parent nodes: BucketLoggingStatus and LoggingEnabled |
|TargetPrefix|String|No|MyLog-|The prefix of the saved log objects. This element can be left empty.Child nodes: none

Parent nodes: BucketLoggingStatus and LoggingEnabled |

## Response headers

The response to a PutBucketLogging request contains only common response headers. For more information, see [Common response headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Examples

-   Sample request on how to enable logging

    ```
    PUT /? logging HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Content-Length: 186
    Date: Fri, 04 May 2012 03:21:12 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:KU5h8YMUC78M30dXqf3JxrTZ****
    <? xml version="1.0" encoding="UTF-8"? >
    <BucketLoggingStatus>
    <LoggingEnabled>
    <TargetBucket>examplebucket</TargetBucket>
    <TargetPrefix>MyLog-</TargetPrefix>
    </LoggingEnabled>
    </BucketLoggingStatus>
    ```

    Sample response

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E888648906008B
    Date: Fri, 04 May 2012 03:21:12 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   Sample request on how to disable logging

    To disable logging for a bucket, you need only to send an empty BucketLoggingStatus. The following code provides an example on how to disable logging:

    ```
    PUT /? logging HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Content-Type: application/xml
    Content-Length: 86
    Date: Fri, 04 May 2012 04:21:12 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:KU5h8YMUC78M30dXqf3JxrTZ****
    <? xml version="1.0" encoding="UTF-8"? >
    <BucketLoggingStatus>
    </BucketLoggingStatus>
    ```

    Sample response

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674125A4D8906008B
    Date: Fri, 04 May 2012 04:21:12 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```


## SDK

You can use OSS SDKs for the following programming languages to call the PutBucketLogging operation:

-   [Java](/intl.en-US/SDK Reference/Java/Buckets/Set logging.md)
-   [Python](/intl.en-US/SDK Reference/Python/Buckets/Set logging.md)
-   [PHP](/intl.en-US/SDK Reference/PHP/Buckets/Set logging.md)
-   [Go](/intl.en-US/SDK Reference/Go/Buckets/Set logging.md)
-   [C](/intl.en-US/SDK Reference/C/Buckets/Set logging.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Buckets/Set logging.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Buckets/Set logging.md)
-   [Ruby](/intl.en-US/SDK Reference/Ruby/Buckets/Set logging.md)

## Error codes

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|NoSuchBucket|404|The error message returned because the source bucket does not exist.|
|InvalidTargetBucketForLogging|400|The error message returned because the source bucket and the destination bucket do not belong to the same data center.|
|InvalidDigest|400|The error message returned because the Content-MD5 value of the message body is inconsistent with the Content-MD5 value of the request header.|
|MalformedXML|400|The error message returned because the XML format in the request is invalid.|
|InvalidTargetBucketForLogging|403|The error message returned because the requester is not the owner of the destination bucket.|
|AccessDenied|403|The error message returned because the requester is not the owner of the source bucket.|

