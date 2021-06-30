# Enable transfer acceleration

OSS uses data centers distributed around the globe to implement transfer acceleration. When a request is sent to your bucket, it is resolved and routed to the data center where the bucket resides over the most optimal network path and protocol. The transfer acceleration feature provides an optimized end-to-end acceleration solution to access OSS over the Internet.

Real-name registration is complete.

You can complete real-name registration by submitting your information on the [Real-name Registration](https://account-intl.console.aliyun.com/#/intlAuth) page.

## Usage notes

-   Only the owner of a bucket or RAM users to which the oss:PutBucketTransferAcceleration permission is granted can initiate requests to configure transfer acceleration for the bucket.
-   After you enable transfer acceleration for a bucket, you can use an accelerate endpoint in addition to the default endpoint to access the bucket. The access speed is accelerated only when you use the accelerate endpoint.
-   You are charged for the transfer acceleration fees when you use the accelerate endpoint to access a bucket. For more information, see [Transfer acceleration fees](/intl.en-US/Pricing/Billing items and methods/Transfer acceleration fees.md).

For more information about transfer acceleration, see [Transfer acceleration](/intl.en-US/Developer Guide/Buckets/Transfer acceleration.md) in the Developer Guide.

## Procedure

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  In the left-side navigation pane, choose **Transmission** \> **Transfer Acceleration**.

4.  Click **Configure**, turn on Transfer Acceleration, and then click **Save**.

    Transfer acceleration takes effect within 30 minutes after it is enabled. After transfer acceleration is enabled for the bucket, the following two accelerate endpoints are added to the bucket:

    -   Global accelerate endpoint: `oss-accelerate.aliyuncs.com`. Transfer acceleration access points are distributed across the world. You can use this endpoint to accelerate data transfer for buckets in all regions.
    -   Accelerate endpoint of regions outside mainland China: `oss-accelerate-overseas.aliyuncs.com`. Transfer acceleration access points are distributed across regions outside mainland China. This endpoint can be used to accelerate data transfer across buckets in the China \(Hong Kong\) region and regions outside mainland China.
    You can compare the access speeds when you use the accelerate and default endpoints to access OSS in different regions. For more information, see [The Comparison of OSS Direct Data Transfer and Accelerated data Transfer in Different Regions](https://oss-accelerate-test.oss-accelerate.aliyuncs.com/acc/oss-transfer-acc.html).


