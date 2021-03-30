# Enable transfer acceleration

Real-name registration is complete.

You can complete real-name registration by submitting your information on the [Real-name Registration](https://account-intl.console.aliyun.com/#/intlAuth) page.

## Usage notes

When you use transfer acceleration, take note of the following items:

-   The charges for transfer acceleration are billed as a separate item from OSS. For more information, see [Object Storage Service Pricing](https://cn.aliyun.com/price/product#/oss/detail).
-   Transfer acceleration is applied only when you use an accelerate endpoint to access a bucket that has transfer acceleration enabled.

For more information about transfer acceleration, see [Transfer acceleration](/intl.en-US/Developer Guide/Buckets/Transfer acceleration.md).

## Procedures

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  In the left-side navigation pane, choose **Transmission** \> **Transfer Acceleration**.

4.  Click **Configure**, turn on Transfer Acceleration, and then click **Save**.

    Transfer acceleration takes effect within 30 minutes after it is enabled. After transfer acceleration is enabled for the bucket, the following two accelerate endpoints are added to the bucket.

    You can compare the access speeds from different regions when accelerate and default endpoints are used. For more information, see [The Comparison of OSS Direct Data Transfer and Accelerated data Transfer in Different Regions](https://oss-accelerate-test.oss-accelerate.aliyuncs.com/acc/oss-transfer-acc.html).


