# Download to files

This topic describes how to download objects from a bucket to a file.

The following code provides an example on how to download exampleobject.txt from the testfolder directory in examplebucket to examplefile.txt in the D:\\localpath path.

```
// Set yourEndpoint to the endpoint of the region where the bucket is located. For example, if your bucket is in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com.
String endpoint = "yourEndpoint";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "yourAccessKeyId";
String accessKeySecret = "yourAccessKeySecret";
// Specify the bucket name.
String bucketName = "examplebucket";
// Specify the complete path of the object excluding the bucket name. Example: testfolder/exampleobject.txt.
String objectName = "testfolder/exampleobject.txt";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Download the object to a file, and save it to a specified path. If the specified file exists, the downloaded object replaces the file. Otherwise, the file is created.
// By default, if the path is not specified, the downloaded file is saved to the path of the project to which the sample program belongs.
ossClient.getObject(new GetObjectRequest(bucketName, objectName), new File("D:\\localpath\\examplefile.txt"));

// Shut down the OSSClient instance.
ossClient.shutdown();     
```

