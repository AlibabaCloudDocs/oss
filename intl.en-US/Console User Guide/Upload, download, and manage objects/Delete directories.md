# Delete directories

You can delete directories from a bucket by using the OSS console. When you delete a directory, the objects inside are also deleted.

## Usage notes

-   Before you delete a directory, make sure that the required objects inside are moved to other paths.
-   When a directory has a large number of objects, it takes a long time for OSS to delete the directory. We recommend that you configure lifecycle rules for the bucket to delete the objects. For more information, see [Configure lifecycle rules](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure lifecycle rules.md).

## Procedures

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket in which the directory you want to delete is located.

3.  In the left-side navigation pane, click **Files**. Delete the directory in different methods based on the status of the bucket.

    **Note:** Do not refresh or close the Task List panel when you delete directories. Otherwise, the tasks are interrupted.

    -   Delete the directory from a bucket for which the hierarchical namespace feature is enabled

        Click **Completely Delete** in the Actions column corresponding to the directory that you want to delete. In the message that appears, click **OK**.

        The directory and the objects inside are permanently deleted.

    -   Delete the directory from a bucket for which the hierarchical namespace feature is disabled

        If the hierarchical namespace and versioning features are disabled for a bucket, the directory is deleted in the same way as it is deleted when the hierarchical namespace feature is enabled.

        If the hierarchical namespace feature is disabled and versioning is enabled for the bucket, you can delete the directory by using the following methods:

        -   Store the deleted directory as a previous version
            1.  In the upper-right corner of the object list, set **Display Previous Versions** to **Hide**.
            2.  Click **Delete** in the Actions column corresponding to the directory that you want to delete. In the massage that appears, click **OK**.

                The deleted directory and the objects inside are stored as previous versions that can be recovered. For more information about how to recover previous versions, see [Recover previous versions](/intl.en-US/Console User Guide/Manage buckets/Redundancy for fault tolerance/Configure versioning.md).

        -   Permanently delete the directory
            1.  In the upper-right corner of the object list, set **Display Previous Versions** to **Show**.
            2.  Click **Completely Delete** in the Actions column corresponding to the directory that you want to delete. In the massage that appears, click **OK**.

                The directory and the objects inside are permanently deleted.

4.  In the Task List panel, you can view the deletion progress.

    When OSS performs deletion tasks, you can perform the following operations:

    -   Click **Removed** to remove the completed tasks from Task List.
    -   Click **Pause All** to pause all the running tasks. When tasks are paused, you can perform the following operations:
        -   Click **Start** to resume a task.
        -   Click **Remove** to remove a task. If you remove a task, the objects that are not deleted in the task are retained.
    -   Click **Start All** to resume all paused tasks.

