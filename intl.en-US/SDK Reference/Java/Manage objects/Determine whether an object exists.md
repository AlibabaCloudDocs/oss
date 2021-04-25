# Determine whether an object exists

This topic describes how to determine whether an object exists.

The following code provides an example on how to check whether an object named exampleobject.txt exists in a bucket named examplebucket:

```
// Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
String endpoint = "yourEndpoint";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
String accessKeyId = "yourAccessKeyId";
String accessKeySecret = "yourAccessKeySecret";

// Create an OSSClient instance. 
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Check whether the object exists. A value of true returned indicates the specified object exists. Otherwise, the specified object or bucket does not exist. 
// Configure whether to perform redirection-based or mirroring-based back-to-origin. If you set isINoss to true, redirection-based or mirroring-based back-to-origin is not performed. If you set isINoss to false, redirection-based or mirroring-based back-to-origin is performed when the specified object does not exist. 
//boolean isINoss = true;
// Specify the name of the bucket and the full path of the object. The full path of the object cannot contain bucket names. 
boolean found = ossClient.doesObjectExist("examplebucket", "exampleobject.txt");
//boolean found = ossClient.doesObjectExist("examplebucket", "exampleobject.txt", isINoss);
System.out.println(found);

// Shut down the OSSClient instance. 
ossClient.shutdown();        
```

