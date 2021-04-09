# Rename a directory or object

You can rename objects in a bucket. If the hierarchical namespace feature is enabled for a bucket when the bucket is created, you can also rename directories in the bucket.

## Background information

If the hierarchical namespace feature is not enabled for a bucket, you can rename only objects in the buckets. When you rename an object, take note of the following items:

-   The [CopyObject](/intl.en-US/API Reference/Object operations/Basic operations/CopyObject.md) operation is called to rename objects.
-   You can rename only objects that are up to 1 GB in size in the OSS console. To rename objects that are larger than 1 GB, we recommend that you use [ossutil](/intl.en-US/Tools/ossutil/Common commands/cp/Copy objects.md).
-   In a bucket for which [versioning](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md) is enabled, you can rename an object only when you set **Display Previous Versions** to **Hide**. After you rename an object, a delete marker is created for the original object.
-   If the object that you rename is an IA, Archive, or Cold Archive object and is stored for less than the specified minimum storage duration, you are charged for the entire minimum storage duration. For more information, see [Storage fees](/intl.en-US/Pricing/Billing items and methods/Storage fees.md).

If the hierarchical namespace feature is enabled for a bucket, you can also rename directories in the bucket. When you rename objects or directories in a bucket for which the hierarchical namespace feature is enabled, take note of the following items:

-   The [Rename](/intl.en-US/API Reference/Object operations/Directory management/Rename.md) operation is called to rename objects or directories.
-   The size of the object you want to rename is not limited in the OSS console.

For more information about the hierarchical namespace feature, see [Hierarchical namespace](/intl.en-US/Developer Guide/Objects/Hierarchical namespace.md).

## Procedures

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket in which the object or directory you want to rename is stored.

3.  In the left-side navigation pane, click **Files**.

4.  Move the pointer over the objector directory you want to rename and click the ![Rename](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8420893161/p182159.png) icon to the right of the objector directory name.

5.  In the Rename dialog box, enter the new objector directory name and click **OK**.

    The objector directory name must conform to the following conventions:

    -   The name can contain only UTF-8 characters.
    -   The name must be 1 to 254 characters in length.
    -   The name cannot include forward slashes \(/\) or backslashes \(\\\).

