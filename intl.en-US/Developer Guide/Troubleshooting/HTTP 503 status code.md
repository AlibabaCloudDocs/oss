# HTTP 503 status code

This topic describes the types of error messages returned with HTTP status code 503, and the common causes and solutions to these errors.

## DownloadTrafficRateLimitExceeded

-   Error message: Please reduce your upload request traffic.
-   Cause: The download traffic exceeds the limit.
-   Solution: Make sure that the download traffic does not exceed the limit. The default download traffic limit is is 5 Gbit/s. To adjust the limit, submit a ticket.

## UploadTrafficRateLimitExceeded

-   Error message: Please reduce your upload request traffic.
-   Cause: The upload traffic exceeds the limit.
-   Solution: Make sure that the upload traffic does not exceed the limit. The default upload traffic limit is 5 Gbit/s. To adjust the limit, submit a ticket.

## MetaOperationQpsLimitExceeded

-   Error message: Qps limit for the meta operation is exceeded.
-   Cause: The queries per second \(QPS\) exceeds the default limit.

    OSS limits the QPS for the following API operations:

    -   Operations related to services, such as GetService \(ListBuckets\)
    -   Operations related to buckets, such as PutBucket and GetBucketLifecycle
    -   Operations related to CORS, such as PutBucketCORS and GetBucketCORS
    -   Operations related to LiveChannel, such as PutLiveChannel and DeleteLiveChannel.
-   Solution: Perform the operation again after a few seconds.

## ServiceUnavailable

-   Error message: Thread pool is almost full, please retry later.
-   Cause: The OSS server is busy.
-   Solution: Try again later.

## TotalQpsLimitExceeded

-   Error message: Max total qps limit is exceeded.
-   Cause: The QPS exceeds the limit.
-   Solution: Limit the total QPS for your Alibaba Cloud account. The limit of the total QPS for a single account is 10,000. However, the actual values that can be achieved are different in different read and write modes:

    -   Sequential read/write: 2,000 QPS

        If you upload a large number of objects whose names contain sequential prefixes such as timestamps and letters, multiple object indexes may be stored in a single partition. If too many requests are sent to query these objects, the response time may be slow. We recommend that you do not upload a large number of objects whose names contain sequential prefixes. For more information about how to change sequential prefixes to random prefixes, see [OSS performance and scalability best practices](/intl.en-US/Best Practices/OSS performance and scalability best practices.md).

    -   Non-sequential read/write: 10,000 QPS
    If you require a higher QPS, contact technical support.


## ActiveRequestLimitExceeded

-   Error message: Max active request limit is exceeded.
-   Cause: The number of concurrent requests exceeds the limit.
-   Solution: Contact technical support.

## CpuLimitExceeded

-   Error message: Please reduce your request rate.
-   Cause: The CPU cores consumed by concurrent requests for IMG exceeds the limit.
-   Solution: Reduce the number of concurrent requests for IMG. If you want to increase the number of concurrent requests , contact technical support.

