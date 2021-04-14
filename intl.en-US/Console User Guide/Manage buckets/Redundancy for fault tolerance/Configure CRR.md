# Configure CRR

Cross-region replication \(CRR\) provides automatic and asynchronous \(near real-time\) replication of objects across buckets in different OSS regions. Operations such as the creation, overwriting, and deletion of objects can be synchronized from a source bucket to a destination bucket.

When you use CRR, take note of the following items:

-   Billing
    -   OSS charges you for the traffic generated when you use CRR to replicate objects. For more information about the billing methods, see [Traffic fees](/intl.en-US/Pricing/Billing items and methods/Traffic fees.md).
    -   Each time an object is synchronized, OSS counts the number of requests and charges fees for the requests. For more information about the billing methods, see [API operation calling fees](/intl.en-US/Pricing/Billing items and methods/API operation calling fees.md).
-   Usage notes
    -   You can configure CRR rules to synchronize data from a source bucket to multiple destination buckets. You can configure up to 100 CRR rules for a bucket. A bucket can be configured as the source bucket or destination bucket. If your business requires more than 100 CRR rules for a bucket, contact the [technical support](https://workorder-intl.console.aliyun.com/#/ticket/createIndex).
    -   The source bucket and destination bucket must be both versioned or unversioned. The versioning state of the source bucket and destination bucket cannot be changed when data is being synchronized between the two buckets.

For more information about CRR, see [Cross-region replication](/intl.en-US/Developer Guide/Data security/Disaster recovery/Cross-region replication.md).

## Enable CRR

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, find the bucket for which you want to enable CRR.

3.  In the left-side navigation pane, choose **Redundancy for Fault Tolerance** \> **Cross-Region Replication**.

4.  Click **Enable**. Then, configure the parameters described in the following table in the Cross-Region Replication panel.

    |Parameter|Description|
    |---------|-----------|
    |**Source Region**|The region where the current bucket is located.|
    |**Source Bucket**|The name of the current bucket.|
    |**Destination Region**|Select the region where the destination bucket is located.|
    |**Destination Bucket**|Select the destination bucket to which you want to synchronize data.|
    |**Acceleration Type**|Only **Transfer Acceleration** is supported. Transfer acceleration can be used to increase the transfer speed when data is replicated across regions within mainland China and outside mainland China. If you enable transfer acceleration, you are charged for this feature. For information about the billing methods, see [Transfer acceleration fees](/intl.en-US/Pricing/Billing items and methods/Transfer acceleration fees.md).|
    |**Applied To**|Select the source data that you want to synchronize.     -   **All Files in Source Bucket**: synchronizes all objects from the source bucket to the destination bucket.
    -   **Files with Specified Prefix**: synchronizes the objects whose names contain the specified prefix from the source bucket to the destination bucket. You can specify up to 10 prefixes. |
    |**Object Tagging**|Objects that have the specified tags are synchronized to the destination bucket. Select **Configure Rules** and add tags \(key-value pairs\). You can add up to 10 tags. When you set this parameter, make sure that the following conditions are met:

    -   Object tags are configured. For more information, see [Configure object tagging](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure object tagging.md).
    -   Versioning is enabled for the source bucket and destination bucket.
    -   **Add/Change** is set for Operations.
    -   If the source region is China \(Hangzhou\), the destination region can be a region except for China \(Hangzhou\). If the source region is Australia \(Sydney\), the destination region can be a different region outside mainland China. |
    |**Operations**|Select the synchronization policy.     -   **Add/Change**: synchronizes only the added or changed data from the source bucket to the destination bucket.
    -   **Add/Delete/Change**: synchronizes all data changes such as the creation, overwriting, and deletion of objects from the source bucket to the destination bucket.
For more information about how to configure CRR for objects in versioned buckets, see [Cross-region replication in specific scenarios](/intl.en-US/Developer Guide/Data security/Disaster recovery/Cross-region replication in specific scenarios.md). |
    |**Replicate Historical Data**|Specify whether to synchronize historical data before you enable CRR.     -   **Yes**: synchronizes historical data to the destination bucket.

**Note:** When historical data is synchronized, objects in the source bucket may overwrite objects in the destination bucket if these objects have the same names. To avoid data loss, we recommend that you enable versioning for the source and destination buckets.

    -   **No**: synchronizes only objects that are uploaded or updated after the CRR rule takes effect to the destination bucket. |
    |**KMS-based Encryption**|If KMS-based encryption is configured for the source objects or destination bucket, you must select **KMS-based Encryption** and configure the following parameters:    -   **CMK**: specifies a customer master key \(CMK\) used to encrypt the destination object.

To use a CMK to encrypt objects, you must create the CMK in the same region as the destination bucket. For more information, see [Manage CMKs]().

    -   **Authorized RAM Role**: authorizes a RAM role to perform KMS-based encryption on the destination objects.
        -   **New RAM Role**: A RAM role is created to perform KMS-based encryption on the destination object. The role name is in the following format: `kms-replication-source bucket name-destination bucket name`.
        -   **AliyunOSSRole**: The AliyunOSSRole role is used to perform KMS-based encryption on the destination object. If the AliyunOSSRole role does not exist, OSS automatically creates the AliyunOSSRole role when you select this option.
**Note:**

    -   You can use [HeadObject](/intl.en-US/API Reference/Object operations/Basic operations/HeadObject.md) to query the encryption status of the source object and [GetBucketEncryption](/intl.en-US/API Reference/Bucket operations/Encryption/GetBucketEncryption.md) to query the encryption status of the destination bucket.
    -   For more information about how to configure CRR for buckets for that have server-side encryption configured, see [Cross-region replication in specific scenarios](/intl.en-US/Developer Guide/Data security/Disaster recovery/Cross-region replication in specific scenarios.md). |

5.  Click **OK**.

    -   A CRR rule cannot be edited or deleted after it is created.
    -   The synchronization task starts 3 to 5 minutes after a CRR rule is configured. You can view the synchronization progress by choosing **Redundancy for Fault Tolerance** \> **Cross-Region Replication** on the overview page of the source bucket.
    -   In CRR, data is asynchronously \(near real-time\) replicated. It takes several minutes to several hours for the data to be replicated to the destination bucket based on the amount of data.

## Disable CRR

You can click **Disable** to disable CRR.

![sync](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4658906061/p135995.png)

After CRR is disabled, the replicated data is stored in the destination bucket, and the incremental data in the source bucket is not replicated to the destination bucket.

