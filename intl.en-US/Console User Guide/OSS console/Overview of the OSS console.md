# Overview of the OSS console

The OSS console is a web-based platform that allows you to easily manage your OSS resources. You can view the general information about all of your buckets on the overview page of the console, such as the quantity, storage usage, and traffic of your buckets and API requests sent to the buckets.

## Get started with OSS

If you use OSS for the first time, we recommend that you click **Quick Start** in the upper-right corner of the Overview page in the OSS console to learn the basic knowledge of OSS.

## Query the resource usage of buckets

You can use the following methods to query the resource usage of all buckets owned by the current Alibaba Cloud account, including the storage and traffic used by the bucket and the number of API requests sent to the buckets.

**Note:** The data on the Overview page is not updated in real time and has a latency of 1 to 3 hours. These statistics are for reference only. To query detailed statistics of your buckets, see [t4329.md\#]().

-   Query the current storage usage of all buckets

    You can query the storage usage of all buckets in the **Basic Statistics** section, including the storage usage of Standard, IA, Archive, and ColdArchive data and ECS snapshots.

    IA, Archive, and ColdArchive storage has a minimum billable size of 64 KB for each object. Objects that are smaller than 64 KB in size are charged for 64 KB. Objects that are larger than or equal to 64 KB in size are charged based on their actual sizes. When multiple IA, Archive, or ColdArchive objects that are smaller than 64 KB in size are stored in a bucket, the billed storage capacity is larger than the actual size of all objects in the bucket. For more information about storage classes, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md).

-   Query the traffic consumed by all buckets this month

    You can query the traffic consumed by all buckets this month in the **Basic Statistics** section, including inbound and outbound traffic over the Internet and CDN back-to-origin traffic. For more information about the traffic types and billing methods, see [Traffic fees](/intl.en-US/Pricing/Billing items and methods/Traffic fees.md).

-   Query the number of API requests sent to all buckets this month

    You can query the number of API requests sent to all buckets this month in the **Basic Statistics** section, including read and write requests. For more information about the API operations used to send read and write requests, see [View resource usage](/intl.en-US/Console User Guide/Manage buckets/View resource usage.md).

-   Query the storage usage of all buckets within a specified period

    You can view the graph of the usage of different storage classes within specified periods in the **Storage Class** section. You can view only data from the last 65 days. For example, if it were December 31, 2020, you would be able to query data as far back as October 27, 2020.

-   Query the traffic consumed by all buckets within a specified period

    You can view the graph of the traffic consumed by all buckets within specified periods in the **Traffic Usage** section. You can view only data from the last 65 days. For example, if it were December 31, 2020, you would be able to query data as far back as October 27, 2020.


## Query the number of buckets

You can query the number of buckets owned by your Alibaba Cloud account by region or storage class in the Bucket Management section in the upper-right corner of the Overview page.

-   To query the number and percentage of buckets in different regions, click **Sort by Region**.
-   To query the number and percentage of buckets of different storage classes, click **Sort by Storage Class**.

To export the information about buckets such as names, regions, storage classes, and creation times to a local CSV file, click **Export CSV**.

## Download OSS tools

You can view and download tools used to manage OSS in the Tools section. For example, ossbrowser is a graphical OSS management tool that supports Windows, Linux, and macOS and ossutil is a command-line tool used to manage objects and buckets. For more information about OSS tools, see [OSS tools](/intl.en-US/Tools/OSS tools.md).

## Configure alert rules

You can configure alert rules in the Alert Rules section on the right side of the Overview page. You can specify the metrics that you want to monitor in alert rules. When the monitored metric exceeds the specified threshold, OSS immediately sends a notification to you. For more information, see [Alert service](/intl.en-US/Developer Guide/Monitoring service/Alert service.md).

