# Cross-region replication

Cross-region replication \(CRR\) provides automatic and asynchronous \(near real-time\) replication of objects across buckets in different OSS regions. Operations such as the creation, overwriting, and deletion of objects can be synchronized from a source bucket to a destination bucket.

## Implementation methods

You can configure CRR in the OSS console or by using OSS SDK for Java:

-   [Console](/intl.en-US/Console User Guide/Manage buckets/Redundancy for fault tolerance/Configure CRR.md)
-   [OSS SDK for Java](/intl.en-US/SDK Reference/Java/Buckets/Cross-region replication.md)

## Scenarios

CRR can meet your requirements on geo-disaster recovery and data replication. Objects in the destination bucket are exact replicas of those in the source bucket. They have the same object names, versioning information, object content, and object metadata such as the creation time, owner, user metadata, and object ACLs. You can configure CRR rules in the following scenarios to meet your requirements:

-   Compliance requirements

    OSS stores multiple replicas of each object in physical disks. However, replicas of data must be stored at a distance from each other to meet compliance requirements. CRR allows you to replicate data between geographically distant data centers to meet compliance requirements.

-   Minimum latency

    You have customers located in two geographical locations. To minimize the latency when the customers access objects, you can maintain the replicas of objects in data centers that are geographically closer to the customers.

-   Data backup and disaster recovery

    You have high requirements on data security and availability, and want to explicitly maintain the replicas of all data written to a data center in another data center. If one data center is damaged in a catastrophic event such as an earthquake or a tsunami, you can use data that is backed up in another data center.

-   Data replication

    For business reasons, you may need to migrate data from one OSS data center to another data center.

-   Operational reasons

    You have compute clusters that are deployed in two data centers to analyze the same group of objects. You can maintain the replicas of the objects in the two regions.


## Capabilities

CRR supports the following capabilities:

-   Data synchronization between buckets in different regions

    You can configure CRR rules to synchronize data from a source bucket to multiple destination buckets. You can configure up to 100 CRR rules for a bucket. A bucket can be configured as the source bucket or destination bucket.

    ![1](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2599738161/p248978.jpg)

    If your business requires more than 100 CRR rules for a bucket, contact the [technical support](https://workorder-intl.console.aliyun.com/#/ticket/createIndex).

-   Real-time data synchronization

    This capability monitors data addition, deletion, and modification in real time and synchronizes these changes to the destination bucket. Operations performed on objects that are smaller than 2 MB in size are synchronized within minutes to ensure data consistency between the source and the destination buckets.

-   Historical data migration

    This capability synchronizes historical data from the source bucket to the destination bucket. This way, two identical data replicas are individually stored in the source bucket and destination bucket.

-   Real-time display of the synchronization progress

    This capability displays the last synchronization time for real-time data synchronization and the percentage of synchronization for historical data migration.

-   Versioning

    CRR ensures the consistency between the data in the source and destination buckets for which versioning is enabled. If you configure a CRR rule to synchronize only added and modified data, delete operations performed on the specified version of an object in the source bucket are not synchronized to the destination bucket. Delete markers created in the source bucket are synchronized to the destination bucket.

-   Transfer acceleration

    You can use transfer acceleration to speed up data transmission when CRR tasks are performed across regions within mainland China and outside mainland China. For more information about transfer acceleration, see [Transfer acceleration](/intl.en-US/Developer Guide/Buckets/Transfer acceleration.md).

-   Replication of encrypted data

    CRR allows you to replicate unencrypted objects and objects that are encrypted by using SSE-KMS or SSE-OSS at the server side. For more information, see [Cross-region replication in specific scenarios](/intl.en-US/Developer Guide/Data security/Disaster recovery/Cross-region replication in specific scenarios.md).


## Usage notes

When you use CRR, take note of the following items:

-   Supported regions
    -   You must enable transfer acceleration when you perform CRR between regions within mainland China and regions outside mainland China.
    -   CRR rules based on object tags can be configured only in the following scenarios:
        -   The source region is China \(Hangzhou\) and the destination region is a region except for China \(Hangzhou\).
        -   The source region is Australia \(Sydney\) and the destination region can be a different region outside mainland China.
-   Billing
    -   After you configure a CRR rule between two buckets, you are charged for the traffic generated to replicate objects from the source bucket to the destination. For more information, see [Traffic fees](/intl.en-US/Pricing/Billing items and methods/Traffic fees.md).
    -   Each time an object is synchronized, OSS counts the number of requests and charges you on a pay-as-you-go basis. For more information, see [API operation calling fees](/intl.en-US/Pricing/Billing items and methods/API operation calling fees.md).
    -   If you enable transfer acceleration, you are charged for this feature. For more information, see [Transfer acceleration fees](/intl.en-US/Pricing/Billing items and methods/Transfer acceleration fees.md).
-   Replication time

    In CRR, data is asynchronous replicated in near real-time. Based on the size of the data, it takes several minutes to several hours to copy data from the source bucket to the destination bucket.

-   Limits
    -   You can configure CRR between two unversioned buckets or versioned buckets.
    -   The versioning status of two buckets between which a CRR rule is configured cannot be changed.
    -   You can manage two buckets between which a CRR rule is configured at the same time. Therefore, the object replicated from the source bucket may overwrite the object that has the same name in the destination bucket.

