# Configure event notification rules

You can configure event notification rules in the OSS console for objects that you want to monitor. If the events that you specified in the rules occur on these objects, OSS immediately sends you a notification.

Message Service \(MNS\) is activated.

You can go to the[MNS product page](https://www.alibabacloud.com/product/message-service?spm=a2796.7919406.6791778070.535.10863c37LYZmSP) to activate MNS.

When you use the event notification feature, take note of the following items:

-   Fees for MNS are incurred when you use event notification. For more information, see [t1835565.dita\#concept\_2028746]().
-   The event notification feature is available in all regions except for the China \(Heyuan\), China \(Guangzhou\), China \(Hohhot\), China \(Ulanqab\), UAE \(Dubai\), and Malaysia \(Kuala Lumpur\) regions.
-   You can configure up to 10 event notification rules in a region.
-   Notifications are not sent for TS and M3U8 objects that are generated in RTMP-based stream ingest. For more information about RTMP-based stream ingest, see[Overview](/intl.en-US/API Reference/LiveChannel-related operations/Overview.md).

For more information about event notification, see [Event notification](/intl.en-US/Developer Guide/Event notification.md).

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket for which you want to configure event notification rules.

3.  In the left-side navigation pane, choose **Basic Settings** \> **Event Notification**.

4.  In the Event Notification section, click **Configure**. On the page that appears, click **Create Rule**.

5.  In the Create Rule panel, configure the rule parameters. The parameters are described in the following table:

    |Parameter|Description|
    |---------|-----------|
    |**Rule Name**|Specify the name of the event notification rule.The name of an event notification rule can contain only letters, digits, and hyphens \(-\). The name cannot exceed 85 characters in length. |
    |**Events**|Select one or more events that can trigger the event notification rule from the drop-down list. For example, if you select CopyObject, the event notification rule is triggered when specific objects are created or overwritten by copying objects.If multiple event notification rules apply to the same object, the values of this parameter in these rules must be different. For example, if you configure Events as **CopyObject** when you create an event notification rule for objects whose names contain the examplefolder prefix, you cannot select **CopyObject** as the value for Events when you create another event notification rule for objects whose names contain the same prefix.

For more information about the events, see [Event types](/intl.en-US/Developer Guide/Event notification.md). |
    |**Resource Description**|Specify the objects to which the event notification rule applies.    -   **Full Name**: Enter the complete path of the object to which the event notification rule applies. Example: examplefolder/myphoto.jpg.
    -   **Prefix and Suffix**: Enter the prefix and suffix of the objects to which the event notification rule applies. The following examples show how to configure the prefix and suffix:
        -   To create a rule that applies to all objects in the bucket, leave Prefix and Suffix empty.
        -   To create a rule that applies to all objects in the examplefolder folder in the root folder of the bucket, set Prefix to **examplefolder/** and leave Suffix empty.
        -   To create a rule that applies to all JPG objects in the bucket, leave Prefix empty and set Suffix to **.jpg**.
        -   To create a rule that applies to all MP3 objects in the examplefolder folder in the root folder of the bucket, set Prefix to **examplefolder/** and Suffix to **.mp3**.
To create a Resource Description, click **Add**. You can create up to five **Resource Description** entries. |
    |**Endpoint**|Configure the endpoint to which notifications are sent. Valid values: **HTTP** and **Queue**.    -   **HTTP**: Enter the address of the HTTP endpoint to which notifications are sent. Example: `http://198.51.100.1:8080`. For more information about how to enable an HTTP endpoint, see [Manage topics]() and [HttpEndpoint]().
    -   **Queue**: Enter the name of an MNS queue. For more information about how to create a queue, see [Create a queue]().
To create an endpoint, click **Add**. You can create up to five endpoints. |

6.  Click **OK**.


