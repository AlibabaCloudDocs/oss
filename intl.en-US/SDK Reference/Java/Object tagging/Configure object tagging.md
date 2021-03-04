# Configure object tagging

OSS allows you to configure object tagging to classify objects. You can configure object lifecycle rules and ACLs based on tags.

## Background information

When you configure object tagging, note the following items:

-   You can add tags to an object when you upload it or add tags to an uploaded object. If you add tags to an object that already has tags, the original tags are overwritten. For more information about how to configure object tagging, see [t159899.md\#](/intl.en-US/API Reference/Object operations/Tagging/PutObjectTagging.md).
-   To add tags to an object, you must have the permissions to call PutObjectTagging.
-   The Last-Modified value of an object is not updated when its tags are changed.
-   The key and value of the tag can contain letters, digits, spaces, and the following special characters:

    + ‑ = . \_ : /


Object tagging uses a key-value pair to identify objects. For more information about object tagging, see [Configure object tagging](/intl.en-US/Developer Guide/Objects/Manage files/Configure object tagging.md).

## Add tags to an object when you upload it

The following examples describe how to add tags to objects in simple upload, multipart upload, append upload, and resumable upload.

-   Add tags to an object when you upload it in simple upload.

    The following code provides an example on how to add tags to an object when you upload it by calling PutObject:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    String objectName = "<yourObjectName>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    Map<String, String> tags = new HashMap<String, String>();
    tags.put("key1", "value1");
    tags.put("key2", "value2");
    
    // Configure the tags in the HTTP headers.
    ObjectMetadata metadata = new ObjectMetadata();
    metadata.setObjectTagging(tags);
    
    // Upload the object and add tags to it.
    String content = "<yourtContent>";
    ossClient.putObject(bucketName, objectName, new ByteArrayInputStream(content.getBytes()), metadata);
    
    // Shut down OSSClient.
    ossClient.shutdown();
    ```

-   Add tags to an object when you upload it by using multipart upload.

    The following code provides an example on how to add tags to an object when you upload it by calling MultipartUpload:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    String objectName = "<yourObjectName>";
    String localFile = "<yourLocalFile>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    /* Step 1: Initialize a multipart upload task.
    */
    // Configure the tags in the HTTP header.
    Map<String, String> tags = new HashMap<String, String>();
    tags.put("key1", "value1");
    tags.put("key2", "value2");
    
    ObjectMetadata metadata = new ObjectMetadata();
    metadata.setObjectTagging(tags);
    
    // Send an InitiateMultipartUploadRequest request and add tags to the object.
    InitiateMultipartUploadRequest request = new InitiateMultipartUploadRequest(bucketName, objectName, metadata);
    InitiateMultipartUploadResult result = ossClient.initiateMultipartUpload(request);
    // Obtain the uploadId, which uniquely identifies the multipart upload task. You can use it to cancel or query the multipart upload task.
    String uploadId = result.getUploadId();
    
    /* Step 2: Upload parts.
    */
    // partETags is a set of ETags of parts. A PartETag consists of an ETag and a part number.
    List<PartETag> partETags =  new ArrayList<PartETag>();
    // Calculate the total number of upload parts.
    final long partSize = 1 * 1024 * 1024L;   // 1MB
    final File sampleFile = new File("<localFile>");
    long fileLength = sampleFile.length();
    int partCount = (int) (fileLength / partSize);
    if (fileLength % partSize ! = 0) {
        partCount++;
     }
    // Upload each part sequentially until all parts are uploaded.
    for (int i = 0; i < partCount; i++) {
        long startPos = i * partSize;
        long curPartSize = (i + 1 == partCount) ? (fileLength - startPos) : partSize;
        InputStream instream = new FileInputStream(sampleFile);
        // Skip the parts that have been uploaded.
        instream.skip(startPos);
        UploadPartRequest uploadPartRequest = new UploadPartRequest();
        uploadPartRequest.setBucketName(bucketName);
        uploadPartRequest.setKey(objectName);
        uploadPartRequest.setUploadId(uploadId);
        uploadPartRequest.setInputStream(instream);
        // Configure the size of each part. Each part must be larger than 100 KB, with the exception of the last part.
        uploadPartRequest.setPartSize(curPartSize);
        // Configure part numbers. Each part is configured with a part number. The number can be from 1 to 10000. If you configure a number beyond the range, OSS returns an InvalidArgument error code.
        uploadPartRequest.setPartNumber( i + 1);
      // Parts are not necessarily uploaded in order. They may be uploaded from different OSS clients. OSS sorts the parts based on their part numbers and combines them to obtain a complete object.
        UploadPartResult uploadPartResult = ossClient.uploadPart(uploadPartRequest);
      // When you have uploaded a part, OSS returns a result that contains a PartETag. The PartETag is stored in partETags.
        partETags.add(uploadPartResult.getPartETag());
    }
    
    /* Step 3: Complete multipart upload.
    */
    // partETags must be sequenced by part numbers in ascending order.
    Collections.sort(partETags, new Comparator<PartETag>() {
        public int compare(PartETag p1, PartETag p2) {
            return p1.getPartNumber() - p2.getPartNumber();
        }
    });
    // You must provide all valid PartETags when you perform this operation. OSS verifies the validity of all parts one by one after it receives PartETags. After part verification is complete, OSS combines these parts into a complete object.
    CompleteMultipartUploadRequest completeMultipartUploadRequest =
            new CompleteMultipartUploadRequest(bucketName, objectName, uploadId, partETags);
    ossClient.completeMultipartUpload(completeMultipartUploadRequest);
    
    // View the tags added to the object.
    TagSet tagSet = ossClient.getObjectTagging(bucketName, objectName);
    Map<String, String> getTags = tagSet.getAllTags();
    System.out.println("object tagging: "+ getTags.toString());
    
    // Shut down OSSClient.
    ossClient.shutdown();
    ```

-   Add tags to an object when you upload it by using append upload.

    The following code provides an example on how to add tags to an object when you upload it by calling AppendObject:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    String objectName = "<yourObjectName>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    String content1 = "Hello OSS A \n";
    String content2 = "Hello OSS B \n";
    String content3 = "Hello OSS C \n";
    
    Map<String, String> tags = new HashMap<String, String>();
    tags.put("key1", "value1");
    tags.put("key2", "value2");
    
    ObjectMetadata meta = new ObjectMetadata();
    // Configures the tags for the uploaded object.
     meta.setObjectTagging(tags);
    // Specify the type of content you want to upload.
    meta.setContentType("text/plain");
    
    // Configure parameters with AppendObjectRequest.
    AppendObjectRequest appendObjectRequest = new AppendObjectRequest(bucketName, objectName, new ByteArrayInputStream(content1.getBytes()), meta);
    
    // Configure a single parameter with AppendObjectRequest.
    // Specify the bucket name.
    //appendObjectRequest.setBucketName("<yourBucketName>");
    // Specify the object name.
    //appendObjectRequest.setKey("<yourObjectName>");
    // Configure the type of content you want to append. Two types of content are available: InputStream and File. Select InputStream.
    //appendObjectRequest.setInputStream(new ByteArrayInputStream(content1.getBytes()));
    // Configure the type of content you want to append. Two types of content are available: InputStream and File. Select File.
    //appendObjectRequest.setFile(new File("<yourLocalFile>"));
    // Specify object metadata. Only the metadata specified in the first append takes effect.
    //appendObjectRequest.setMetadata(meta);
    
    // Start the first append. Only the tags configured when the object is appended for the first time take effect.
    // Configure the position from where the object is appended.
    appendObjectRequest.setPosition(0L);
    AppendObjectResult appendObjectResult = ossClient.appendObject(appendObjectRequest);
    // Calculate the CRC-64 value. The value is calculated based on the ECMA-182 standard.
    System.out.println(appendObjectResult.getObjectCRC());
    
    // Start the second append.
    // nextPosition indicates the position provided for the next request, that is, the length of the current object.
    appendObjectRequest.setPosition(appendObjectResult.getNextPosition());
    appendObjectRequest.setInputStream(new ByteArrayInputStream(content2.getBytes()));
    appendObjectResult = ossClient.appendObject(appendObjectRequest);
    
    // Start the third append.
    appendObjectRequest.setPosition(appendObjectResult.getNextPosition());
    appendObjectRequest.setInputStream(new ByteArrayInputStream(content3.getBytes()));
    appendObjectResult = ossClient.appendObject(appendObjectRequest);
    
    // View the tags added to the uploaded object.
    TagSet tagSet = ossClient.getObjectTagging(bucketName, objectName);
    Map<String, String> getTags = tagSet.getAllTags();
    System.out.println("object tagging: "+ getTags.toString());
    
    // Shut down OSSClient.
    ossClient.shutdown();
    ```

-   Add tags to an object when you upload it by using resumable upload.

    The following code provides an example on how to add tags to an object when you upload it by using resumable upload:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Configure the tags to be added to the object.
    Map<String, String> tags = new HashMap<String, String>();
    tags.put("key1", "value1");
    tags.put("key2", "value2");
    
    ObjectMetadata meta = new ObjectMetadata();
    // Specify the type of content you want to upload.
    meta.setContentType("text/plain");
    // Add the tags to the object.
    meta.setObjectTagging(tags);
    
    // Configure parameters at a time with UploadFileRequest.
    UploadFileRequest uploadFileRequest = new UploadFileRequest("<yourBucketName>","<yourObjectName>");
    
    // Configure parameters separately with UploadFileRequest.
    // Specify the bucket name.
    //uploadFileRequest.setBucketName("<yourBucketName>");
    // Specify the object name.
    //uploadFileRequest.setKey("<yourObjectName>");
    // Specify the local file to upload.
    uploadFileRequest.setUploadFile("<yourLocalFile>");
    // Specify the number of threads for simultaneous upload. The default value is 1.
    uploadFileRequest.setTaskNum(5);
    // Specify the size of each part to upload. Valid values: 100 KB to 5 GB. The default size of each part is calculated as follows: Object size/10000.
    uploadFileRequest.setPartSize(1 * 1024 * 1024);
    // Specify whether to enable resumable upload. By default, resumable upload is disabled.
    uploadFileRequest.setEnableCheckpoint(true);
    // Specify the checkpoint file that records the upload results of each part. You must set this parameter if you want to perform resumable upload by using multipart upload. The checkpoint file stores the progress information about a multipart upload task. If a part fails to upload, the task can be continued based on the progress recorded in the checkpoint file. After the object is uploaded, the file is deleted. By default, this file shares the same directory (uploadFile.ucp) as the file you want to upload.
    uploadFileRequest.setCheckpointFile("<yourCheckpointFile>");
    // Configure object metadata.
    uploadFileRequest.setObjectMetadata(meta);
    // Configure upload callback. The parameter type is Callback.
    uploadFileRequest.setCallback("<yourCallbackEvent>");
    
    // Start the resumable upload task and configure the tags to be added to the object.
    ossClient.uploadFile(uploadFileRequest);
    
    // View the tags added to the object.
    TagSet tagSet = ossClient.getObjectTagging(bucketName, objectName);
    Map<String, String> getTags = tagSet.getAllTags();
    System.out.println("object tagging: "+ getTags.toString());
    
    // Shut down OSSClient.
    ossClient.shutdown();
    ```


## Add tags to an uploaded object or modify the tags added to an object

The following code provides an example on how to add tags to an uploaded object or modify the tags added to an object:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String objectName = "<yourObjectName>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

Map<String, String> tags = new HashMap<String, String>();
tags.put("key1", "value1");
tags.put("key2", "value2");

// Add tags to the object.
ossClient.setObjectTagging(bucketName, objectName, tags);

// Shut down OSSClient.
ossClient.shutdown();
```

## Add tags to an object when you copy it

You can specify one of the following tagging rules when you upload an object: Valid values:

-   Copy: The tag of the source object is copied to the destination object. This is the default value.
-   Replace: The tag of the destination object is set to the tag specified in the request instead of the tag of the source object.

The following examples describe how to add tags to objects smaller than 1 GB in simple copy mode and larger than 1 GB in multipart copy mode.

-   The following code provides an example on how to add tags to an object smaller than 1 GB when you copy it by calling CopyObject:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    String sourceBucketName = "<yourSourceBucketName>";
    String sourceObjectName = "<yourSourceObjectName>";
    String destinationBucketName = "<yourDestinationBucketName>";
    String destinationObjectName = "<yourDestinationObjectName>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Create a CopyObjectRequest object.
    CopyObjectRequest copyObjectRequest = new CopyObjectRequest(sourceBucketName, sourceObjectName, destinationBucketName, destinationObjectName);
    
    // Configure the tags to be added to the object. If the headers are not configured, the tags of the source object are added to the destination object by default.
    Map<String, String> headers = new HashMap<String, String>();
    headers.put(OSSHeaders.COPY_OBJECT_TAGGING_DIRECTIVE, "REPLACE");
    headers.put(OSSHeaders.OSS_TAGGING, "key1=value1&key2=value2");
    copyObjectRequest.setHeaders(headers);
    
    // Copy the source object to the destination object.
    CopyObjectResult result = ossClient.copyObject(copyObjectRequest);
    System.out.println("ETag: " + result.getETag() + " LastModified: " + result.getLastModified());
    
    // View the tags added to the destination object.
    TagSet tagSet = ossClient.getObjectTagging(bucketName, destinationObjectName);
    Map<String, String> getTags = tagSet.getAllTags();
    System.out.println("dest object tagging: "+ getTags.toString());
    
    // Shut down OSSClient.
    ossClient.shutdown();
    ```

-   The following code provides an example on how to add tags to an object larger than 1 GB when you copy it by calling MultipartUpload:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    String sourceBucketName = "<yourSourceBucketName>";
    String sourceObjectName = "<yourSourceObjectName>";
    String destinationBucketName = "<yourDestinationBucketName>";
    String destinationObjectName = "<yourDestinationObjectName>";
    
    ObjectMetadata objectMetadata = ossClient.getObjectMetadata(sourceBucketName, sourceObjectName);
    // Query the size of the object to copy.
    long contentLength = objectMetadata.getContentLength();
    // Set the size of each part to 10 MB.
    long partSize = 1024 * 1024 * 10;
    // Calculate the total number of parts.
    int partCount = (int) (contentLength / partSize);
    if (contentLength % partSize ! = 0) {
        partCount++;
    }
    
    System.out.println("total part count:" + partCount);
    
    // Configure the tags in the HTTP headers.
    Map<String, String> tags2 = new HashMap<String, String>();
    tags2.put("key3", "value3");
    tags2.put("key4", "value4");
    
    ObjectMetadata metadata = new ObjectMetadata();
    metadata.setObjectTagging(tags2);
    
    // Initiate a multipart copy task and configure the tags to be added to the destination object.
    InitiateMultipartUploadRequest initiateMultipartUploadRequest = new InitiateMultipartUploadRequest(destinationBucketName, destinationObjectName, metadata);
    InitiateMultipartUploadResult initiateMultipartUploadResult = ossClient.initiateMultipartUpload(initiateMultipartUploadRequest);
    String uploadId = initiateMultipartUploadResult.getUploadId();
    
    // Start the multipart copy task.
    List<PartETag> partETags = new ArrayList<PartETag>();
    for (int i = 0; i < partCount; i++) {
         // Calculate the size of each part.
        long skipBytes = partSize * i;
        long size = partSize < contentLength - skipBytes ? partSize : contentLength - skipBytes;
    
        // Create an UploadPartCopyRequest object. You can call the UploadPartCopyRequest operation to specify conditions.
        UploadPartCopyRequest uploadPartCopyRequest = 
            new UploadPartCopyRequest(sourceBucketName, sourceObjectName, destinationBucketName, destinationObjectName);
        uploadPartCopyRequest.setUploadId(uploadId);
        uploadPartCopyRequest.setPartSize(size);
        uploadPartCopyRequest.setBeginIndex(skipBytes);
        uploadPartCopyRequest.setPartNumber(i + 1);
    
        UploadPartCopyResult uploadPartCopyResult = ossClient.uploadPartCopy(uploadPartCopyRequest);
        // Store the returned part ETag in partETags.
        partETags.add(uploadPartCopyResult.getPartETag());
    }
    
    // Complete the multipart copy task.
    CompleteMultipartUploadRequest completeMultipartUploadRequest = new CompleteMultipartUploadRequest(
                        destinationBucketName, destinationObjectName, uploadId, partETags);
    CompleteMultipartUploadResult completeMultipartUploadResult = ossClient.completeMultipartUpload(completeMultipartUploadRequest);
    
    // View the tags added to the source object.
    TagSet tagSet = ossClient.getObjectTagging(bucketName, sourceObjectName);
    Map<String, String> getTags = tagSet.getAllTags();
    System.out.println("src object tagging: "+ getTags.toString());
    
    // View the tags added to the destination object.
    tagSet = ossClient.getObjectTagging(bucketName, destinationObjectName);
    getTags = tagSet.getAllTags();
    System.out.println("dest object tagging: "+ getTags.toString());
    
    // Shut down OSSClient.
    ossClient.shutdown();
    ```


## Add tags to a symbolic link object

The following code provides an example on how to add tags to a symbolic link object:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String symLink = "<yourSymLink>";
String destinationObjectName = "<yourDestinationObjectName>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Configure the tags to be added to the symbolic link object.
Map<String, String> tags = new HashMap<String, String>();
tags.put("key1", "value1");
tags.put("key2", "value2");

// Create metadata for the object to upload.
ObjectMetadata metadata = new ObjectMetadata();
metadata.setObjectTagging(tags);

// Create a request to create the symbolic link.
CreateSymlinkRequest createSymlinkRequest = new CreateSymlinkRequest(bucketName, symLink, destinationObjectName);

// Set the object metadata.
createSymlinkRequest.setMetadata(metadata);

// Create a symbolic link.
ossClient.createSymlink(createSymlinkRequest);

// View the tags added to the symbolic link object.
TagSet tagSet = ossClient.getObjectTagging(bucketName, symLink);
Map<String, String> getTags = tagSet.getAllTags();
System.out.println("symLink tagging: "+ getTags.toString());

// Shut down OSSClient.
ossClient.shutdown();
```

