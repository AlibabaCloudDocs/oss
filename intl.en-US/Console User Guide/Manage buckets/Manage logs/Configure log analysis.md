# Configure log analysis

You can use the log analysis feature to analyze the logs stored in a specified bucket. This feature helps you audit the operations performed on OSS, check access statistics, track exceptions, and locate problems.

A Log Service project is associated with the bucket.

You can associate only existing Log Service projects with a bucket to analyze the logs of the bucket. If no Log Service projects are associated with a bucket, we recommend that you use the real-time log query feature to query and analyze OSS access logs of the last seven days free of charge. For more information, see [Real-time log query](/intl.en-US/Console User Guide/Manage buckets/Manage logs/Configure real-time log query.md).

**Note:** You are charged for Log Service if you use the log analysis feature. For more information, see the[pricing page](https://www.alibabacloud.com/product/log-service/pricing) and [Billing overview](/intl.en-US/Pricing/Billing overview.md).

## Procedures

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Recommended Services**. On the page that appears, click the **More Services** tab.

3.  Move the pointer over the **Log Service** section and click **Configure**.

4.  On the **Log Service** page, click **Analyze Log** in the Actions column corresponding to the project that you want to use to analyze the logs in the associated bucket.

    To analyze logs stored in other buckets, click **Associate Buckets** in the Actions column corresponding to the project that you want to use to analyze logs to associate the project with the buckets.

5.  Use statements to analyze logs.

    For example, you can use the following statement to query the number of GET requests in the logs: `* and GET | SELECT COUNT(*) as pv`. For more information, see [Search syntax](/intl.en-US/Index and query/Query/Search syntax.md).


