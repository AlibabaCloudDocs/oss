# Configure versioning

OSS allows you to configure versioning for a bucket to protect objects stored in the bucket. After you enable versioning for a bucket, objects that are overwritten or deleted in the bucket are saved as previous versions. You can use versioning to recover a previous version of an object that was accidentally overwritten or deleted.

If you enable versioning for a bucket, you are charged for the storage of previous versions of objects in the bucket. If you download or recover a previous version of an object, you are charged for the requests and traffic. To avoid unnecessary storage costs, we recommend that you delete the previous versions of objects that you no longer need at your earliest opportunity. For more information, see [Billing items and methods](/intl.en-US/Pricing/Billing items and methods/Overview.md).

## Enable versioning

When versioning is enabled for a bucket, OSS specifies a unique ID for each version of an object stored in the bucket.

-   Enable versioning when you create a bucket
    1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
    2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click **Create Bucket**.
    3.  In the Create Bucket panel, configure parameters.

        Set **Versioning** to **Enable**. For more information about how to configure other parameters, see [Create buckets](/intl.en-US/Console User Guide/Manage buckets/Create buckets.md).

    4.  Click **OK**.
-   Enable versioning for an existing bucket
    1.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket for which you want to enable versioning.
    2.  Choose **Redundancy for Fault Tolerance** \> **Versioning**.
    3.  Click **Configure**. Set Versioning to **Enabled**.
    4.  Click **Save**.

After versioning is enabled for a bucket, you can view all versions of objects in the bucket on the Files page. If you want to view only the current versions of objects, set **Display Previous Versions** to **Hide**. Listing previous versions of objects do not affect the performance of the console. If the response time of the console is slow when you list objects, see [FAQ](/intl.en-US/Developer Guide/Data security/Versioning/FAQ.md) to troubleshoot the problem.

## Recover previous versions

You can recover a specified previous version of an object as the current version.

1.  On the Overview page, click **Files** in the left-side navigation pane.

2.  Recover a specified previous version as the current version.

    **Note:** You can recover only one previous version of an object at a time. The previous version that you want to recover cannot be a delete marker.

    -   Recover the previous version of an object

        Click **Restore** in the Actions column that corresponds to the previous version that you want to recover.

    -   Recover the previous versions of multiple objects

        Select the previous versions that you want to recover and then choose **Batch Operation** \> **Restore**.


## Download a specified version of an object

You can download a specified previous version of an object.

1.  On the Overview page, click **Files** in the left-side navigation pane.

2.  Click the version that you want to download. In the panel that appears, click **Download** to the right of **Object URL**.

3.  Select the location where you want to store the downloaded version and then click **Save**.


## Delete the previous version of an object

To minimize storage costs, we recommend that you delete previous versions that you no longer need at your earliest opportunity.

**Warning:** Deleted previous versions cannot be recovered. Exercise caution when you delete previous versions.

1.  On the Overview page, click **Files** in the left-side navigation pane.

2.  Click **Completely Delete** in the Actions column that corresponds to the previous version you want to delete.

    To delete multiple unnecessary previous versions, select the previous versions you want to delete and choose **Batch Operation** \> **Completely Delete**.

3.  Click **OK**.


You can also configure lifecycle rules to allow OSS to delete previous versions on a regular basis. For more information, see [Configure lifecycle rules](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure lifecycle rules.md).

## Suspend versioning

You can suspend versioning for a versioned bucket to stop OSS from generating new versions for objects. If a new version is generated for an object in a versioning-suspended bucket, OSS sets the ID of the new version to null and retains the previous versions of the object.

You can perform the following steps to suspend versioning for a bucket:

1.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket for which you want to suspend versioning.

2.  Choose **Redundancy for Fault Tolerance** \> **Versioning**.

3.  Click **Configure** and then set the versioning state to **Suspended**.

4.  Click **Save**.


