# Rename directories or objects

After you enable the hierarchical namespace feature for a bucket, you can rename or move objects and directories within the bucket.

## Background information

When you rename or move an object or a directory in a bucket for which hierarchical namespace is enabled, take note of the following items:

-   The [Rename](/intl.en-US/API Reference/Object operations/Directory management/Rename.md) operation is called to rename objects or directories.
-   You can rename objects of all sizes in the OSS console. However, you can move only objects up to 1 GB in size in the OSS console.

For more information about the hierarchical namespace feature, see [Hierarchical namespace](/intl.en-US/Developer Guide/Buckets/Hierarchical namespace.md).

## Procedure

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket in which the object or directory you want to rename or move is stored.

3.  In the left-side navigation pane, click **Files**, and then rename the object or directory. The following table describes the operations that you can perform to rename or move an object or a directory.

    |Scenario|Operation|
    |--------|---------|
    |Rename an object|Move the pointer over the name of the object that you want to rename, and then click the ![edit](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9779712261/p277398.png) icon to rename the object. You must include a suffix in the new object name.|
    |Move an object|In the **Actions** column that corresponds to the object that you want to move, choose **More** \> **Move File**. In the Move File panel, specify the destination directory to which you want to move the object.     -   To move the object to the root directory of the current bucket, leave the destination directory empty.
    -   To move the object to a specific directory in the current bucket, specify the directory as the destination directory. For example, to move the object to a subdirectory named subdir within a directory named destdir, set the destination directory to destdir/subdir. |
    |Rename a directory|Move the pointer over the name of the directory that you want to rename, and then click the ![edit](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9779712261/p277398.png) icon to rename the directory. The directory name cannot start with a forward slash \(/\).|
    |Move a directory|Operations performed to move a directory are similar to those used to rename a directory. However, you must add a forward slash \(/\) to the new name of the directory. The following examples show how to move a directory:     -   A subdirectory named subdir is located within a directory named destdir. To move subdir to another directory named destfolder, set the new directory name to /destfolder/subdir.
    -   A subdirectory named subdir is located within a directory named destdir. To move subdir to the root directory of the current bucket, set the new directory name to /subdir. |


