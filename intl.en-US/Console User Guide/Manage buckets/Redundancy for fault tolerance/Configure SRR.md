# Configure SRR

Same-region replication \(SRR\) allows you to replicate objects across buckets within the same region in an automatic and asynchronous \(near real-time\) manner. Operations such as the creation, overwriting, and deletion of objects can be synchronized from a source bucket to destination buckets.

When you use SRR, take note of the following items:

-   Billing
    -   OSS charges you for the traffic generated when you use SRR to replicate objects. For more information about the billing methods, see [Traffic fees](/intl.en-US/Pricing/Billing items and methods/Traffic fees.md).
    -   Each time an object is synchronized, OSS counts the number of requests and charges you on a pay-as-you-go basis. For more information about the billing methods, see [API operation calling fees](/intl.en-US/Pricing/Billing items and methods/API operation calling fees.md).
-   Limits
    -   You can configure SRR rules to synchronize data from a source bucket to multiple destination buckets in the same region. You can configure up to 100 SRR rules for a bucket. A bucket can be configured as the source bucket or destination bucket. If your business requires more than 100 SRR rules for a bucket, contact the [technical support](https://workorder-intl.console.aliyun.com/#/ticket/createIndex).
    -   The source bucket and destination bucket must be both versioned or unversioned. The versioning state of the source bucket and destination bucket cannot be changed when an SRR rule is configured between the two buckets.

For more information about same-region replication, see [Same-region replication](/intl.en-US/Developer Guide/Data security/Disaster recovery/Same-region replication.md).

## Enable SRR

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the bucket for which you want to enable SRR.

3.  In the left-side navigation pane, choose **Redundancy for Fault Tolerance** \> **Same-Region Replication**.

4.  In the **Same-Region Replication** section, click **Configure**.

5.  Click **Same-Region Replication**.

6.  In the **Same-Region Replication** panel, configure the parameters described in the following table.

    |Parameter|Description|
    |---------|-----------|
    |**Source Region**|The region in which the current bucket is located.|
    |**Source Bucket**|The name of the current bucket.|
    |**Destination Bucket**|Select the destination bucket to which you want to synchronize data.|
    |**Applied To**|Select the source data that you want to synchronize.     -   **All Files in Source Bucket**: synchronizes all objects from the source bucket to the destination bucket.
    -   **Files with Specified Prefix**: synchronizes the objects whose names contain the specified prefix from the source bucket to the destination bucket. You can specify up to 10 prefixes. |
    |**Object Tagging**|Objects that have the specified tags are synchronized to the destination bucket. Select **Configure Rules** and add tags \(key-value pairs\). You can add up to 10 tags. When you set this parameter, make sure that the following conditions are met:

    -   Object tags are configured. For more information, see [Configure object tagging](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure object tagging.md).
    -   Versioning is enabled for the source bucket and destination bucket.
    -   Operations is set to **Add/Change**. |
    |**Operations**|Select the synchronization policy.     -   **Add/Change**: synchronizes only the added or changed data from the source bucket to the destination bucket.
    -   **Add/Delete/Change**: synchronizes all data changes such as the creation, overwriting, and deletion of objects from the source bucket to the destination bucket.
For more information about how to configure CRR for objects in versioned buckets, see [Cross-region replication in specific scenarios](/intl.en-US/Developer Guide/Data security/Disaster recovery/Cross-region replication in specific scenarios.md). |
    |**Replicate Historical Data**|Specify whether to synchronize historical data generated before the SRR rule takes effect.     -   **Yes**: synchronizes historical data to the destination bucket.

**Note:** When historical data is synchronized, objects in the source bucket may overwrite objects in the destination bucket if these objects have the same names. To avoid data loss, we recommend that you enable versioning for the source and destination buckets.

    -   **No**: synchronizes only objects that are uploaded or updated after the SRR rule takes effect to the destination bucket. |
    |**KMS-based Encryption**|If KMS-based encryption is configured for the source objects or destination bucket, you must select **KMS-based Encryption** and configure the following parameters:    -   **CMK**: specifies a customer master key \(CMK\) used to encrypt the destination object.

To use a CMK to encrypt objects, you must create the CMK in the same region as the destination bucket. For more information, see [Manage CMKs]().

    -   **Authorized RAM Role**: authorizes a RAM role to perform KMS-based encryption on the destination objects.
        -   **New RAM Role**: A RAM role is created to perform KMS-based encryption on destination objects. The role name is in the following format: `kms-replication-source bucket name-destination bucket name`.
        -   **AliyunOSSRole**: The AliyunOSSRole role is used to perform KMS-based encryption on destination objects. If the AliyunOSSRole role does not exist, OSS automatically creates the AliyunOSSRole role when you select this option.
**Note:** You can use [HeadObject](/intl.en-US/API Reference/Object operations/Basic operations/HeadObject.md) to query the encryption status of the source object and [GetBucketEncryption](/intl.en-US/API Reference/Bucket operations/Encryption/GetBucketEncryption.md) to query the encryption status of the destination bucket. |

7.  Click **OK**.

    -   An SRR rule cannot be edited or deleted after it is created.
    -   The synchronization task starts immediately after an SRR rule is configured. You can view the synchronization progress on the Same-Region Replication page.
    -   It takes several minutes to several hours for the data to be replicated to the destination bucket based on the amount of data.

## Disable SRR

You can click **Disable** to disable SRR.

![sync](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4658906061/p135995.png)

After SRR is disabled, the replicated data is stored in the destination bucket, and the incremental data in the source bucket is not replicated to the destination bucket.

