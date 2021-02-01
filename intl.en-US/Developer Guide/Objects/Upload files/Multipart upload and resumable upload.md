# Multipart upload and resumable upload

Alibaba Cloud OSS provides multipart upload for you to split an object you want to upload into multiple parts and upload the parts separately. After the parts are uploaded, you can call CompleteMultipartUpload to combine these parts into an object. If the upload fails due to network errors in the upload process, you can continue the upload from the last uploaded part to implement resumable upload.

## Scenarios

-   Accelerated upload of large objects

    When the size of an object exceeds 5 GB, use multipart upload to concurrently upload multiple parts. This way, the upload is accelerated.

-   Poor network environments

    When the network environment is poor, we recommend that you use multipart upload. When the upload fails, you need only to upload the parts that failed to be uploaded.

-   Stream upload

    You can use stream upload to upload objects whose sizes are unknown. This scenario is common in industry applications such as video surveillance.


## Multipart upload process

The following flowchart shows the basic process of multipart upload.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7796498951/p1058.png)

Process description:

1.  Split the object that you want to upload into parts based on a specific size.

    All parts except the last part must be larger than or equal to 100 KB in size.

2.  Use the [InitiateMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/InitiateMultipartUpload.md) operation to initialize a multipart upload task.
3.  Use the [UploadPart](/intl.en-US/API Reference/Object operations/Multipart upload/UploadPart.md) operation to upload parts.

    After the object is split into parts, the part sequence is determined by `partNumber` specified in the upload process. These parts can be concurrently uploaded. However, the larger number of parts does not necessarily increase upload speeds. You must specify the number of parts based on your network and device load balancing status.

    If you want to terminate an upload task, you can call the [AbortMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/AbortMultipartUpload.md) operation. The uploaded parts are deleted.

4.  Use the [CompleteMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/CompleteMultipartUpload.md) operation to combine parts into an object.

## Implementation modes for multipart upload

|Implementation mode|Description|
|-------------------|-----------|
|[ossutil](/intl.en-US/Tools/ossutil/Common commands/cp/Overview.md)|A high-performance command-line tool|
|[Java SDK](/intl.en-US/SDK Reference/Java/Upload objects/Multipart upload.md)|SDK demos for various programming languages|
|[Python SDK](/intl.en-US/SDK Reference/Python/Upload objects/Multipart upload.md)|
|[PHP SDK](/intl.en-US/SDK Reference/PHP/Upload objects/Multipart upload.md)|
|[Go SDK](/intl.en-US/SDK Reference/Go/Upload objects/Multipart upload.md)|
|[C SDK](/intl.en-US/SDK Reference/C/Upload objects/Multipart upload.md)|
|[.NET SDK](/intl.en-US/SDK Reference/. NET/Upload objects/Multipart upload.md)|
|[Node.js SDK](/intl.en-US/SDK Reference/Node. js/Upload objects/Multipart upload.md)|
|[Android SDK](/intl.en-US/SDK Reference/Android/Upload objects/Multipart upload.md)|
|[iOS SDK](/intl.en-US/SDK Reference/iOS/Upload objects/Multipart upload.md)|
|[Browser.js SDK](/intl.en-US/SDK Reference/Browser.js/Upload objects.md)|

## Usage notes

-   Size limit
    -   The size of the object you want to upload cannot exceed 48.8 TB.
    -   Each object can be split into up to 10,000 parts. The size of each part except the last part can range from 100 KB to 5 GB. The size of the last part can be smaller than 100 KB.
-   Delete parts

    After the multipart upload process is interrupted, uploaded parts are saved in the specified bucket. To avoid additional storage fees, we recommend that you use the following methods to delete these parts if you no longer use these parts.

    -   Manually delete parts. For more information, see [Delete parts](/intl.en-US/Console User Guide/Upload, download, and manage objects/Manage parts.md).
    -   Use lifecycle rules to automatically delete parts. For more information, see [Configure lifecycle rules](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure lifecycle rules.md).
-   Naming conventions
    -   The name can contain only UTF-8 characters.
    -   The name must be 1 to 1,023 bytes in length.
    -   The name cannot start with a forward slash \(/\) or backslash \(\\\).
-   Optimize performance for object upload

    If you upload a large number of objects with sequential prefixes such as timestamps and letters in the object names, multiple object indexes may be stored in a single partition. If too many requests are sent to query these objects, the responsiveness may become slow. We recommend that you do not upload a large number of objects by using sequential prefixes. For more information, see [OSS performance and scalability best practices](/intl.en-US/Best Practices/OSS performance and scalability best practices.md).

-   Overwrite objects

    When you upload an object that shares the same name as an object in OSS, the existing OSS object is overwritten by the uploaded object. You can use the following methods to prevent objects from being unexpectedly overwritten:

    -   Enable versioning

        When versioning is enabled, overwritten objects are saved as previous versions. You can recover previous versions at any time. For more information, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md).

    -   Include a parameter in the upload request header

        Include the x-oss-forbid-overwrite parameter in the upload request header, and set the parameter value to `true`. This way, when you upload an object whose object name is the same as that of an existing object, the object fails to be uploaded. OSS returns the `FileAlreadyExists` error. For more information, see [InitiateMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/InitiateMultipartUpload.md).


## Implementation modes for resumable upload

If the system stops responding during multipart upload, you can resume the upload by calling the [ListMultipartUploads](/intl.en-US/API Reference/Object operations/Multipart upload/ListMultipartUploads.md) and [ListParts](/intl.en-US/API Reference/Object operations/Multipart upload/ListParts.md) operations to retrieve all multipart upload tasks on an object and list the uploaded parts in each task. This method allows an upload task to be resumed from the last uploaded part.

|Implementation mode|Description|
|-------------------|-----------|
|[Java SDK](/intl.en-US/SDK Reference/Java/Upload objects/Resumable upload.md)|SDK demos for various programming languages|
|[Python SDK](/intl.en-US/SDK Reference/Python/Upload objects/Resumable upload.md)|
|[Go SDK](/intl.en-US/SDK Reference/Go/Upload objects/Resumable upload.md)|
|[C SDK](/intl.en-US/SDK Reference/C/Upload objects/Resumable upload.md)|
|[.NET SDK](/intl.en-US/SDK Reference/. NET/Upload objects/Resumable upload.md)|
|[Node.js SDK](/intl.en-US/SDK Reference/Node. js/Upload objects/Resumable upload.md)|
|[Android SDK](/intl.en-US/SDK Reference/Android/Upload objects/Resumable upload.md)|
|[iOS SDK](/intl.en-US/SDK Reference/iOS/Upload objects/Resumable upload.md)|

