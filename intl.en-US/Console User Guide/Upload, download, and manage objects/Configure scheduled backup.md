# Configure scheduled backup

You can use the scheduled backup feature provided by Object Storage Service \(OSS\) to back up objects in a bucket to Hybrid Backup Recovery \(HBR\) on a regular basis. If an object is unexpectedly lost, you can recover the object from HBR.

-   HBR is activated. You can go to the [HBR product page](https://www.alibabacloud.com/zh/products/hybrid-backup-recovery) to activate HBR.
-   HBR is authorized to read data from OSS. Log on to the [HBR console](https://hbr.console.aliyun.com). In the left-side navigation pane, click **OSS Backup**. Follow the prompts to authorize HBR.

## Billing

When you use scheduled backup, you are charged for the following fees:

-   Fees for access requests to OSS

    -   Before each backup task starts, HBR uses the ListObject operation to obtain the list of the objects to back up. Each ListObject request can list up to 1,000 objects.
    -   After the object list is obtained, HBR uses the HeadObject operation to obtain the metadata of the objects to back up. Each HeadObject request can obtain the metadata of one object.
    -   HBR uses the GetObject operation to back up objects. Each GetObject request can back up one object.
    -   HBR uses the ListObject operation to query the backup progress and verify the objects that are backed up. Each ListObject request can verify up to 1,000 objects.
    For more information about the billing methods for OSS API requests, see [API operation calling fees](/intl.en-US/Pricing/Billing items and methods/API operation calling fees.md).

-   If you back up an Infrequent Access \(IA\) object, you are charged for data retrieval fees. For more information about the billing methods, see [Data processing fees](/intl.en-US/Pricing/Billing items and methods/Data processing fees.md).
-   You are charged for the objects stored in HBR vaults. For information about the billing methods, see [Billing methods and billable items](/intl.en-US/Pricing/Billing methods and billable items.md).

    If you use the default configurations for scheduled backup, the storage of backups in HBR vaults is free for the first 30 days.


## Limits

-   Scheduled backup cannot be configured for buckets whose storage classes are Archive or Cold Archive.
-   The backup and recovery of symbolic links, Archive and Cold Archive objects, and the ACL of objects are not supported.
-   The scheduled backup feature is supported only in the following regions: China \(Hangzhou\), China \(Shanghai\), China \(Shenzhen\), China \(Beijing\), China \(Zhangjiakou\), China \(Hong Kong\), Singapore, Australia \(Sydney\), Indonesia \(Jakarta\), and US \(Silicon Valley\).

## Method 1: Configure scheduled backup for an existing bucket

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the bucket for which you want to configure scheduled backup.

3.  In the left-side navigation pane, choose **Files** \> **Scheduled Backup**.

4.  Click **Source Bucket**.

5.  Configure a backup schedule.

    -   Configure a free backup schedule.

        HBR provides a free trial period of 30 days for you to use scheduled backup for data stored in OSS. If you want to try the scheduled backup feature free of charge, configure the parameters described in the following table in the Create Backup Plan panel.

        |Parameter|Description|
        |---------|-----------|
        |**Source OSS Bucket**|The name of the current bucket.|
        |**Plan Name**|Specify the name of the backup plan. The name can be up to 64 characters in length. You can leave this parameter unspecified. In this case, HBR generates a backup plan name based on the current time. |
        |**Start Time**|Select the start time of the backup plan.|
        |**Pay After Trial Ends**|Specifies whether to pay for the backup plan after its free trial ends.         -   **No**: The backup plan automatically stops after the free trial period. The objects that are backed up are deleted one week after the backup plan stops.
        -   **Yes**: The backup plan continues to be executed after the free trial period. HBR charges the storage fees for the objects that are backed up. |

        The preceding configuration creates a backup plan that backs up the objects once every day and retains the backups for one week.

    -   Configure a paid backup plan.

        You can configure a paid backup plan to specify the backup interval, the objects to back up, and the number of days to retain backups.

        1.  In the Create Backup Plan panel, click **Show Advanced Settings**.
        2.  Click **Switch to Paid Plan**. Then, click **OK** in the message that appears.
        3.  Configure the parameters described in the following table.

            |Parameter|Description|
            |---------|-----------|
            |**Source OSS Bucket**|The name of the current bucket.|
            |**Plan Name**|Set the name of the backup plan. The name can be up to 64 characters in length. You can leave this parameter unspecified. In this case, HBR generates a backup plan name based on the current time. |
            |**Start Time**|The start time of the backup plan.|
            |**Source Path**|Set the prefix of the objects to back up. If you leave this parameter empty, all objects in the bucket are backed up.|
            |**Backup Interval**|Set the execution frequency of the backup task. Unit: days or weeks.|
            |**Retention Policy**|Set the retention policy for the backups.             -   **Limited**: HBR retains the objects that are backed up for the specified period of time and automatically deletes the objects that are backed up after they expire.
            -   **Permanent**: Objects that are backed up are permanently stored. |
            |**Retention Period**|Set the retention period of the objects that are backed up. Units: days, weeks, months, and years. This parameter can be set only when you set **Retention Policy** to **Limited**. |
            |**Backup Vault**|Set the backup vault in which objects that are backed up are stored.             -   **Create Vault**: If no vaults exist in HBR, or you want to store the objects that are backed up in a new vault, you can create a backup vault.
            -   **Select Vault**: If you have created vaults in HBR, you can select an existing vault to store the objects that are backed up. |

6.  Click **OK**.


## Method 2: Configure scheduled backup when you create a bucket

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click **Create Bucket**.

3.  In the Create Bucket panel, configure parameters.

    Set **Scheduled Backup** to **Enable**. For more information about other parameters, see [Create buckets](/intl.en-US/Console User Guide/Manage buckets/Create buckets.md).

4.  Click **OK**.

    After the bucket is created, a backup plan is automatically created to back up data once a day and retain the objects that are backed up for one week. The backup plan has a free trial duration of 30 days. You can choose **Files** \> **Scheduled Backup** to view or modify the created plan.


## References

You can create recovery tasks to recover objects that are backed up to OSS. For more information about how to create recovery tasks, see [Back up and restore OSS objects](/intl.en-US/Back up OSS/Back up and restore OSS objects.md).

