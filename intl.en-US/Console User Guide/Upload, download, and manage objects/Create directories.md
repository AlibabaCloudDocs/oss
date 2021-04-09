# Create directories

If you want to organize objects that you upload to a bucket, you can create directories. This topic describes how to create directories or folders in the OSS console.

A bucket is created. For more information, see [Create buckets](/intl.en-US/Quick Start/OSS console/Create buckets.md).

When the hierarchical namespace feature is disabled for a bucket,OSS uses a flat structure instead of a hierarchical structure for objects. All elements are stored as objects in buckets. To help organize objects and simplify management, the OSS console displays objects whose names end with a forward slash \(/\) as directories These objects can be uploaded and downloaded. You can use directories in the OSS console in the same manner as you use directories in Windows.

When the hierarchical namespace feature is enabled for a bucket, you can update a parent directory to perform multiple directory-level tasks at the same time. Traditionally, OSS uses a flat namespace to store objects in buckets and uses objects whose names end with a forward slash \(/\) to simulate directories. Compared with this method, the hierarchical namespace feature greatly improves the performance of operations that are related to directories. For more information about the hierarchical namespace feature, see [Hierarchical namespace](/intl.en-US/Developer Guide/Buckets/Hierarchical namespace.md).

## Procedures

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket in which you want to create the folder.

3.  In the left-side navigation pane, click **Files**.

4.  On the Files page, click **Create Folder**.

5.  In the Create Folder panel, enter the **Folder Name**.

6.  Click **OK**.


