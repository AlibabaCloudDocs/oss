# Copy objects

OSS allows you to copy objects of any sizes.

## Copy a small-sized object

You can use ossClient.copyObject to copy an object smaller than 1 GB from a bucket \(source bucket\) to another bucket \(target bucket\) in the same region. ossClient.copyObject supports the following configuration methods.

|Configuration method|Description|
|:-------------------|:----------|
|CopyObjectResult copyObject\(String sourceBucketName, String sourceKey, String destinationBucketName, String destinationKey\)|Allows you to specify a source bucket, source object, target bucket, and target object. After the copy operation is completed, the target object content and its Object Meta are the same as those of the source object. This is called simple copy.|
|CopyObjectResult copyObject\(CopyObjectRequest copyObjectRequest\)|Allows you to specify target Object Meta and copy conditions. If the URL of the source bucket is the same as that of the target object, the Object Meta of the source object is replaced with that of the target object.|

The following table describes the parameters you can configure for CopyObjectRequest.

|Parameter|Description|Configuration method|
|:--------|:----------|:-------------------|
|sourceBucketName|Specifies the name of the source bucket.|setSourceBucketName\(String sourceBucketName\)|
|sourceKey|Specifies the name of a source object.|setSourceKey\(String sourceKey\)|
|destinationBucketName|Specifies the name of a target bucket.|setDestinationBucketName\(String destinationBucketName\)|
|destinationKey|Specifies the name of a target object.|setDestinationKey\(String destinationKey\)|
|newObjectMetadata|Specifies the target Object Meta.|setNewObjectMetadata\(ObjectMetadata newObjectMetadata\)|
|matchingETagConstraints|Restrictions on copies. Specifies the conditions for object copy. If ETag of a source object is the same as what OSS calculates, the source object is copied. Otherwise, an error is returned.|setMatchingETagConstraints\(List<String\> matchingETagConstraints\)|
|nonmatchingEtagConstraints|Specifies the conditions for object copy. If ETag of a source object is different from the ETag the user provides, the source object is copied. Otherwise, an error is returned.|setNonmatchingETagConstraints\(List<String\> nonmatchingEtagConstraints\)|
|unmodifiedSinceConstraint|Specifies the conditions for object copy. If the specified time is later than or equal to the time an object is modified, the source object is copied. Otherwise, an error is returned.|setUnmodifiedSinceConstraint\(Date unmodifiedSinceConstraint\)|
|modifiedSinceConstraint|Specifies the conditions for object copy. If a source object is modified after the specified time, the source object is copied. Otherwise, an error is returned.|setModifiedSinceConstraint\(Date modifiedSinceConstraint\)|

The following table describes the parameters you can configure for CopyObjectRequest.

|Parameter|Description|Configuration method|
|:--------|:----------|:-------------------|
|ETag|Specifies the unique identifier of an object.|String getETag\(\)|
|Lastmodified|Specifies the latest time an object is modified.|Date getLastModified\(\)|

**Note:** Object copy rules are as follows:

-   Users have permissions to read and write the source object.
-   Data cannot be copied across regions. For example, you cannot copy an object in a bucket from China East1 \(Hangzhou\) to China North1 \(Qingdao\).
-   The object size is limited to a maximum of 1 GB.

-   Simple copy

    Run the following code for simple copy:

    ```
    // This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    String sourceBucketName = "<yourSourceBucketName>";
    String sourceObjectName = "<yourSourceObjectName>";
    String destinationBucketName = "<yourDestinationBucketName>";
    String destinationObjectName = "<yourDestinationObjectName>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Copy an object.
    CopyObjectResult result = ossClient.copyObject(sourceBucketName, sourceObjectName, destinationBucketName, destinationObjectName);
    System.out.println("ETag: " + result.getETag() + " LastModified: " + result.getLastModified());
    
    // Close your OSSClient.
    ossClient.shutdown();
                        
    ```

-   CopyObjectRequest-based copy

    Run the following code for CopyObjectRequest-based copy:

    ```
    // This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
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
    
    // Configure Object Meta for a new object.
    ObjectMetadata meta = new ObjectMetadata();
    meta.setContentType("text/html");
    copyObjectRequest.setNewObjectMetadata(meta);
    
    // Copy the object.
    CopyObjectResult result = ossClient.copyObject(copyObjectRequest);
    System.out.println("ETag: " + result.getETag() + " LastModified: " + result.getLastModified());
    
    // Close your OSSClient.
    ossClient.shutdown();
                        
    ```


## Copy a large-sized object

To copy an object larger than 1 GB, you need to use UploadPartCopy. In other words, you need to split the object into multiple parts and copy each of them simultaneously. To enable UploadPartCopy, perform the following steps:

1.  Use ossClient.initiateMultipartUpload to initiate an UploadPartCopy task.
2.  Start UploadPartCopy with ossClient.uploadPartCopy. Aside from the last part, all parts must be larger than 100 KB.
3.  Submit the UploadPartCopy task with ossClient.completeMultipartUpload.

Run the following code for UploadPartCopy task.

```
// This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

String sourceBucketName = "<yourSourceBucketName>";
String sourceObjectName = "<yourSourceObjectName>";
String destinationBucketName = "<yourDestinationBucketName>";
String destinationObjectName = "<yourDestinationObjectName>";

ObjectMetadata objectMetadata = ossClient.getObjectMetadata(sourceBucketName, sourceObjectName);
// Obtain the size of the object you want to copy.
long contentLength = objectMetadata.getContentLength();

// Set the minimum size to 10 MB for each part.
long partSize = 1024 * 1024 * 10;

// Calculate the total number of parts.
int partCount = (int) (contentLength / partSize);
if (contentLength % partSize ! = 0) {
    partCount++;
}
System.out.println("total part count:" + partCount);

// Initiate an UploadPartCopy task. You can specify Object Meta for the target object with InitiateMultipartUploadRequest.
InitiateMultipartUploadRequest initiateMultipartUploadRequest = new InitiateMultipartUploadRequest(destinationBucketName, destinationObjectName);
InitiateMultipartUploadResult initiateMultipartUploadResult = ossClient.initiateMultipartUpload(initiateMultipartUploadRequest);
String uploadId = initiateMultipartUploadResult.getUploadId();

// Start the UploadPartCopy task.
List<PartETag> partETags = new ArrayList<PartETag>();
for (int i = 0; i < partCount; i++) {
     // Calculate the size of each part.
    long skipBytes = partSize * i;
    long size = partSize < contentLength - skipBytes ? partSize : contentLength - skipBytes;

    // Create UploadPartCopyRequest. You can specify conditions with UploadPartCopyRequest.
    UploadPartCopyRequest uploadPartCopyRequest = 
        new UploadPartCopyRequest(sourceBucketName, sourceObjectName, destinationBucketName, destinationObjectName);
    uploadPartCopyRequest.setUploadId(uploadId);
    uploadPartCopyRequest.setPartSize(size);
    uploadPartCopyRequest.setBeginIndex(skipBytes);
    uploadPartCopyRequest.setPartNumber(i + 1);
    UploadPartCopyResult uploadPartCopyResult = ossClient.uploadPartCopy(uploadPartCopyRequest);

    // Save the returned ETag of parts to partETags.
    partETags.add(uploadPartCopyResult.getPartETag());
}

// Submit the UploadPartCopy task.
CompleteMultipartUploadRequest completeMultipartUploadRequest = new CompleteMultipartUploadRequest(
                    destinationBucketName, destinationObjectName, uploadId, partETags);
ossClient.completeMultipartUpload(completeMultipartUploadRequest);

// Close your OSSClient.
ossClient.shutdown();
            
```

For more information about UploadPartCopy, see [UploadPartCopy](/intl.en-US/API Reference/Object operations/Multipart upload/UploadPartCopy.md). For the complete code of UploadPartCopy, see [GitHub](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/UploadPartCopySample.java).

