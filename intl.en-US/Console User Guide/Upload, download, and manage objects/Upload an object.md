# Upload an object

After a bucket is created, you can upload any type of objects to it.

A bucket is created. For more information, see [Create a bucket](/intl.en-US/Quick Start/Create buckets.md).

When you upload an object in the OSS console, the object is uploaded by using [Form upload](/intl.en-US/Developer Guide/Objects/Upload files/Form upload.md) and by calling [PostObject](/intl.en-US/API Reference/Object operations/Basic operations/PostObject.md). In this case, the size of the object to be uploaded cannot exceed 5 GB.

In Alibaba Finance Cloud, you cannot upload objects by using the console because OSS does not have a region connected to the Internet. You must use OSS SDKs, ossutil, or ossbrowser to upload objects.

**Note:** For objects larger than 5 GB, we recommend that you use the following methods to upload the objects:

-   Multipart upload by using SDKs or APIs: For more information, see [Multipart upload and resumable upload](/intl.en-US/Developer Guide/Objects/Upload files/Multipart upload and resumable upload.md).
-   ossbrowser: For more information, see [Quick start](/intl.en-US/Tools/ossbrowser/Quick start.md).
-   ossutil: For more information, see [Overview](/intl.en-US/Tools/ossutil/Overview.md).

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket to which you want to upload objects.

3.  In the left-side navigation, click **Files**. On the Files page, click **Upload**.

4.  In the Upload panel, set the following parameters.

    |Parameter|Description|
    |---------|-----------|
    |**Upload To**|Set the folder to which objects are uploaded.     -   **Current**: Objects are uploaded to the current folder.
    -   **Specified**: Objects are uploaded to the specified folder. You must enter a folder name. If the entered folder does not exist, OSS automatically creates the corresponding folder and uploads the object to the folder. |
    |**File ACL**|Set the ACL of the object to upload. Default value: **Inherited from Bucket**.     -   **Inherited from Bucket**: The ACL of each object is the same as that of the bucket.
    -   **Private**: Only the owner or authorized users can read and write objects in the bucket. Other users, including anonymous users cannot access the objects in the bucket without authorization.
    -   **Public Read**: Only the bucket owner can perform write operations on objects in the bucket. Other users, including anonymous users, can perform only read operations on objects in the bucket.
    -   **Public Read/Write**: All users, including anonymous users, can read and write objects in the bucket.
For more information about object ACL, see [Object ACL](/intl.en-US/Developer Guide/Data security/Access and control/ACL.md). |
    |**Upload**|Drag and drop one or more objects to upload to this section, or click **Upload** to select one or more objects to upload. **Note:**

    -   When the uploaded object has the same name as an existing object in the bucket, the existing object is overwritten.
    -   If you drag a folder to the Upload section to upload it, the folder and the files and subfolders in it are all uploaded to the bucket.
    -   Do not refresh or close the upload page when objects are being uploaded. Otherwise, the upload tasks are interrupted and the upload object list is cleared. |

5.  After the upload is complete, close the Upload Tasks panel.


