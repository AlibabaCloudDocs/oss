# Versioning

OSS allows you to configure versioning to protect data in buckets. When versioning is enabled for a bucket, bucket data that is overwritten or deleted is saved as a previous version. You can use versioning to recover an object in a bucket to a previous version and protect your data from being accidentally overwritten or deleted.

When versioning is enabled, OSS specifies a unique ID for each version of all objects in a bucket. You can also download a previous version of an object or recover the previous version as the current version at any time based on the version ID. For more information, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md).

**Note:** If you enable versioning for a bucket, you are charged only for the storage of the current versions and previous versions of all objects in the bucket. To avoid unnecessary storage costs, we recommend that you delete the previous versions of objects that you no longer need. If you download a previous version of an object or recover the previous version as the current version, fees are incurred by the requests and traffic. For more information, see [Billing items and methods](/intl.en-US/Pricing/Billing items and methods/Overview.md).

## Enable versioning

-   Enable versioning when you create a bucket
    1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
    2.  On the **Overview** page, click **Create Bucket** in the upper-right corner.
    3.  In the Create Bucket panel, configure parameters.

        Set **Versioning** to **Enable**. For more information about parameter descriptions, see [Create buckets](/intl.en-US/Console User Guide/Manage buckets/Create buckets.md).

    4.  Click **OK**.
-   Enable versioning for an existing bucket
    1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
    2.  Click **Buckets**. On the Buckets page, click the name of a bucket.
    3.  Choose **Redundancy for Fault Tolerance** \> **Versioning**.

        You can also click **Buckets**. Then, turn on **Versioning** for the corresponding bucket.****

    4.  Click **Configure**. Select **Enabled**.

        **Note:** If you no longer use more object versions, you can select **Suspended** after you enable versioning for a bucket. OSS specifies that the ID of a new version is null and no more previous versions are generated. However, the existing versions are retained.

    5.  Click **Save**.

## Recover previous versions

You can recover a specified previous version of an object as the current version.

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**. On the Buckets page, click the name of a bucket.

3.  Click **Files**. In the upper-right corner, set **Display Previous Versions** to **Show**.

4.  Click **Restore** in the Actions column corresponding to a version. OSS copies the specified version of the object to the current folder. The existing current version is overwritten. The specified version becomes the current version.


## Download previous versions

You can download a previous version of an object.

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**. On the Buckets page, click the name of a bucket.

3.  Click **Files**. In the upper-right corner, set **Display Previous Versions** to **Show**.

4.  Click the object version. In the panel that appears, click **Download** in the **Signed URL** session.

5.  Set the location where you want to store the object, and click **Save**.


## Delete previous versions

We recommend that you delete unnecessary previous versions in a timely manner to minimize storage costs.

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**. On the Buckets page, click the name of a bucket.

3.  Click **Files**. In the upper-right corner, set **Display Previous Versions** to **Show**.

4.  Click **Completely Delete** in the Actions column corresponding to the previous version you want to delete.

    To delete multiple unnecessary previous versions, choose **Batch Operation** \> **Completely Delete**.

5.  Click **OK**.


**Note:**

-   Previous versions cannot be recovered after they are deleted. Exercise caution when you perform this operation.
-   If you delete the current version, the latest previous version becomes the current version.
-   You can also configure lifecycle rules to automatically delete previous versions on a regular basis. For more information, see [Configure lifecycle rules](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure lifecycle rules.md).

