# Disable overwrite for objects with the same name

By default, if you upload an object that has the same name as an existing object, the existing object is overwritten by the uploaded object. This topic describes how to set the x-oss-forbid-overwrite request header to disable overwrite for objects with the same name when you copy objects or use the simple upload and multipart upload methods.

## Simple upload

The following code provides an example on how to disable overwrite for an object with the same name when you use simple upload:

```
// This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String objectName = "<yourObjectName>";
String content = "Hello OSS!" ;

OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Create a PutObjectRequest object.
PutObjectRequest putObjectRequest = new PutObjectRequest(bucketName, objectName, new ByteArrayInputStream(content.getBytes()));

// Specify whether to overwrite the object with the same name.
// By default, if x-oss-forbid-overwrite is not specified, the object with the same name is overwritten.
// If x-oss-forbid-overwrite is set to false, the object with the same name is overwritten.
// If x-oss-forbid-overwrite is set to true, the object with the same name is not overwritten. If an object with the same name already exists, an error is reported.
ObjectMetadata metadata = new ObjectMetadata();
metadata.setHeader("x-oss-forbid-overwrite", "true");
putObjectRequest.setMetadata(metadata);

// Upload the object.
ossClient.putObject(putObjectRequest);

// Shut down the OSSClient instance.
ossClient.shutdown();
```

For more information about simple upload, see [PutObject](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.md).

## Copy objects

-   Copy a small object

    The following code provides an example on how to disable overwrite for the object with the same name when you copy a small object by calling the CopyObjectRequest operation:

    ```
    // This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to https://ram.console.aliyun.com.
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
    
    // Configure new object metadata.
    ObjectMetadata metadata = new ObjectMetadata();
    metadata.setContentType("text/html");
    // Specify whether to overwrite the object with the same name.
    // By default, if x-oss-forbid-overwrite is not specified, the object with the same name is overwritten.
    // If x-oss-forbid-overwrite is set to false, the object with the same name is overwritten.
    // If x-oss-forbid-overwrite is set to true, the object with the same name is not overwritten. If an object with the same name already exists, an error is reported.
    metadata.setHeader("x-oss-forbid-overwrite", "true");
    copyObjectRequest.setNewObjectMetadata(metadata);
    
    // Copy the object.
    CopyObjectResult result = ossClient.copyObject(copyObjectRequest);
    System.out.println("ETag: " + result.getETag() + " LastModified: " + result.getLastModified());
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
    ```

-   Copy a large object

    The following code provides an example on how to disable overwrite for the object with the same name when you copy a large object by using multipart copy:

    ```
    // This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to https://ram.console.aliyun.com.
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
    
    // Initiate a multipart copy task. You can call the InitiateMultipartUploadRequest operation to specify the metadata of the target object.
    InitiateMultipartUploadRequest initiateMultipartUploadRequest = new InitiateMultipartUploadRequest(destinationBucketName, destinationObjectName);
    // Specify whether to overwrite the object with the same name.
    // By default, if x-oss-forbid-overwrite is not specified, the object with the same name is overwritten.
    // If x-oss-forbid-overwrite is set to false, the object with the same name is overwritten.
    // If x-oss-forbid-overwrite is set to true, the object with the same name is not overwritten. If an object with the same name already exists, an error is reported.
    ObjectMetadata metadata = new ObjectMetadata();
    metadata.setHeader("x-oss-forbid-overwrite", "true");
    initiateMultipartUploadRequest.setObjectMetadata(metadata);
    
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
    
    // Specify whether to overwrite the object with the same name.
    // By default, if x-oss-forbid-overwrite is not specified, the object with the same name is overwritten.
    // If x-oss-forbid-overwrite is set to false, the object with the same name is overwritten.
    // If x-oss-forbid-overwrite is set to true, the object with the same name is not overwritten. If an object with the same name already exists, an error is reported.
    completeMultipartUploadRequest.addHeader("x-oss-forbid-overwrite", "true");
    
    ossClient.completeMultipartUpload(completeMultipartUploadRequest);
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
    ```


For more information about how to copy an object, see [CopyObject](/intl.en-US/API Reference/Object operations/Basic operations/CopyObject.md).

## Multipart upload

The following code provides an example on how to disable overwrite for the object with the same name when you use multipart upload:

```
// This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String objectName = "<yourObjectName>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Create an InitiateMultipartUploadRequest object.
InitiateMultipartUploadRequest request = new InitiateMultipartUploadRequest(bucketName, objectName);

// Specify whether to overwrite the object with the same name.
// By default, if x-oss-forbid-overwrite is not specified, the object with the same name is overwritten.
// If x-oss-forbid-overwrite is set to false, the object with the same name is overwritten.
// If x-oss-forbid-overwrite is set to true, the object with the same name is not overwritten. If an object with the same name already exists, an error is reported.
ObjectMetadata metadata = new ObjectMetadata();
metadata.setHeader("x-oss-forbid-overwrite", "true");
request.setObjectMetadata(metadata);

// Initiate a multipart upload task.
InitiateMultipartUploadResult upresult = ossClient.initiateMultipartUpload(request);
// Obtain an upload ID. The upload ID is the unique identification of a multipart upload task. You can initiate related requests based on the upload ID, such as canceling a multipart upload and querying a multipart upload.
String uploadId = upresult.getUploadId();

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
// Upload each part until all parts are uploaded.
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


// Create a CompleteMultipartUploadRequest object.
// When you have completed multipart upload, you must provide all valid partETags. After receiving the partETags, OSS verifies the validity of all parts one by one. After part verification is complete, OSS combines these parts into a complete object.
CompleteMultipartUploadRequest completeMultipartUploadRequest =
        new CompleteMultipartUploadRequest(bucketName, objectName, uploadId, partETags);

// Specify whether to overwrite the object with the same name.
// By default, if x-oss-forbid-overwrite is not specified, the object with the same name is overwritten.
// If x-oss-forbid-overwrite is set to false, the object with the same name is overwritten.
// If x-oss-forbid-overwrite is set to true, the object with the same name is not overwritten. If an object with the same name already exists, an error is reported.
completeMultipartUploadRequest.addHeader("x-oss-forbid-overwrite", "true");

// Complete multipart upload.
CompleteMultipartUploadResult completeMultipartUploadResult = ossClient.completeMultipartUpload(completeMultipartUploadRequest);

// Shut down the OSSClient instance.
ossClient.shutdown();
```

For more information about multipart upload, see [InitiateMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/InitiateMultipartUpload.md) and [CompleteMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/CompleteMultipartUpload.md).

