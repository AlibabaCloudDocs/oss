# Same-region replication

Same-region replication \(SRR\) allows you to replicate objects across buckets within the same region in an automatic and asynchronous \(near real-time\) manner. Operations such as the creation, overwriting, and deletion of objects are synchronized from a source bucket to destination buckets.

## Scenarios

If your data cannot be transferred out of your country or region based on the compliance requirements of local laws and regulations, you can configure SRR rules to store the replicas of data in the source bucket in multiple destination buckets within the same region. Objects in the destination bucket are exact replicas of those in the source bucket. They have the same object names, versioning information, object content, and object metadata such as the creation time, owner, user metadata, and object access control lists \(ACLs\).

## Configuration method

You can enable SRR for a bucket in the OSS console. For more information, see [Configure SRR]().

## Capabilities

SRR supports the following capabilities:

-   Data synchronization between buckets in the same region

    You can configure SRR rules to synchronize data from a source bucket to multiple destination buckets in the same region. You can configure up to 100 SRR rules for a bucket. A bucket can be configured as the source bucket or destination bucket.

    ![1](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2599738161/p248978.jpg)

    If your business requires more than 100 SRR rules for a bucket,contact the [technical support](https://workorder-intl.console.aliyun.com/#/ticket/createIndex).

-   Real-time data synchronization

    This capability monitors data addition, deletion, and modification in real time and synchronizes these changes to the destination bucket. Operations performed on objects that are smaller than 2 MB in size are synchronized within minutes to ensure data consistency between the source and the destination buckets.

-   Historical data migration

    This capability synchronizes historical data from the source bucket to the destination bucket. This way, two identical data replicas are individually stored in the source bucket and destination bucket.

-   Real-time display of the synchronization progress

    This capability displays the last synchronization time for real-time data synchronization and the percentage of synchronization for historical data migration.

-   Versioning

    SRR ensures the consistency between the data in the source and destination buckets for which versioning is enabled. If you configure an SRR rule to synchronize only added and modified data, delete operations performed on the specified version of an object in the source bucket are not synchronized to the destination bucket. Delete markers created in the source bucket are synchronized to the destination bucket.


## Usage notes

-   Billing
    -   OSS charges you for the traffic generated when you use SRR to replicate objects. For more information about the billing methods, see [Traffic fees](/intl.en-US/Pricing/Billing items and methods/Traffic fees.md).
    -   Each time an object is synchronized, OSS counts the number of requests and charges you on a pay-as-you-go basis. For more information, see [API operation calling fees](/intl.en-US/Pricing/Billing items and methods/API operation calling fees.md).
-   Replication time

    In SRR, data is asynchronously replicated in near real-time. Based on the size of the data, it takes several minutes to several hours to copy data from the source bucket to the destination bucket.

-   Limits
    -   You can configure SRR only between two unversioned buckets or versioned buckets.
    -   The versioning status of two buckets between which an SRR rule is configured cannot be changed.
    -   You can manage two buckets between which an SRR rule is configured at the same time. Therefore, the object replicated from the source bucket may overwrite the object that has the same name in the destination bucket.

