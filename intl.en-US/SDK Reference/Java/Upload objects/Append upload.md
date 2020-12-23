# Append upload

You can use AppendObject to append content to append objects that are uploaded.

## Usage notes

When you call AppendObject to upload an object, take note of the following items:

-   If the object to append does not exist, an append object is created.
-   If the object to append is an existing append object and the specified position from which the append operation starts is not equal to the current object size, the PositionNotEqualToLength error is returned. If the object to append is a non-append object, the ObjectNotAppendable error is returned.
-   The CopyObject operation cannot be performed on append objects.

For more information about how to perform append upload, see [AppendObject](/intl.en-US/API Reference/Object operations/Basic operations/AppendObject.md). For the complete code of append upload, visit [GitHub](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/AppendObjectSample.java).

## Sample codes

The following code provides an example on how to perform append upload:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

String content1 = "Hello OSS A \n";
String content2 = "Hello OSS B \n";
String content3 = "Hello OSS C \n";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

ObjectMetadata meta = new ObjectMetadata();
// Specify the type of content you want to upload.
meta.setContentType("text/plain");

// Configure multiple parameters by using AppendObjectRequest.
AppendObjectRequest appendObjectRequest = new AppendObjectRequest("<yourBucketName>", "<yourObjectName>", new ByteArrayInputStream(content1.getBytes()),meta);

// Configure a single parameter by using AppendObjectRequest.
// Configure the bucket name.
//appendObjectRequest.setBucketName("<yourBucketName>");
// Configure the object name. The object name indicates the complete path of the object excluding the bucket name. Example: example/test.txt.
//appendObjectRequest.setKey("<yourObjectName>");
// Specify the content you want to append. Two types of content are available: InputStream and File. The following code provides an example on how to specify the content of the InputStream type.
//appendObjectRequest.setInputStream(new ByteArrayInputStream(content1.getBytes()));
// Specify the content you want to append. Two types of content are available: InputStream and File. The following code provides an example on how to specify the content of the File type.
//appendObjectRequest.setFile(new File("<yourLocalFile>"));
// Specify the object metadata. You can specify the metadata of an object only when you perform the append operation on the object for the first time.
//appendObjectRequest.setMetadata(meta);

// Perform the first append operation.
// Configure the position from which the append operation starts.
appendObjectRequest.setPosition(0L);
AppendObjectResult appendObjectResult = ossClient.appendObject(appendObjectRequest);
// Calculate the 64-bit CRC value of the object. The value is calculated based on the ECMA-182 standard.
System.out.println(appendObjectResult.getObjectCRC());

// Perform the second append operation.
// nextPosition indicates the position from which the next append operation starts, which is the current length of the object.
appendObjectRequest.setPosition(appendObjectResult.getNextPosition());
appendObjectRequest.setInputStream(new ByteArrayInputStream(content2.getBytes()));
appendObjectResult = ossClient.appendObject(appendObjectRequest);

// Perform the third append operation.
appendObjectRequest.setPosition(appendObjectResult.getNextPosition());
appendObjectRequest.setInputStream(new ByteArrayInputStream(content3.getBytes()));
appendObjectResult = ossClient.appendObject(appendObjectRequest);

// Shut down the OSSClient instance.
ossClient.shutdown();
        
```

For more information about the naming conventions for buckets, see [bucket](/intl.en-US/Developer Guide/Terms.md). For more information about the naming conventions for objects, see [object](/intl.en-US/Developer Guide/Terms.md).

