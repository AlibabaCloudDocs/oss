# Determine whether buckets exist

A bucket is a container for objects stored in OSS. Every object is contained in a bucket. This topic describes how to determine whether a bucket exists.

The following code provides an example on how to determine whether a bucket named examplebucket exists:

```
// Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com.
String endpoint = "yourEndpoint";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "yourAccessKeyId";
String accessKeySecret = "yourAccessKeySecret";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Determine whether the bucket named examplebucket exists. If the value of true is returned, the bucket exists. Otherwise, the bucket does not exist.
boolean exists = ossClient.doesBucketExist("examplebucket");
System.out.println(exists);

// Shut down the OSSClient instance.
ossClient.shutdown();            
```

