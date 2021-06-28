# Multipart upload and resumable upload

Alibaba Cloud Object Storage Service \(OSS\) provides multipart upload for you to split and upload an object into multiple parts. After the parts are uploaded, you can call CompleteMultipartUpload to assemble these parts into the original object. If the upload fails due to network errors, you can continue the upload from the last uploaded part to implement resumable upload.

## Scenarios

-   Accelerated upload of large objects

    When the object that you want to upload is larger than 5 GB, you can use multipart upload to split the object into parts and upload the parts in parallel to accelerate the upload.

-   Poor network environments

    We recommend that you use multipart upload in a network environment that is poor This way, when an upload task fails, you only need to upload the parts that failed.

-   Stream upload

    You can use stream upload to upload objects of unknown sizes. This scenario is common in industry applications such as video surveillance.


## Multipart upload process

The following flowchart shows the basic process of multipart upload.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7796498951/p1058.png)

The preceding process consists of the following steps:

1.  Split the object that you want to upload into parts based on a specific size.
2.  Call the [InitiateMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/InitiateMultipartUpload.md) operation to initialize a multipart upload task.
3.  Call the [UploadPart](/intl.en-US/API Reference/Object operations/Multipart upload/UploadPart.md) operation to upload the parts.

    After an object is split into parts, a `partNumber` is specified for each part to indicate the sequence of the parts. Therefore, you can upload the parts in parallel without worrying about the part sequence. A larger amount of concurrent uploads does not necessarily increase upload speeds. Therefore, we recommend that you specify the number of concurrent uploads base on your network conditions and the workload of your devices.

    If you want to cancel a multipart upload task, you can call the [AbortMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/AbortMultipartUpload.md) operation. After a multipart upload task is canceled, parts that are uploaded by the task are also deleted.

4.  Call the [CompleteMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/CompleteMultipartUpload.md) operation to combine the uploaded parts into an object.

## Limits

|Item|Limit|
|----|-----|
|Object size|Multipart upload supports objects up to 48.8 TB in size.|
|Number of parts|You can set the number of parts to a value that ranges from 1 to 10000|
|Part size|Each part can be 100 KB to 5 GB in size. The size of the last part is not limited.|
|Maximum number of parts that can be returned for a single ListParts operation|Up to 1,000 parts can be returned for a single ListParts operation.|
|Maximum number of multipart upload tasks that can be returned for a single ListMultipartUploads operation|Up to 1,000 multipart upload tasks can be returned for a single ListMultipartUploads operation.|

## Usage notes

-   Optimize performance for object upload

    If you upload a large number of objects whose names have sequential prefixes such as timestamps and letters, multiple object indexes may be stored in a single partition. If an excessive number of requests are sent to query these objects, the responsiveness may become slow. We recommend that you do not upload a large number of objects whose names have sequential prefixes. For more information, see [OSS performance and scalability best practices](/intl.en-US/Best Practices/OSS performance and scalability best practices.md).

-   Overwrite objects

    If you upload an object whose name is the same as an existing object in OSS, the existing object is overwritten. You can use the following methods to prevent objects from being unexpectedly overwritten:

    -   Enable versioning

        When versioning is enabled for a bucket, overwritten objects are saved as previous versions. You can recover a previous version at any time. For more information, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md).

    -   Add a specific header in the upload request

        Add the x-oss-forbid-overwrite header in the upload request and set the parameter value to `true`. This way, when you upload an object whose object name is the same as that of an existing object, the object fails to be uploaded. OSS returns the `FileAlreadyExists` error. For more information, see [InitiateMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/InitiateMultipartUpload.md).

-   Delete parts

    When a multipart upload task is interrupted, parts that are uploaded by the task are stored in the specified bucket. To avoid additional storage fees, we recommend that you use the following methods to delete these parts if you no longer use these parts.

    -   Manually delete the parts. For more information, see [Delete parts](/intl.en-US/Console User Guide/Upload, download, and manage objects/Manage parts.md).
    -   Configure lifecycle rules to automatically delete the parts. For more information, see [Configure lifecycle rules](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure lifecycle rules.md).

## Implementation methods for multipart upload

|Implementation method|Description|
|---------------------|-----------|
|[ossutil](/intl.en-US/Tools/ossutil/Common commands/cp/Overview.md)|A high-performance command-line tool|
|[OSS SDK for Java](/intl.en-US/SDK Reference/Java/Upload objects/Multipart upload.md)|SDK demos for various programming languages|
|[OSS SDK for Python](/intl.en-US/SDK Reference/Python/Upload objects/Multipart upload.md)|
|[OSS SDK for PHP](/intl.en-US/SDK Reference/PHP/Upload objects/Multipart upload.md)|
|[OSS SDK for Go](/intl.en-US/SDK Reference/Go/Upload objects/Multipart upload.md)|
|[OSS SDK for C](/intl.en-US/SDK Reference/C/Upload objects/Multipart upload.md)|
|[OSS SDK for .NET](/intl.en-US/SDK Reference/. NET/Upload objects/Multipart upload.md)|
|[OSS SDK for Node.js](/intl.en-US/SDK Reference/Node. js/Upload objects/Multipart upload.md)|
|[OSS SDK for Android](/intl.en-US/SDK Reference/Android/Upload objects/Multipart upload.md)|
|[OSS SDK for iOS](/intl.en-US/SDK Reference/iOS/Upload objects/Multipart upload.md)|
|[OSS SDK for Browser.js](/intl.en-US/SDK Reference/Browser.js/Upload objects.md)|

## Implementation methods for resumable upload

If a multipart upload task fails, you can call the [ListMultipartUploads](/intl.en-US/API Reference/Object operations/Multipart upload/ListMultipartUploads.md) operation to query all multipart upload tasks that are initiated to upload a specific object and call the [ListParts](/intl.en-US/API Reference/Object operations/Multipart upload/ListParts.md) operation to list the uploaded parts in each multipart upload task. Then, you can continue the upload from the last uploaded part to implement resumable upload.

|Implementation method|Description|
|---------------------|-----------|
|[OSS SDK for Java](/intl.en-US/SDK Reference/Java/Upload objects/Resumable upload.md)|SDK demos for various programming languages|
|[OSS SDK for Python](/intl.en-US/SDK Reference/Python/Upload objects/Resumable upload.md)|
|[OSS SDK for Go](/intl.en-US/SDK Reference/Go/Upload objects/Resumable upload.md)|
|[OSS SDK for C](/intl.en-US/SDK Reference/C/Upload objects/Resumable upload.md)|
|[OSS SDK for .NET](/intl.en-US/SDK Reference/. NET/Upload objects/Resumable upload.md)|
|[OSS SDK for Node.js](/intl.en-US/SDK Reference/Node. js/Upload objects/Resumable upload.md)|
|[OSS SDK for Android](/intl.en-US/SDK Reference/Android/Upload objects/Resumable upload.md)|
|[OSS SDK for iOS](/intl.en-US/SDK Reference/iOS/Upload objects/Resumable upload.md)|

