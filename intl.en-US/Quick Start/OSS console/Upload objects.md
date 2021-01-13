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
    |**File ACL**|Select the ACL of the objects that you want to upload.     -   **Inherited from Bucket**: The ACL of each object is the same as that of the bucket. This is the default ACL of uploaded objects.
    -   **Private**: Only the owner or authorized users can read and write the uploaded objects. Other users, including anonymous users cannot access the objects without authorization.
    -   **Public Read**: Only the bucket owner can perform write operations on the uploaded objects. Other users, including anonymous users, can perform only read operations on the objects.
    -   **Public Read/Write**: All users, including anonymous users, can read and write the uploaded objects.
For more information, see [Object ACL](/intl.en-US/Developer Guide/Data security/Access and control/ACL.md). |
    |**Upload**|Drag and drop one or more objects to upload to this section, or click **Upload** to select one or more objects to upload. **Note:**

    -   If the object to upload has the same name as an existing object in the bucket, the existing object is overwritten.
    -   If you drag a folder to the Upload section to upload it, the folder and the files and subfolders in it are all uploaded to the bucket.
    -   Do not refresh or close the upload page when objects are being uploaded. Otherwise, the upload tasks are interrupted and the list of upload tasks is cleared. |

5.  Wait until the objects are uploaded. Then, close the Upload Tasks.

    After the objects are uploaded, you can view the name, size, and the storage classes of the uploaded objects in the specified folder.


## Other implementation methods

|Operation|Implementation method|
|---------|---------------------|
|Upload objects|[ossutil](/intl.en-US/Tools/ossutil/Common commands/cp/Upload objects.md)|
|[ossbrowser](/intl.en-US/Tools/ossbrowser/Quick start.md)|
|[API operations](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.md)|
|[Java SDK](/intl.en-US/SDK Reference/Java/Upload objects/Overview.md)|
|[Python SDK](/intl.en-US/SDK Reference/Python/Upload objects/Overview.md)|
|[Go SDK](/intl.en-US/SDK Reference/Go/Upload objects/Overview.md)|
|[C++ SDK](/intl.en-US/SDK Reference/C++/Upload objects/Overview.md)|
|[C SDK](/intl.en-US/SDK Reference/C/Upload objects/Overview.md)|
|[PHP SDK](/intl.en-US/SDK Reference/PHP/Upload objects/Overview .md)|
|[Node.js SDK](/intl.en-US/SDK Reference/Node. js/Upload objects/Overview.md)|
|[Browser.js](/intl.en-US/SDK Reference/Browser.js/Upload objects.md)|
|[Android SDK](/intl.en-US/SDK Reference/Android/Upload objects/Overview.md)|
|[iOS SDK](/intl.en-US/SDK Reference/iOS/Upload objects/Overview.md)|

-   You can download uploaded objects to the default download path of the browser or a specified local path. For more information, see [t4335.md\#](/intl.en-US/Quick Start/OSS console/Download objects.md).
-   You can share the URLs of uploaded objects with third parties for download or preview. For more information, see [t2009049.md\#]().
-   After objects are uploaded to OSS, you can use upload callback to submit a callback request to the specified application server. For more information, see [Upload callback](/intl.en-US/Developer Guide/Objects/Upload files/Upload callback.md).

