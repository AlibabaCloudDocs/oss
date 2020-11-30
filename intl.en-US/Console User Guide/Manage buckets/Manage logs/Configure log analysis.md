# Configure log analysis

You can use Log Service in the OSS console to analyze a large number of logs. This topic describes how to activate and use Log Service in the OSS console.

OSS users often need to analyze access logs and resource consumption data. Examples:

-   OSS bucket usage, traffic usage, and requests
-   Logs of object creation, modification, and deletion operations
-   Frequently accessed objects and object access statistics including visits and generated traffic
-   Logs of requests for which errors are returned and the corresponding error content

We recommend that you use [real-time log query](/intl.en-US/Console User Guide/Manage buckets/Manage logs/Configure real-time log query.md) to analyze OSS access logs of the last seven days free of charge.

**Note:**

Log Service is a paid service. For more information about billing, see [Log Service Pricing](https://www.alibabacloud.com/product/log-service/pricing).

## Activate Log Service

1.  Log on to the [OSS console](https://oss.console.aliyun.com/overview).

2.  In the left-side navigation pane, click **Recommended Services**. On the page that appears, click the **More Service** tab.

3.  Move the pointer over the **Log Service** section and click **Activate**.

4.  On the Enable Service page, read and select **I agree with Log Service Agreement of Service**, and click **Enable Now**.


## Associate a bucket with Log Service

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Recommended Services**. On the page that appears, click the **More Service** tab.

3.  Move the pointer over the **Log Service** section and click **Configure**.

4.  On the **Log Service** page, click **Associate**.

5.  In the Associate Bucket with Logstore panel, associate a bucket with Log Service.

    1.  In step 1, set **Region**, **Project Name**, and **Description** \(optional\).

        -   When you set **Region**, select the region where available buckets are located.
        -   When you set **Project Name**, make sure that the name meets the following conventions:
            -   The project name can contain only lowercase letters, digits, and hyphens \(-\).
            -   The project name must start and end with a lowercase letter or a digit.
            -   The project name must be 3 to 63 characters in length.
    2.  Click **Next**.

    3.  In step 2, set **Logstore Name**, **Data Lifecycle \(Days\)**, and **Shards**.

        If you have configured a Logstore, click **Choose Logstore** and select the Logstore that you want to associate with the bucket.

        -   **Logstore Name**: Specify the name of the Logstore.
            -   The Logstore name can contain only lowercase letters, digits, hyphens \(-\), and underscores \(\_\).
            -   The Logstore name must start and end with a lowercase letter or digit.
            -   The Logstore name must be 3 to 63 characters in length.
        -   **Data Lifecycle \(Days\)**: Set the period in days for which to retain data.
        -   **Shards**: Set the number of shards. For more information, see [Shards](/intl.en-US/Product Introduction/Basic concepts/Shards.md).
    4.  Click **Next**.

    5.  In step 2, set **Associate Buckets** and click **Submit**.


## Configure index information

1.  Click **Configure Index**.

2.  Log Service provides several preset indexes for querying OSS access logs. For more information about the related fields, see [Log fields](/intl.en-US/Data Collection/Cloud product collection/OSS access logs/Log fields.md).

    1.  Find the target project and click the project name.

    2.  Double-click the Logstore name and choose **Index Attributes** \> **Modify**.

        ![Configure indexes](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7867549951/p39792.png)

        **Note:** By default, Log Service creates four dashboards for the Logstore associated with one or more buckets. After you complete the configuration, you can view these dashboards on the Dashboard page. You can also click **Analyze Log** next to a target Logstore on the Log Service page in the OSS console, and click a dashboard name in the left-side navigation pane to view the dashboard.


## Analyze logs

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Recommended Services**. On the page that appears, click the **More Service** tab.

3.  Move the pointer over the **Log Service** section and click **Configure**.

4.  On the **Log Service** page, click **Analyze Log** in the Actions column corresponding to the project that you want to use to analyze logs.

5.  On the log analysis page, you can view the log analysis results either in Logstores or in the dashboard.


