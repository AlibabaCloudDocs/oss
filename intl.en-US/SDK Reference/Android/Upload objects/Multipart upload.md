# Multipart upload

By using the multipart upload feature provided by OSS, you can split a large object into multiple parts and upload them separately. After all parts are uploaded, call the CompleteMultipartUpload operation to combine these parts into a single object to implement resumable upload.

## Multipart upload process

To implement multipart upload, perform the following operations:

1.  Initiate a multipart upload task.

    You can call the oss.initMultipartUpload method to create a unique upload ID in OSS.

2.  Upload the parts.

    You can call the oss.uploadPart method to upload parts.

    **Note:**

    -   Part numbers identify the relative positions of parts in an object that share the same upload ID. If you have uploaded a part and used its part number again to upload another part, the latter part overwrites the former part.
    -   OSS includes the MD5 hash of part data in the ETag header and returns the MD5 hash to the user.
    -   OSS calculates the MD5 hash of uploaded data and compares it with the MD5 hash calculated by the SDK. If the two hashes are different, the InvalidDigest error code is returned.
3.  Complete the multipart upload.

    After you have uploaded all parts, you can call the oss.CompleteMultipartUpload method to combine the parts into a complete object.


## Complete sample code

The following code provides a complete example that describes the process of multipart upload:

```
// Initiate a multipart upload task.
// InitiateMultipartUploadRequest specifies the name of the object that you want to upload and the name of the destination bucket.
// objectKey is equivalent to objectName and indicates the complete path of the object that you want to upload to OSS. The path must include the file extension of the object. For example, you can set objectKey to abc/efg/123.jpg.
InitiateMultipartUploadRequest init = new InitiateMultipartUploadRequest("<bucketName>", "<objectKey>");
InitiateMultipartUploadResult initResult = oss.initMultipartUpload(init);
// The response to initResult contains the upload ID. The upload ID is the unique identifier of the multipart upload task.
String uploadId = initResult.getUploadId();

// Set the size of a single part. Unit: Byte. Valid values: 100 KB to 5 GB.
int partCount = 100*1024;
// Multipart upload.
for (int i = 1; i < 5; i++) {
    byte[] data = new byte[partCount];

    RandomAccessFile raf = new RandomAccessFile("path", "r");
    long skip = (i-1) * partCount;
    raf.seek(skip);
    raf.readFully(data, 0, partCount);

    UploadPartRequest uploadPart = new UploadPartRequest();
    uploadPart.setBucketName(mBucketName);
    uploadPart.setObjectKey(objectKey);
    uploadPart.setUploadId(uploadId);
    uploadPart.setPartNumber(i); // The part numbers of the parts to upload start from 1.
    uploadPart.setPartContent(data);
    try {
        oss.uploadPart(uploadPart);
    } catch (ServiceException serviceException) {
        OSSLog.logError(serviceException.getErrorCode());
    }
}

// Complete the multipart upload task.
CompleteMultipartUploadRequest complete = new CompleteMultipartUploadRequest("<bucketName>", "<objectName>", "<uploadId>", "<partETagList>";

// Configure upload callback. You can set the CALLBACK_SERVER parameter to complete the multipart upload request. A callback request is sent to the specified server address after the multipart upload task is complete. You can view the servercallback result in completeResult.getServerCallbackReturnBody() of the response.
complete.setCallbackParam(new HashMap<String, String>() {
    {
        put("callbackUrl", CALLBACK_SERVER); // Set the CALLBACK_SERVER parameter to your server address.
        put("callbackBody", "test");
    }
});
CompleteMultipartUploadResult completeResult = oss.completeMultipartUpload(complete);
OSSLog.logError("-------------- serverCallback: " + completeResult.getServerCallbackReturnBody());
```

The preceding code uses uploadPart to upload each part.

-   You must specify the upload ID and part numbers for each multipart upload request. Valid values of PartNumber range from 1 to 10000. If a part number is not within this range, the InvalidArgument error code is returned.
-   If you use uploadPart, each part except for the last part must be larger than 100 KB in size. After all parts are uploaded by using uploadPart, the size of each part is verified.
-   When you upload each part, make sure that the stream is directed to the start position of the part to upload.
-   Each time a part is uploaded, OSS returns a response that contains the ETag value of the part. If the ETag value is the same as the MD5 hash, combine the ETag value with the part number into a PartETag and save the PartETag for subsequent uploads of parts.

## Upload a file by using multipart upload

You can use one of the following multipart upload methods to upload a file from its file path:

-   Method 1

    ```
    ObjectMetadata meta = new ObjectMetadata();
    // Configure object metadata.
    meta.setHeader("x-oss-object-acl", "public-read-write");
    MultipartUploadRequest rq = new MultipartUploadRequest("<bucketName>", "<objkey>",
            "<filepath>", meta);
    // Set PartSize. The default value of PartSize is 256 KB. The minimum value is 100 KB.
    rq.setPartSize(1024 * 1024);
    rq.setProgressCallback(new OSSProgressCallback<MultipartUploadRequest>() {
        @Override
        public void onProgress(MultipartUploadRequest request, long currentSize, long totalSize) {
            OSSLog.logDebug("[testMultipartUpload] - " + currentSize + " " + totalSize, false);
        }
    });
    
    CompleteMultipartUploadResult result = oss.multipartUpload(rq);
    ```

-   Method 2

    ```
    MultipartUploadRequest request = new MultipartUploadRequest("<bucketName>", "<objkey>",
                    "<filepath>");
    
    request.setProgressCallback(new OSSProgressCallback<MultipartUploadRequest>() {
        @Override
        public void onProgress(MultipartUploadRequest request, long currentSize, long totalSize) {
            OSSLog.logDebug("[testMultipartUpload] - " + currentSize + " " + totalSize, false);
        }
    });
    
    OSSAsyncTask task = oss.asyncMultipartUpload(request, new OSSCompletedCallback<MultipartUploadRequest, CompleteMultipartUploadResult>() {
        @Override
        public void onSuccess(MultipartUploadRequest request, CompleteMultipartUploadResult result) {
            OSSLog.logInfo(result.getServerCallbackReturnBody());
        }
    
        @Override
        public void onFailure(MultipartUploadRequest request, ClientException clientException, ServiceException serviceException) {
            OSSLog.logError(serviceException.getRawMessage());
        }
    });
    
    // Thread.sleep(100);
    // Cancel the multipart upload task.
    // task.cancel();   
    
    task.waitUntilFinished();
    ```


## List uploaded parts

You can call the oss.listParts method to obtain all the parts that are uploaded in a multipart upload task.

The following code provides an example on how to list uploaded parts:

```
// List uploaded parts.
ListPartsRequest listParts = new ListPartsRequest("<bucketName>", "<objectName>", "<uploadId>");
ListPartsResult result = oss.listParts(listParts);

List<PartETag> partETagList = new ArrayList<PartETag>();
for (PartSummary part : result.getParts()) {
    partETagList.add(new PartETag(part.getPartNumber(), part.getETag()));
}
```

**Note:** By default, if a bucket contains more than 1,000 parts that are uploaded using multipart upload, OSS returns the first 1,000 parts. In the response, the IsTruncated field is set to false and NextPartNumberMarker is returned to indicate the next starting position to continue reading data.

## Cancel a multipart upload task

You can call the oss.abortMultipartUpload method to cancel a multipart upload request based on the specified upload ID. If you cancel a multipart upload task, you cannot use the upload ID to upload parts. The uploaded parts are deleted.

The following code provides an example on how to cancel a multipart upload task:

```
// Cancel the multipart upload task.
AbortMultipartUploadRequest abort = new AbortMultipartUploadRequest("<bucketName>", "<objectName>", "<uploadId>");
AbortMultipartUploadResult abortResult = oss.abortMultipartUpload(abort);
```

