# Copy objects

This topic describes how to copy an object from a source bucket to a destination bucket within the same region. The source bucket and the destination bucket can be the same or different buckets.

## Usage notes

When you copy objects, take note of the following items:

-   You must have read and write permissions on the source object.
-   The source bucket and destination bucket must be in the same region. For example, objects in a bucket located in the China \(Hangzhou\) region cannot be copied to another bucket located in the China \(Qingdao\) region.

## Copy a small object

You can use the ossClient.copyObject method to copy an object smaller than 1GB. The following table describes the configuration methods of ossClient.copyObject.

|Configuration method|Description|
|:-------------------|:----------|
|CopyObjectResult copyObject\(String sourceBucketName, String sourceKey, String destinationBucketName, String destinationKey\)|Allows you to specify the source bucket, source object, destination bucket, and destination object. After an object is copied, the content and metadata of the destination object are the same as that of the source object. This method is called simple copy.|
|CopyObjectResult copyObject\(CopyObjectRequest copyObjectRequest\)|Allows you to specify metadata of the destination object and copy conditions. If the URLs of the source object and destination object for the copy operation are the same, the metadata of the source object replaces the metadata of the destination object.|

The following table describes the parameters you can configure for CopyObjectRequest.

|Parameter|Description|Method|
|:--------|:----------|:-----|
|sourceBucketName|The name of the source bucket.|setSourceBucketName\(String sourceBucketName\)|
|sourceKey|The name of the source object.|setSourceKey\(String sourceKey\)|
|destinationBucketName|The name of the destination bucket.|setDestinationBucketName\(String destinationBucketName\)|
|destinationKey|The name of the destination object.|setDestinationKey\(String destinationKey\)|
|newObjectMetadata|The metadata of the destination object.|setNewObjectMetadata\(ObjectMetadata newObjectMetadata\)|
|matchingETagConstraints|The condition for the copy operation. If the ETag value of the source object is the same as the ETag value provided by the user, the copy operation is performed. Otherwise, an error is returned.|setMatchingETagConstraints\(List<String\> matchingETagConstraints\)|
|nonmatchingEtagConstraints|The condition for the copy operation. If the ETag value of the source object is different from the ETag value provided by the user, the copy operation is performed. Otherwise, an error is returned.|setNonmatchingETagConstraints\(List<String\> nonmatchingEtagConstraints\)|
|unmodifiedSinceConstraint|The condition for the copy operation. If the specified time is equal to or later than the actual modified time of the source object, the copy operation is performed. Otherwise, an error is returned.|setUnmodifiedSinceConstraint\(Date unmodifiedSinceConstraint\)|
|modifiedSinceConstraint|The condition for the copy operation. If the source object is modified after the specified time, the copy operation is performed. Otherwise, an error is returned.|setModifiedSinceConstraint\(Date modifiedSinceConstraint\)|

The following table describes the parameters you can configure for CopyObjectRequest.

|Parameter|Description|Method|
|:--------|:----------|:-----|
|etag|The unique tag of the object.|String getETag\(\)|
|lastModified|The last modified time of the object.|Date getLastModified\(\)|

The following methods can be used to copy small objects.

-   Simple copy

    The following code provides an example on how to use simple copy to copy an object named srcexampleobject.txt from the srcexamplebucket bucket to the desexampleobject.txt object in the desexamplebucket bucket.

    ```
    // Set yourEndpoint to the endpoint of the region where the bucket is located. For example, if your bucket is in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com.
    String endpoint = "yourEndpoint";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "yourAccessKeyId";
    String accessKeySecret = "yourAccessKeySecret";
    
    // Specify the name of the source bucket.
    String sourceBucketName = "srcexamplebucket";
    // Specify the complete path of the source object, which does not include the bucket name.
    String sourceKey = "srcexampleobject.txt";
    // Specify the name of the destination bucket. The destination bucket must be in the same region as the source bucket.
    String destinationBucketName = "desexamplebucket";
    // Specify the complete path of the destination object, which does not include the bucket name.
    String destinationKey = "desexampleobject.txt";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Copy the object.
    CopyObjectResult result = ossClient.copyObject(sourceBucketName, sourceKey, destinationBucketName, destinationKey);
    System.out.println("ETag: " + result.getETag() + " LastModified: " + result.getLastModified());
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();                   
    ```

-   Use CopyObjectRequest

    The following code provides an example on how to use CopyObjectRequest to copy an object named srcexampleobject.txt from the srcexamplebucket bucket to an object named desexampleobject.txt in the destination bucket named desexamplebucket:

    ```
    // Set yourEndpoint to the endpoint of the region where the bucket is located. For example, if your bucket is in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com.
    String endpoint = "yourEndpoint";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "yourAccessKeyId";
    String accessKeySecret = "yourAccessKeySecret";
    // Specify the name of the source bucket.
    String sourceBucketName = "srcexamplebucket";
    // Specify the complete path of the source object, which does not include the bucket name.
    String sourceKey = "srcexampleobject.txt";
    // Specify the name of the destination bucket. The destination bucket must be in the same region as the source bucket.
    String destinationBucketName = "desexamplebucket";
    // Specify the complete path of the destination object, which does not include the bucket name.
    String destinationKey = "desexampleobject.txt";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Create CopyObjectRequest.
    CopyObjectRequest copyObjectRequest = new CopyObjectRequest(sourceBucketName, sourceKey, destinationBucketName, destinationKey);
    
    // Configure new object metadata.
    ObjectMetadata meta = new ObjectMetadata();
    meta.setContentType("text/txt");
    copyObjectRequest.setNewObjectMetadata(meta);
    
    // Copy the object.
    CopyObjectResult result = ossClient.copyObject(copyObjectRequest);
    System.out.println("ETag: " + result.getETag() + " LastModified: " + result.getLastModified());
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();                   
    ```


## Copy a large object

To copy an object larger than 1 GB, you must split the object into smaller parts and copy them sequentially by using UploadPartCopy. To implement multipart copy, perform the following operations:

1.  Use ossClient.initiateMultipartUpload to initiate a multipart copy task.
2.  Use ossClient.uploadPartCopy to perform the multipart copy task. All parts, except for the last part, must be larger than 100 KB.
3.  Use ossClient.completeMultipartUpload to complete the multipart copy task.

For the complete code that is used to perform multipart copy, visit [GitHub](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/UploadPartCopySample.java).

The following code provides an example on how to use multipart copy to copy an object named srcexampleobject.txt from the srcexamplebucket bucket to the desexampleobject.txt object in the desexamplebucket bucket.

```
// Set yourEndpoint to the endpoint of the region where the bucket is located. For example, if your bucket is in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com.
String endpoint = "yourEndpoint";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "yourAccessKeyId";
String accessKeySecret = "yourAccessKeySecret";
// Specify the name of the source bucket.
String sourceBucketName = "srcexamplebucket";
// Specify the complete path of the source object, which does not include the bucket name.
String sourceKey = "srcexampleobject.txt";
// Specify the name of the destination bucket. The destination bucket must be in the same region as the source bucket.
String destinationBucketName = "desexamplebucket";
// Specify the complete path of the destination object, which does not include the bucket name.
String destinationKey = "desexampleobject.txt";

ObjectMetadata objectMetadata = ossClient.getObjectMetadata(sourceBucketName, sourceKey);
// Query the size of the object to copy.
long contentLength = objectMetadata.getContentLength();

// Set the size of each part to 10 MB. Unit: bytes.
long partSize = 1024 * 1024 * 10;

// Calculate the total number of parts.
int partCount = (int) (contentLength / partSize);
if (contentLength % partSize != 0) {
    partCount++;
}
System.out.println("total part count:" + partCount);

// Initiate a multipart copy task. You can call the InitiateMultipartUploadRequest operation to specify the metadata of the destination object.
InitiateMultipartUploadRequest initiateMultipartUploadRequest = new InitiateMultipartUploadRequest(destinationBucketName, destinationKey);
InitiateMultipartUploadResult initiateMultipartUploadResult = ossClient.initiateMultipartUpload(initiateMultipartUploadRequest);
String uploadId = initiateMultipartUploadResult.getUploadId();

// Start the multipart copy task.
List<PartETag> partETags = new ArrayList<PartETag>();
for (int i = 0; i < partCount; i++) {
     // Calculate the size of each part.
    long skipBytes = partSize * i;
    long size = partSize < contentLength - skipBytes ? partSize : contentLength - skipBytes;

    // Create an UploadPartCopyRequest request. You can call the UploadPartCopyRequest operation to specify conditions.
    UploadPartCopyRequest uploadPartCopyRequest = 
        new UploadPartCopyRequest(sourceBucketName, sourceKey, destinationBucketName, destinationKey);
    uploadPartCopyRequest.setUploadId(uploadId);
    uploadPartCopyRequest.setPartSize(size);
    uploadPartCopyRequest.setBeginIndex(skipBytes);
    uploadPartCopyRequest.setPartNumber(i + 1);
    UploadPartCopyResult uploadPartCopyResult = ossClient.uploadPartCopy(uploadPartCopyRequest);

    // Store the returned part ETags in partETags.
    partETags.add(uploadPartCopyResult.getPartETag());
}

// Complete the multipart copy task.
CompleteMultipartUploadRequest completeMultipartUploadRequest = new CompleteMultipartUploadRequest(
                    destinationBucketName, destinationKey, uploadId, partETags);
ossClient.completeMultipartUpload(completeMultipartUploadRequest);

// Shut down the OSSClient instance.
ossClient.shutdown();           
```

For more information about multipart copy, see [UploadPartCopy](/intl.en-US/API Reference/Object operations/Multipart upload/UploadPartCopy.md).

