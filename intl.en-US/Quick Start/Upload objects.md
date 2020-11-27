# Upload objects

After a bucket is created, you can upload any type of objects to it.

A bucket is created. For more information, see [Create buckets](/intl.en-US/Quick Start/Create buckets.md).

## Use the OSS console

To upload an object to a bucket in the OSS console, perform the following steps:

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  Choose **Files** \> **Upload**.

4.  In the Upload dialog box that appears, set the following parameters.

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
    -   If you upload a folder, only the files in the folder are uploaded and stored in the same folder in the bucket.
    -   Do not refresh or close the upload page when objects are being uploaded. Otherwise, the upload tasks are interrupted and the upload object list is cleared. |

5.  After the upload is complete, close the Upload Tasks dialog box.


## Use ossbrowser

ossbrowser is a graphical object management tool for OSS. You can use ossbrowser to upload an object to an OSS bucket. For more information, see [Quick start](/intl.en-US/Tools/ossbrowser/Quick start.md).

## Use ossutil

ossutil is a command line tool for OSS. You can use ossutil to upload an object to OSS. For more information, see [Upload objects](/intl.en-US/Tools/ossutil/Common commands/cp/Overview.md).

## Use APIs and SDKs

OSS provides APIs and SDK packages in multiple programming languages to facilitate secondary development. For more information, see the following topics:

-   API operations:
    -   Simple upload: [PutObject](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.md)
    -   Append upload: [AppendObject](/intl.en-US/API Reference/Object operations/Basic operations/AppendObject.md)
    -   Multipart upload: [UploadPart](/intl.en-US/API Reference/Object operations/Multipart upload/UploadPart.md)
-   SDK reference
    -   OSS SDK for Java: [Upload objects](/intl.en-US/SDK Reference/Java/Upload objects/Overview.md)
    -   OSS SDK for Python: [Upload objects](/intl.en-US/SDK Reference/Python/Upload objects/Overview.md)
    -   OSS SDK for Go: [Upload objects](/intl.en-US/SDK Reference/Go/Upload objects/Overview.md)
    -   OSS SDK for C++: [Upload objects](/intl.en-US/SDK Reference/C++/Upload objects/Overview.md)
    -   OSS SDK for C: [Upload objects](/intl.en-US/SDK Reference/C/Upload objects/Overview.md)
    -   OSS SDK for PHP: [Upload objects](/intl.en-US/SDK Reference/PHP/Upload objects/Overview .md)
    -   OSS SDK for Node.js: [Upload objects](/intl.en-US/SDK Reference/Node. js/Upload objects/Overview.md)
    -   OSS SDK for Browser.js: [Upload objects](/intl.en-US/SDK Reference/Browser.js/Upload objects.md)
    -   OSS SDK for Android: [Upload objects](/intl.en-US/SDK Reference/Android/Upload objects/Overview.md)
    -   OSS SDK for iOS: [Upload objects](/intl.en-US/SDK Reference/iOS/Upload objects/Overview.md)

## Usage notes

-   Objects larger than 5 GB cannot be uploaded by using the following methods: console upload, [simple upload](/intl.en-US/Developer Guide/Objects/Upload files/Simple upload.md), [form upload](/intl.en-US/Developer Guide/Objects/Upload files/Form upload.md), [append upload](/intl.en-US/Developer Guide/Objects/Upload files/Append upload.md). Objects larger than 48.8 TB cannot be uploaded by using [multipart upload](/intl.en-US/Developer Guide/Objects/Upload files/Multipart upload and resumable upload.md). For more information, see [Limits](/intl.en-US/Product Introduction/Limits.md).
-   If you upload a large number of objects by using sequential prefixes such as timestamps and letters in the object names, multiple object indexes may be stored in a single partition. If too many requests are sent to query these objects, the responsiveness may be slow. We recommend that you do not upload a large number of objects by using sequential prefixes. For more information about how to change sequential prefixes to random prefixes, see [OSS performance and scalability best practices](/intl.en-US/Best Practices/OSS performance and scalability best practices.md).

-   Images can be processed after they are uploaded. For more information, see [Image processing](/intl.en-US/Developer Guide/Data Processing/Image Processing/IMG implementation modes.md).
-   Audio or video objects can be processed after they are uploaded. For more information, see [ApsaraVideo for Media Processing](/intl.en-US/Developer Guide/Cloud data processing.md).
-   After objects are uploaded to OSS, you can use upload callback to submit a callback request to the specified application server and perform subsequent operations. For more information, see [Upload callback](/intl.en-US/Developer Guide/Objects/Upload files/Upload callback.md).
-   You can also download objects that have been uploaded to OSS. For more information, see [Download objects](/intl.en-US/Quick Start/Download objects.md).

