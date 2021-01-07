# Overview of the OSS console

The OSS console is a web-based graphical user interface \(GUI\) platform used to manage OSS. The GUI provides an easy and convenient method to manage your OSS resources. This topic provides an overview of the OSS console.

## Overview page

The Overview page displays an overview of all your OSS buckets.

-   **Basic Statistics**

    In the **Basic Statistics** section, you can view the statistical values for **Storage Usage**, **Traffic This Month**, and **Requests This Month** of all buckets in your account.

    **Note:** The data on this page is not updated in real time. There is a latency of one to two hours. These statistics are for reference only.

    -   **Storage Usage**: shows the storage usage of all buckets in your account.
        -   **Total \(Excluding ECS Snapshots\)**: shows the storage usage of all storage classes of buckets, excluding ECS snapshots.
        -   **Standard \(Locally Redundant Storage\)**: shows the storage usage of Standard locally redundant storage \(LRS\) objects. For more information about storage classes, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md).
        -   **Standard \(Zone-redundant Storage\)**: shows the storage usage of Standard zone-redundant storage \(ZRS\) objects. If ZRS is enabled for a bucket, the redundancy type of objects in the bucket is ZRS. For more information, see [ZRS](/intl.en-US/Developer Guide/Data security/Disaster recovery/ZRS.md).
        -   **IA \(Locally Redundant Storage\)**: shows the storage usage of IA LRS objects. The storage usage is displayed based on **Actual Storage Usage** and **Billed Storage Usage**.
            -   **Actual Storage Usage**: shows the actual storage usage of IA objects.
            -   **Billed Storage Usage**: shows the billed storage usage of IA objects. The minimum billable size for IA objects is 64 KB. Objects smaller than 64 KB are charged as 64 KB. Therefore, if a bucket contains a large number of small objects, **billed storage usage** may be greater than **actual storage usage**. For more information, see [t4341.md\#section\_t3z\_lxt\_tdb](/intl.en-US/Developer Guide/Storage classes/Overview.mdsection_t3z_lxt_tdb).
        -   **IA \(Zone-redundant Storage\)**: shows the storage usage of IA ZRS objects. If ZRS is enabled for a bucket, the redundancy type of objects in the bucket is ZRS. The storage usage is displayed based on **Actual Storage Usage** and **Billed Storage Usage**. The storage usage for IA ZRS is calculated in the same way as that for IA LAS.
        -   **Archive**: shows the storage usage of Archive objects. The storage usage is displayed based on **Actual Storage Usage** and **Billed Storage Usage**. The storage usage for Archive is calculated in the same way as that for IA LAS.
        -   **Cold Archive**: shows the storage usage of Cold Archive objects. The storage usage is displayed based on **Actual Storage Usage** and **Billed Storage Usage**. The storage usage for Cold Archive is calculated in the same way as that for IA LAS.
        -   **ECS Snapshots**: shows the storage usage of ECS snapshots. For more information about the ECS snapshot feature, see [Snapshot overview](/intl.en-US/Snapshots/Snapshot overview.md).
    -   **Traffic This Month**: shows the total traffic usage in the current month of all buckets in your account.
        -   **Outbound Traffic Over Public Network**: shows the traffic generated when you browse or download OSS data over the Internet.
        -   **Inbound Traffic Over Public Network**: shows the traffic generated when you upload data to OSS over the Internet.
        -   **CDN Back-to-Origin Traffic**: shows the back-to-origin traffic generated when you browse or download OSS data over CDN.
    -   **Requests This Month**: displays the total number of API requests received in the current month by all buckets in your account.
        -   **Read Requests**: shows all GET requests. For more information, see the "GET requests" section of the [View resource usage](/intl.en-US/Console User Guide/Manage buckets/View resource usage.md) topic.
        -   **Write Requests**: shows all PUT requests. For more information, see the "PUT requests" section of the [View resource usage](/intl.en-US/Console User Guide/Manage buckets/View resource usage.md) topic.
-   **Basic Settings**

    In the **Basic Settings** section, you can view the basic configurations of all buckets.

    -   **Versioning**: When versioning is enabled for a bucket, data that is overwritten or deleted is saved as a previous version. Versioning allows you to recover objects in a bucket to any previous version, and protects your data from being accidentally overwritten or deleted. For more information about versioning, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md).
    -   **Transfer Acceleration**: helps you accelerate the access to OSS buckets and is ideal for scenarios such as remote data transmission and uploads and downloads of large objects of gigabytes or terabytes in size. For more information about transfer acceleration, see [Transfer acceleration](/intl.en-US/Developer Guide/Buckets/Transfer acceleration.md).
    -   **Cross-Region Replication**: You can click **View Details** to view all buckets that have cross-region replication \(CRR\) configured and the existing CRR rules. You can click **Configure** in the Actions column corresponding to a bucket to go to the **Cross-Region Replication** section of the bucket. For more information about how to configure CRR, see [Configure CRR](/intl.en-US/Console User Guide/Manage buckets/Redundancy for fault tolerance/Configure CRR.md).
    -   **Domain Names**: You can click **View Details** to view all buckets that have custom domain names bound. You can click **Configure** in the Actions column corresponding to a bucket to go to the **Domain Names** tab of the bucket. For more information about how to bind a custom domain name, see [Bind custom domain names](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind custom domain names.md).
    -   **Event Notification**: You can click **View Details** to view all buckets that have event notification configured and the configured event notification rules. You can click **Configure** in the Actions column corresponding to a bucket to go to the **Event Notification** page of the bucket. For more information about how to configure event notification, see [Configure event notification](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure event notification.md).
    -   **Security Token \(Authorization for RAM Users\)**: Click **RAM Console**. On the Quick Security Token Configuration page, click **Start Authorization** to create a RAM user and a role for authorization by using STS. For more information about authorization by using STS, see [Access OSS with a temporary access credential provided by STS](/intl.en-US/Developer Guide/Data security/Access and control/Access OSS with a temporary access credential provided by STS.md).
-   **Data Processing**

    In the **Data Processing** section, you can view the data processing services of OSS. You can use these services to process your objects stored in OSS. You can click **Help** for each of these services to view the details of these features.

-   **Bucket Management**

    In the Bucket Management section, you can view the number of buckets and perform simple operations on buckets.

    -   **Create Bucket**: You can click this button to create a bucket. For more information, see [Create buckets](/intl.en-US/Console User Guide/Manage buckets/Create buckets.md).
    -   **View Buckets**: You can click this button to go to the **Buckets** page.
    -   **Export CSV**: You can click this button to export the bucket list to a local computer. This list includes bucket information such as the bucket name, region, storage class, and creation time.
-   **Frequently Accessed**: shows common functions and documentations.
-   **Alert Rules**: shows basic information about the alert rules configured for your buckets. You can click **Manage** to configure alert rules for the buckets. For more information, see [Alert service](/intl.en-US/Developer Guide/Monitoring service/Alert service.md).

## Buckets

The Buckets page shows all the buckets that you created. You can view the statistical information of your buckets.

-   **Create Bucket**: You can click this button to create a bucket. For more information, see [Create buckets](/intl.en-US/Console User Guide/Manage buckets/Create buckets.md).
-   **Bucket Name** search box: You can enter a bucket name in the search box to search for the specified bucket. Fuzzy match is supported.
-   **Region** drop-down list: You can select a region from the drop-down list to view all the buckets in this region.
-   **Storage Class** drop-down list: You can select a storage class from the drop-down list to view all the buckets of the specified storage class.
-   ![Export CSV](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3667549951/p87165.png): You can click this icon to export the bucket list to a local file. The list includes bucket information such as the bucket name, region, storage class, and creation time.
-   ![Refresh](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3667549951/p87166.png): You can click this icon to refresh the bucket list.

The bucket list section displays bucket information such as the bucket name, tag, region, storage class, redundancy option, capacity, traffic of the current month, visits of the current month, storage usage of each storage class of objects, versioning status, and transfer acceleration status. In the bucket list:

-   You can click the ![Sort order ](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3667549951/p87167.png) icon next to Bucket Name to sort the buckets in alphabetical order or reverse alphabetical order.
-   You can click **Add** in the **Bucket Tagging** column corresponding to a bucket to add tags to the bucket. For more information about tagging, see [Configure bucket tagging](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure bucket tagging.md).
-   You can click the switch in the **Versioning** column corresponding to a bucket to set the versioning status for the bucket. For more information about versioning, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md).
-   You can click the switch in the **Transfer Acceleration** column corresponding to a bucket to enable or disable transfer acceleration for the bucket. For more information about transfer acceleration, see [Transfer acceleration](/intl.en-US/Developer Guide/Buckets/Transfer acceleration.md).

## My OSS Paths

In the My OSS Paths section, you can manually add OSS paths or configure a policy to automatically record recently accessed OSS paths to access target OSS resources.

-   ![Configure](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3667549951/p101539.png): Click this icon. You can configure whether to automatically record the most recently accessed OSS paths in the Manage Paths panel.
    -   **Most Recently Accessed**: Select whether to record the recently accessed OSS paths.
    -   **Preserved Paths**: Specify the number of recorded OSS paths. Default value: 10. Valid values: 1 to 10.
-   ![Add Authorized OSS Path](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3667549951/p101541.png): You can click this icon to manually add OSS paths.
    -   **Region**: Set the region of the bucket.
    -   **Bucket**: Enter the name of a bucket that you are authorized to access.
    -   **Select Authorized Bucket**: Click to select a bucket that you are authorized to access.
    -   **File Path**: Enter the path that you are authorized to access. For example, if you are authorized to access path in the root path, enter path. To access the entire bucket, leave File Path unspecified.

After you add an OSS path, you can click the path to access it. You can click the ![Sticky](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3667549951/p101551.png) icon on the right of a path to sticky the path to the top. You can also click the ![Delete](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3667549951/p101553.png) icon on the right of a path to delete the path.

## Resource Plans

On the Resource Plans page, you can purchase, renew, and upgrade resource plans as well as view plan usage.

-   **Purchase**: You can click this button to purchase a resource plan. For more information, see [Overview](/intl.en-US/Pricing/Billing methods/Subscription/Overview.md).
-   **View Fees**: You can click this button to view the details of all your resource plans, including the expired resource plans.
-   **View Plan Usage**: You can click this button to view the usage of your resource plans.
-   Resource Plans: shows the resource plans that you purchase and have not expired.
    -   You can click **Upgrade** next to the corresponding resource plans to upgrade your resource plans. For more information, see [Upgrade resource plans](/intl.en-US/Pricing/Billing methods/Subscription/Overview.mdsection_nnt_zug_lsv).
    -   You can click **Renew** next to the corresponding resource plans to renew your resource plans. For more information, see [Renew resource plans](/intl.en-US/Pricing/Billing methods/Subscription/Overview.mdsection_axy_bot_o4m).

## Cross-region Replication

In the left-side navigation pane, you can click Cross-Region Replication to view the CRR rules that are configured for the buckets in the current account. You can click **Configure** in the Actions column corresponding to a CRR rule to configure the rule. For more information about how to configure CRR, see [Configure CRR](/intl.en-US/Console User Guide/Manage buckets/Redundancy for fault tolerance/Configure CRR.md).

## Tools

In the left-side navigation pane, you can click Tools to view the OSS tools that are recommended to help you manage your OSS resources. You can click **Help** for a tool to view more information about how to use the tool.

## Data Import

In the left-side navigation pane, you can click Data Import to view the services that are recommended to help you migrate multiple data to OSS. You can click **Help** for a service to view more information about how to use the service.

## Recommended Services

In the left-side navigation pane, you can click Recommended Services to view the common services that are recommended for you to use with OSS. You can click **Help** for a service to view more information about the service.

