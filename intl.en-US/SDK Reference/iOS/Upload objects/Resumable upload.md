# Resumable upload

In a wireless network environment, it takes a long time to upload a large object and the upload may fail due to poor network connectivity or network switches. If the upload fails, the entire object must be uploaded again. To resolve this problem, OSS SDK for iOS provides the resumable upload feature.

## Background information

We recommend that you do not use resumable upload when you upload objects smaller than 5 GB in size from a mobile device. Resumable upload is implemented based on the multipart upload method. Resumable upload of a single object requires multiple network requests, which is inefficient. The following section provides the precautions you need to understand before and during resumable upload.

-   Before resumable upload

    Before you upload an object to OSS by using the resumable upload method, you can specify a folder for the checkpoint file that stores resumable upload records. The checkpoint file applies only to the current resumable upload task.

    -   If you do not specify the folder for the checkpoint file and part of a large object fails to be uploaded due to network problems, it takes a large amount of time and consumes a large amount of traffic to upload the entire large object again.
    -   If you specify the folder for the checkpoint file, a failed resumable upload task can be resumed from the position recorded in the checkpoint file.
-   During resumable upload

    When you use resumable upload, you must understand the following information:

    -   Resumable upload allows you to upload only local files. Resumable upload supports the upload callback feature, which is used in the same way as it is in common upload tasks. For more information, see [Callback](/intl.en-US/API Reference/Object operations/Basic operations/Callback.md).
    -   Resumable upload is implemented by calling the following API operations: InitMultipartUpload, UploadPart, ListParts, CompleteMultipartUpload, and AbortMultipartUpload. If you want to use the STS authentication mode to perform resumable upload tasks, ensure that you are authorized to call the preceding API operations.
    -   By default, the MD5 hash of each part is verified in a resumable upload task. Therefore, you do not need to specify the `Content-Md5` header in the request.
    -   When a resumable upload task fails, the uploaded parts become useless in OSS. You can configure lifecycle rules for a bucket to delete expired parts. For more information, see [Manage lifecycle rules](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure lifecycle rules.md).

        **Note:** By default, if the current upload task is canceled, the parts uploaded to the server are deleted synchronously. If you want to retain resumable upload records, you must specify a folder to store the checkpoint file that contains the resumable upload records and modify the setting of the deleteUploadIdOnCancelling parameter. If resumable upload records are retained on the mobile device for an extended period of time but the parts uploaded to the server have been deleted by the configured lifecycle rules of the bucket, the resumable upload records on the server may be different from those on the mobile device.


## Sample code

The following sections provide examples on how to use resumable upload.

-   The following code provides an example on how to call the `resumableUpload` method to perform a resumable upload task when the checkpoint file is permanently stored in the local device:

    ```
    OSSResumableUploadRequest * resumableUpload = [OSSResumableUploadRequest new];
    resumableUpload.bucketName = OSS_BUCKET_PRIVATE;
    //...
    NSString *cachesDir = [NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES) firstObject];
    resumableUpload.recordDirectoryPath = cachesDir;
                        
    ```

    **Note:** When a resumable upload task fails, a checkpoint file is generated to record the resumable upload progress. The next upload starts from the position recorded in the checkpoint file to upload the entire object. After the object is uploaded, the checkpoint file is automatically deleted. By default, the checkpoint file is not retained permanently on the local device.

-   The following code provides a complete example on how to perform a resumable upload task:

    ```
    // Obtain an upload ID to upload an object.
    OSSResumableUploadRequest * resumableUpload = [OSSResumableUploadRequest new];
    resumableUpload.bucketName = <bucketName>;
    // objectKey is equivalent to objectName that indicates the complete path of the object you want to upload to OSS by using resumable upload. The path must include the extension of the object. For example, you can set objectKey to abc/efg/123.jpg.
    resumableUpload.objectKey = <objectKey>;
    resumableUpload.partSize = 1024 * 1024;
    resumableUpload.uploadProgress = ^(int64_t bytesSent, int64_t totalByteSent, int64_t totalBytesExpectedToSend) {
        NSLog(@"%lld, %lld, %lld", bytesSent, totalByteSent, totalBytesExpectedToSend);
    };
    NSString *cachesDir = [NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES) firstObject];
    // Specify the path to store the checkpoint file.
    resumableUpload.recordDirectoryPath = cachesDir;
    // Set the deleteUploadOnCancelling parameter to NO. NO indicates that the checkpoint file is not deleted when the upload task fails. The next upload starts from the position recorded in the checkpoint file to upload the entire object. If you do not specify this parameter, default value YES is used. YES indicates that the checkpoint file is deleted when the upload task fails. The entire object is uploaded again during the next upload.
    resumableUpload.deleteUploadIdOnCancelling = NO;
    
    resumableUpload.uploadingFileURL = [NSURL fileURLWithPath:<your file path>];
    OSSTask * resumeTask = [client resumableUpload:resumableUpload];
    [resumeTask continueWithBlock:^id(OSSTask *task) {
        if (task.error) {
            NSLog(@"error: %@", task.error);
            if ([task.error.domain isEqualToString:OSSClientErrorDomain] && task.error.code == OSSClientErrorCodeCannotResumeUpload) {
                // The task cannot be resumed. You must obtain a new upload ID to upload the object.
            }
        } else {
            NSLog(@"Upload file success");
        }
        return nil;
    }];
    
    // [resumeTask waitUntilFinished];
    
    // [resumableUpload cancel];
                        
    ```


