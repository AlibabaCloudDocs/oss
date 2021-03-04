# Upload objects

You can upload objects up to 5 GB in size in the OSS console.

A bucket is created. For more information, see [Create buckets](/intl.en-US/Quick Start/OSS console/Create buckets.md).

## Procedure

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket to which you want to upload objects.

3.  In the left-side navigation pane, click **Files**. Then, click **Upload**.

4.  In the Upload panel, configure the parameters described in the following table.

    |Parameter|Description|
    |---------|-----------|
    |**Upload To**|Set the folder to which you want to upload objects.     -   **Current**: Objects are uploaded to the current folder.
    -   **Specified**: Objects are uploaded to the specified folder. You must enter a folder name. If the entered folder does not exist, OSS automatically creates the folder and uploads the objects to the folder. |
    |**File ACL**|Select the ACL of the objects that you want to upload.     -   **Inherited from Bucket**: The ACL of the uploaded objects are the same as that of the bucket.
    -   **Private**: Only the object owner or authorized users can read and write the uploaded objects. Other users, including anonymous users cannot access the uploaded objects without authorization. We recommend that you set the ACL to this value.
    -   **Public Read**: Only the object owner or authorized users can read and write the uploaded objects. Other users, including anonymous users can only read the uploaded objects. If you set the ACL to this value, data leakage and additional fees may occur. Exercise caution when you set the ACL to Public Read.
    -   **Public Read/Write**: Any users, including anonymous users can read and write the uploaded objects. If you set the ACL to this value, data leakage and additional fees may occur. If a user writes illegal information to your objects, your legitimate interests and rights may be damaged. Therefore, we recommend that you do not set the ACL to Public Read/Write except in special cases.
For more information, see [Object ACL](/intl.en-US/Developer Guide/Data security/Access and control/ACL.md). |
    |**Files to Upload**|Select the files or folders that you want to upload.You can click **Select Files** or **Select Folders** to select on-premises files or folders. You can also drag the files or folders that you want to upload to this section.

If the selected folder includes files that you do not want to upload, click **Remove** in the Actions column corresponding to these files to remove them from the file list.

**Note:**

    -   If the object to upload has the same name as an existing object in the bucket, the existing object is overwritten.
    -   If you drag a folder to the Files to Upload section to upload it, the folder and the files and subfolders in this folder are all uploaded to the bucket.
    -   Do not refresh or close the upload page when objects are being uploaded. Otherwise, the upload tasks are interrupted and the list of upload tasks is cleared. |

5.  Click **Upload**.

    You can check the upload progress of each object on the Upload Tasks tab in the Task List panel. After the objects are uploaded, you can view the names, sizes, and the storage classes of the uploaded objects in the specified folder.


## Other implementation methods

|Operation|Implementation method|
|---------|---------------------|
|Upload objects|[ossutil](/intl.en-US/Tools/ossutil/Common commands/cp/Upload objects.md)|
|[ossbrowser](/intl.en-US/Tools/ossbrowser/Quick start.md)|
|[API operations](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.md)|
|[OSS SDK for Java](/intl.en-US/SDK Reference/Java/Upload objects/Overview.md)|
|[OSS SDK for Python](/intl.en-US/SDK Reference/Python/Upload objects/Overview.md)|
|[OSS SDK for Go](/intl.en-US/SDK Reference/Go/Upload objects/Overview.md)|
|[OSS SDK for C++](/intl.en-US/SDK Reference/C++/Upload objects/Overview.md)|
|[OSS SDK for C](/intl.en-US/SDK Reference/C/Upload objects/Overview.md)|
|[OSS SDK for PHP](/intl.en-US/SDK Reference/PHP/Upload objects/Overview .md)|
|[OSS SDK for Node.js](/intl.en-US/SDK Reference/Node. js/Upload objects/Overview.md)|
|[OSS SDK for Browser.js](/intl.en-US/SDK Reference/Browser.js/Upload objects.md)|
|[OSS SDK for Android](/intl.en-US/SDK Reference/Android/Upload objects/Overview.md)|
|[OSS SDK for iOS](/intl.en-US/SDK Reference/iOS/Upload objects/Overview.md)|

-   You can download uploaded objects to the default download path of your browser or a specified on-premises path. For more information, see [t4335.md\#](/intl.en-US/Quick Start/OSS console/Download objects.md).
-   You can share the URLs of uploaded objects with third parties for downloads or previews. For more information, see [t2009049.md\#](/intl.en-US/Quick Start/OSS console/Share objects.md).
-   After objects are uploaded to OSS, you can use upload callback to submit a callback request to the specified application server. For more information, see [Upload callback](/intl.en-US/Developer Guide/Objects/Upload files/Upload callback.md).

