# View whether the hierarchical namespace feature is enabled

After the hierarchical namespace feature is enabled for a bucket, you can manage the directory in the bucket, such as creating a directory. This topic describes how to view whether the hierarchical namespace feature is enabled for a bucket.

The following code provides an example on how to view whether thehierarchical namespace feature is enabled for the examplebucket bucket.

```
// Set yourEndpoint to the endpoint of the region where the bucket is deployed. For example, if your bucket is in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com.
String endpoint = "yourEndpoint";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "yourAccessKeyId";
String accessKeySecret = "yourAccessKeySecret";
// Specify the bucket name.
String bucketName = "examplebucket";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Query the bucket information.
BucketInfo info =  ossClient.getBucketInfo(bucketName);
// View whether the value of HnsStatus is Enabled. When the value of HnsStatus is Enabled, the hierarchical namespace feature is enabled for the bucket.
System.out.println("Hnstatus:" + info.getBucket().getHnsStatus());

// Shut down the OSSClient instance.
ossClient.shutdown();
```

