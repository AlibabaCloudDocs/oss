# Delete folders

You can delete folders from a bucket by using the OSS console. When you delete a folder, the folder is deleted along with its objects.

## Limits

-   To prevent objects from being accidentally deleted, make sure that required objects in a folder are backed up before you delete the folder.
-   If a folder contains a large number of objects, it takes a lot of time to delete objects. We recommend that you configure lifecycle rules to delete the folder. For more information, see [Configure lifecycle rules](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure lifecycle rules.md).

## Procedure

To delete a folder, perform the following steps:

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  In the left-side navigation pane, click **Files**. Delete the specified folder.

    -   Buckets in the unversioned state

        Click **Completely Delete** in the Actions column corresponding to the folder that you want to delete. In the message that appears, click **OK**.

        The folder is completely deleted.

    -   Buckets in the versioned state

        -   Convert a folder to previous versions
            1.  In the upper-right corner, set **Display Previous Versions** to **Hide**.
            2.  Click **Completely Delete** in the Actions column corresponding to the folder that you want to delete. In the massage that appears, click **OK**.

                The folder becomes a previous version. You can recover the folder. For more information about how to recover previous versions of a folder, see [Recover previous versions](/intl.en-US/Console User Guide/Manage buckets/Redundancy for fault tolerance/Versioning.md).

        -   Completely delete a folder
            1.  In the upper-right corner, set **Display Previous Versions** to **Show**.
            2.  Click **Completely Delete** in the Actions column corresponding to the folder that you want to delete. In the massage that appears, click **OK**.

                The folder is completely deleted.

        For more information about versioning, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md).

4.  In the Task List panel, you can view the deletion progress.

    When OSS performs deletion tasks, you can perform the following operations:

    -   Click **Removed** to remove the completed tasks from Task List.
    -   Click **Pause All** to pause the running tasks. When tasks are paused, you can perform the following operations:
        -   Click **Start** to restart the task.
        -   Click **Remove** to remove the task. After tasks are removed, objects that are not deleted are retained.
    -   Click **Start All** to start all paused tasks.
    **Note:** Do not refresh or close the Task List pane when you delete folders. Otherwise, tasks are interrupted.


