# HTTP 424 status code

This topic describes the types of error messages returned with HTTP status code 424, and the common causes and solutions to these errors.

## MirrorFailed

-   Error message: Mirroring failed, please check your mirror configuration.

    Cause: The specified back-to-origin URL cannot be accessed or the object you want to access does not exist on the origin.

    Solution: Check whether the back-to-origin URL specified in the request is valid. For more information, see [Overview](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Back-to-origin rules/Overview.md).

-   Error message: Read body from mirror host failed, please check your mirror host.

    Cause: Data stored on the origin cannot be read.

    Solution: Check whether objects stored on the origin specified by the back-to-origin URL can be accessed.

-   Error message: Bytes read is not equal to expected.

    Cause: Data that is read from the origin is missing or invalid.

    Solution: Check whether the origin can send data to OSS.

-   Error message: MD5 in header\(\) is not equal to MD5 calculated.

    Cause: The MD5 hash of the object obtained from the origin is different from the value of Content-MD5 in the back-to-origin request. In this case, the object obtained from the origin is not stored in OSS.

    Solution: Check the services and network of the origin to troubleshoot this problem.

-   Error message: The object you specified already exists and can not be overwritten.

    Cause: The object specified in the request already exist and cannot be overwritten.

    Solution: Call the ListObjects operation to check whether the object you specifed in the request exists and whether the object can be overwritten. For more information, see [GetBucket \(ListObjects\)](/intl.en-US/API Reference/Bucket operations/Basic operations/GetBucket (ListObjects).md).

-   Error message: The object you specified is immutable.

    Cause: The object obtained from the origin cannot be write to the bucket because the bucket is protected by a retention policy.

    Solution: Call the GetBucketWorm operation to check whether the bucket to which you want to write the object obtained from the origin is protected by a retention policy. For more information, see [GetBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/GetBucketWorm.md).

-   Error message: Meta is too large.

    Cause: The size of the user metadata of the object exceeds the limit.

    Solution: Check the size of the user metadata of the object. You can configure multiple user metadata parameters for an object. However, the total size of the user metadata of an object cannot exceed 8 KB.

-   Error message: Mirror request is rejected by QoS.

    Cause: The queries per second \(QPS\) exceeds the limit.

    Solution: Limit the QPS for mirroring-based back-to-origin. In regions within China, the default QPS for mirroring-based back-to-origin is 2,000, and the default bandwidth is 2 Gbit/s. In regions outside China, the default QPS for mirroring-based back-to-origin is 1,000, and the default bandwidth is 1 Gbit/s. If you require a higher QPS, contact technical support.


