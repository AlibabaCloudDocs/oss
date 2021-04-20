# Configure lifecycle rules to manage object versions

When versioning is enabled for a bucket, data that is overwritten or deleted in the bucket is saved as a previous version. You can configure lifecycle rules to delete expired delete markers and previous versions that you no longer use to reduce storage costs and improve bucket performance.

## Prerequisites

Versioning is enabled for the bucket. For more information, see [Enable versioning](/intl.en-US/Console User Guide/Manage buckets/Redundancy for fault tolerance/Configure versioning.md).

## Scenarios

A user uploaded an object named example.txt to a versioned bucket named examplebucket on February 8, 2020. Within the same year, the user overwrote example.txt and deleted the object without specifying a version ID multiple times. Each time when the object is overwritten or deleted, OSS saves the current version of the object as a previous version and specifies a random string as the globally unique ID. In the following figure, simple strings but not actual version IDs are used for ease of viewing.

![versioning](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1919098161/p243905.jpg)

To meet business requirements, the user wants to manage the versions of example.txt to achieve the following goals:

-   Retain only the versions that are generated on May 8, 2020 and September 10, 2020.
-   Recover the latest previous version generated on May 8, 2020 to the current version.

## Usage notes

When you configure lifecycle rules to manage object versions, take note of the following items:

-   Expiration policy for current versions
    -   In a versioned bucket, if the expiration policy specified in a lifecycle rule is implemented on the current version of an object, OSS adds a delete marker to the object and stores the current version as a previous version. The delete marker becomes the current version of the object.
    -   In a versioning-suspended bucket, if the expiration policy specified in a lifecycle rule is implemented on the current version of an object, OSS adds a delete marker whose ID is null to the object as the new current version. If the object has an existing version whose ID is null, the version is overwritten by the delete marker because version IDs are globally unique.
-   Expiration policy for previous versions

    In a bucket for which versioning is enabled or suspended, if the expiration policy specified in a lifecycle is implemented on a previous version of an object, the previous version is permanently deleted and cannot be recovered.


For more information about lifecycle rules, see [Lifecycle rules](/intl.en-US/Developer Guide/Objects/Object lifecycle/Lifecycle rules.md).

## Procedures

1.  Retain specified object versions.

    To configure lifecycle rules to retain only versions that are generated for example.txt on May 8, 2020 and September 10, 2020, perform the following steps:

    1.  Log on to the [OSS console](https://partners-intl.console.aliyun.com/#/oss).
    2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click exmaplebucket.
    3.  In the left-side navigation pane, choose **Basic Settings** \> **Lifecycle**. In the **Lifecycle** section, click **Configure**.
    4.  On the page that appears, click **Create Rule**. In the Create Rule panel, configure the parameters described in the following table.

        ![lifecycle](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1919098161/p253608.jpg)

        |Category|Parameter|Configuration methods|
        |--------|---------|---------------------|
        |Basic Settings|**Status**|Select **Enabled**.|
        |**Applied To**|Select **Whole Bucket**.|
        |Current Version|**File Lifecycle**|Select **Clean Up Delete Marker**.|
        |Previous Versions|**File Lifecycle**|Select **Validity Period \(Days\)**.|
        |**Delete**|Set the value to 90. An object expires 90 days after it is stored as a previous version and is deleted the day after it expires.|
        |Delete Parts|**Part Lifecycle**|Select **Validity Period \(Days\)**.|
        |**Delete**|Set the value to 90. Parts that are generated in [multipart upload](/intl.en-US/Developer Guide/Objects/Upload files/Multipart upload and resumable upload.md) tasks expire 90 days after they are generated and are deleted the day after they expire.|

    5.  Click **OK**.
2.  Recover a specified object version

    To recover the latest previous version that is generated for example.txt on May 8, 2020 to the current version, perform the following steps:

    1.  On the Overview page of examplebucket, click **Files**.
    2.  Find the previous version of example.txt that you want to recover.
    3.  Click **Restore** in the Actions column that corresponds to the previous version that you want to recover.

