# Upload objects

This topic describes how to upload objects to a bucket with versioning enabled or suspended.

## Simple upload

When you upload an object to a bucket with versioning enabled, OSS automatically adds a unique version ID for the uploaded object, and returns the version ID as the x-oss-version-id field in the response header. In a bucket with versioning suspended, the version ID added to an upload object is null. If you upload an object with the same name as an existing object to a bucket with versioning suspended, the existing object is overwritten to ensure that each object only has one version of which the ID is null.

You can run the following code to simply upload an object:

```
// This example uses the China East 1 (Hangzhou) endpoint. Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Creates an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// In this example, a string is uploaded.
String content = "Hello OSS";
PutObjectResult result = ossClient.putObject("<yourBucketName>", "<yourObjectName>", new ByteArrayInputStream(content.getBytes()));
// Views the version ID of the uploaded object.
System.out.println("result.versionid: " + result.getVersionId());

// Closes the OSSClient instance.
ossClient.shutdown();
                
```

For more information about simple upload, see [PutObject](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.md).

## Append upload

In a bucket with versioning enabled or suspended, the AppendObject operation can be performed only on objects of which the current version is an appendable object.

**Note:**

-   When you perform the AppendObject operation on an object of which the current version is an appendable object, OSS does not generate a historical version for the appendable object.
-   When you perform the PutObject or DeleteObject operation on an object of which the current version is an appendable object, OSS stores the appendable object as a historical version and prevents the object from being further appended.
-   You cannot perform the AppendObject operation on objects of which the current version is not an appendable object \(such as a normal object or a delete marker\).
-   If versioning is enabled or suspended for a bucket, you cannot perform the CopyObject operation on appendable objects in the bucket.

You can run the following code to append files to an existing object:

```
// This example uses the China East 1 (Hangzhou) endpoint. Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

String content1 = "Hello OSS A \n";
String content2 = "Hello OSS B \n";
String content3 = "Hello OSS C \n";

// Creates an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

ObjectMetadata meta = new ObjectMetadata();
// Specifies the content type of file to be uploaded.
meta.setContentType("text/plain");

// Calls AppendObjectRequest to set multiple parameters.
AppendObjectRequest appendObjectRequest = new AppendObjectRequest("<yourBucketName>", "<yourObjectName>", new ByteArrayInputStream(content1.getBytes()),meta);

// Calls AppendObjectRequest to set single parameters.
// Sets the bucket name.
//appendObjectRequest.setBucketName("<yourBucketName>");
// Sets the object name.
//appendObjectRequest.setKey("<yourObjectName>");
// Sets the type of the content to be appended.Two types are available: InputStream and File. The type is set to InputStream here.
//appendObjectRequest.setInputStream(new ByteArrayInputStream(content1.getBytes()));
// Sets the type of the content to be appended.Two types are available: InputStream and File. The type is set to File here.
//appendObjectRequest.setFile(new File("<yourLocalFile>"));
// Sets the metadata of the object. You can set it only when you append contents to the object for the first time.
//appendObjectRequest.setMetadata(meta);

// Performs the first append operation.
// Sets the position from where the content is appended.
appendObjectRequest.setPosition(0L);
AppendObjectResult appendObjectResult = ossClient.appendObject(appendObjectRequest);
// Prints the CRC64 value of the object, which is calculated based on the ECMA-182 standard.
System.out.println(appendObjectResult.getObjectCRC());

// Performs the second append operation.
// nextPosition indicates the position that should be provided in the next request. Its value is the current size of the object.
appendObjectRequest.setPosition(appendObjectResult.getNextPosition());
appendObjectRequest.setInputStream(new ByteArrayInputStream(content2.getBytes()));
appendObjectResult = ossClient.appendObject(appendObjectRequest);

// Performs the third append operation.
appendObjectRequest.setPosition(appendObjectResult.getNextPosition());
appendObjectRequest.setInputStream(new ByteArrayInputStream(content3.getBytes()));
appendObjectResult = ossClient.appendObject(appendObjectRequest);

// Closes the OSSClient instance.
ossClient.shutdown();
```

For more information about append upload, see [AppendObject](/intl.en-US/API Reference/Object operations/Basic operations/AppendObject.md).

## Multipart upload

If you call CompleteMultipartUpload to complete the MultipartUpload task for an object in a bucket with versioning enabled or suspended, OSS generates a unique version ID for the object and returns it as the x-oss-version-id field in the response header.

You can run the following code to upload an object in the multipart upload method:

```
// This example uses the China East 1 (Hangzhou) endpoint. Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String objectName = "<yourObjectName>";

// Creates an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

/* Step 1: Initializes a multipart upload event.
*/
InitiateMultipartUploadRequest request = new InitiateMultipartUploadRequest(bucketName, objectName);
InitiateMultipartUploadResult upresult = ossClient.initiateMultipartUpload(request);
// Returns the uploadId. It is a unique identifier for a multipart upload event. You can perform operations based on this ID, for example, cancel or search a multipart upload event.
String uploadId = upresult.getUploadId();

/* Step 2: Uploads the parts.
*/
// partETags is a set of PartEtags. A PartETag composes of the ETag of a part and the part number.
List<PartETag> partETags =  new ArrayList<PartETag>();
// Calculates the number of parts included in the object.
final long partSize = 1 * 1024 * 1024L;   // 1MB
final File sampleFile = new File("<localFile>");
long fileLength = sampleFile.length();
int partCount = (int) (fileLength / partSize);
if (fileLength % partSize != 0) {
    partCount++;
 }
// Uploads the parts one by one.
for (int i = 0; i < partCount; i++) {
    long startPos = i * partSize;
    long curPartSize = (i + 1 == partCount) ? (fileLength - startPos) : partSize;
    InputStream instream = new FileInputStream(sampleFile);
    // Skips the uploaded parts.
    instream.skip(startPos);
    UploadPartRequest uploadPartRequest = new UploadPartRequest();
    uploadPartRequest.setBucketName(bucketName);
    uploadPartRequest.setKey(objectName);
    uploadPartRequest.setUploadId(uploadId);
    uploadPartRequest.setInputStream(instream);
    // Sets the part size. The minimum size for a part (except for the last part) is 100 KB.
    uploadPartRequest.setPartSize(curPartSize);
    // Sets the part number. Each part has a number ranging from 1 to 10000. If a part number is not in this range, OSS returns the InvalidArgument error.
    uploadPartRequest.setPartNumber( i + 1);
  // Parts can be uploaded out of order or even uploaded in differnt clients. OSS integrates the uploaded parts into a complete object based on the part number.
    UploadPartResult uploadPartResult = ossClient.uploadPart(uploadPartRequest);
  // OSS returns a PartETag each time after a part is uploaded. The returned PartETags are stored in partEtag.
    partETags.add(uploadPartResult.getPartETag());
}

/* Step 3: Completes the multipart upload task.
*/
// To complete a multipart upload task, valid partETags must be provided. After receiving the partETags, OSS verifies each part and then integrates the parts into a complete object after all the parts are verified.
CompleteMultipartUploadRequest completeMultipartUploadRequest =
        new CompleteMultipartUploadRequest(bucketName, objectName, uploadId, partETags);

CompleteMultipartUploadResult completeMultipartUploadResult = ossClient.completeMultipartUpload(completeMultipartUploadRequest);
// Views the version ID of the uploaded object.
System.out.println("restore object versionid: " + completeMultipartUploadResult.getVersionId());

// Closes the OSSClient instance.
ossClient.shutdown();
                
```

For more information about multipart upload, see [CompleteMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/CompleteMultipartUpload.md).

