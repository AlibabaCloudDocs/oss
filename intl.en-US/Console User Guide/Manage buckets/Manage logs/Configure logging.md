# Configure logging

When you access OSS, a large number of access logs are generated. You can enable logging to facilitate log queries. After you enable and configure logging for a bucket, OSS generates an object on an hourly basis based on the predefined naming conventions. This way, access logs are written to a specified bucket as objects.

At least one[AccessKey](https://www.alibabacloud.com/help/zh/doc-detail/53045.html) pair is enabled in your account.

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  In the left-side navigation pane, choose **Logging** \> **Logging**.

4.  Click **Configure**. Set **Destination Bucket** and **Log Prefix**.

    -   **Destination Bucket**: Select the name of the bucket from the drop-down list in which access logs are to store. You must be the owner of the selected bucket, and the bucket must be in the same region as the bucket for which logging is enabled.
    -   **Log Prefix**: Enter the prefix and folder where the access logs are stored. Example: log/<TargetPrefix\>. The access logs are stored in the log/ directoey.
    **Note:** For more information about the format of access logs and log naming conventions, see [Logging](/intl.en-US/Developer Guide/Manage logs/Logging.md).

5.  Click **Save**.


