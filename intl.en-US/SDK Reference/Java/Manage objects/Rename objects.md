# Rename objects

After the hierarchical namespace feature is enabled for a bucket, you can rename objects in the bucket. This topic describes how to rename an object.

**Note:** For more information about specific operations related to the hierarchical namespace feature of buckets, see [Create buckets](/intl.en-US/SDK Reference/Java/Buckets/Create buckets.md).

The following code provides an example on how to change the name of exampleobject.txt object in examplebucket to newexampleobject.txt.

```
// Set yourEndpoint to the endpoint of the region where the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com.
String endpoint = "yourEndpoint";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "yourAccessKeyId";
String accessKeySecret = "yourAccessKeySecret";
// Specify the bucket name.
String bucketName = "examplebucket";
// Set the absolute path of the source object. The absolute path of the directory cannot contain the bucket name.
String sourceObject = "exampleobject.txt";
// Set the absolute path of the destination object in the same bucket as that of the source object. The absolute path of the object cannot contain the bucket name.
String destnationObject = "newexampleobject.txt";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Change the absolute path of the source object in the bucket to the absolute path of the destination object.
RenameObjectRequest renameObjectRequest = new RenameObjectRequest(bucketName, sourceObject, destnationObject);
ossClient.renameObject(renameObjectRequest);

// Shut down the OSSClient instance.
ossClient.shutdown();
```

For more information about how to rename objects, see [Rename](/intl.en-US/API Reference/Object operations/Directory management/Rename.md).

