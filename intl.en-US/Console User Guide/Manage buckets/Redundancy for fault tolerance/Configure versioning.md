# Configure versioning

OSS allows you to configure versioning for a bucket to protect the data stored in the bucket. After you enable versioning for a bucket, data that is overwritten or deleted in the bucket is saved as a previous version. You can recover objects in a versioned bucket to any previous version to protect your data from being accidentally overwritten or deleted.

When versioning is enabled for a bucket, OSS specifies a unique ID for each version of all objects in the bucket. You can download or recover a previous object version based on its version ID. For more information, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md).

If you enable versioning for a bucket, you are charged only for the storage of the current versions and previous versions of all objects in the bucket. If you download a previous version of an object or recover the previous version as the current version, fees are incurred by the requests and traffic. To avoid unnecessary storage costs, we recommend that you delete the previous versions of objects that you no longer need. For more information, see [Billing items and methods](/intl.en-US/Pricing/Billing items and methods/Overview.md).

## Enable versioning

-   Enable versioning when you create a bucket
    1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
    2.  On the **Overview** page, click **Create Bucket** in the upper-right corner.
    3.  In the Create Bucket panel, configure parameters.

        Set **Versioning** to **Enable**. For more information about how to configure other parameters, see [Create buckets](/intl.en-US/Console User Guide/Manage buckets/Create buckets.md).

    4.  Click **OK**.
-   Enable versioning for an existing bucket
    1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
    2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket for which you want to enable versioning.
    3.  Choose **Redundancy for Fault Tolerance** \> **Versioning**.

        You can also click **Buckets** in the left-side navigation pane and turn on **Versioning** for the corresponding bucket.****

    4.  Click **Configure**. Then, click **Enabled**.

        You can also suspend versioning for a versioned bucket to stop generating new object versions. If a new version is added to an object in a versioning-suspended bucket, OSS specifies a version ID of null for the new version and retains the previous versions of the object.

    5.  Click **Save**.

## Recover previous versions

You can recover a specified previous version of an object as the current version.

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket that contains the previous version you want to recover.

3.  Click **Files**. In the upper-right corner, set **Display Previous Versions** to **Show**.

4.  Click **Restore** in the Actions column corresponding to the previous version that you want to recover.

    OSS recovers the previous version as the current version of the object.


## Download previous versions

You can download a previous version of an object.

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket that contains the previous version you want to download.

3.  Click **Files**. In the upper-right corner, set **Display Previous Versions** to **Show**.

4.  Click the object version you want to download. In the panel that appears, click **Download** on the right side of **Signed URL**.

5.  Select the location where you want to store the version and click **Save**.


## Delete previous versions

We recommend that you delete previous versions that you no longer need in a timely manner to minimize storage costs.

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket that contains the previous version you want to delete.

3.  Click **Files**. In the upper-right corner, set **Display Previous Versions** to **Show**.

4.  Click **Completely Delete** in the Actions column corresponding to the previous version you want to delete.

    To delete multiple unnecessary previous versions, select the previous versions you want to delete and choose **Batch Operation** \> **Completely Delete**.

5.  Click **OK**.


**Note:**

-   Deleted previous versions cannot be recovered. Exercise caution when you delete previous versions.
-   If you delete the current version of an object, the latest previous version becomes the current version.
-   You can also configure lifecycle rules to automatically delete previous versions on a regular basis. For more information about how to configure lifecycle rules, see [Configure lifecycle rules](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure lifecycle rules.md).

