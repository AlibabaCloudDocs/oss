# Transfer acceleration

The transfer acceleration feature allows users worldwide to access objects stored in Object Storage Service \(OSS\) more quickly. This feature is applicable to scenarios where data must be transferred over long geographical distances. This feature can also be used to download or upload large objects that are gigabytes or terabytes in size.

For more information about transfer acceleration, see [Transfer acceleration](https://icms.alibaba-inc.com/content/oss/f3b55d?l=1&m=151&n=1436619) in OSS Developer Guide.

## Enable transfer acceleration

The following code provides an example on how to enable transfer acceleration for a bucket named examplebucket:

```
// Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if the bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
String endpoint = "yourEndpoint";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to access OSS because the account has permissions on all API operations. We recommend that you use a Resource Access Management (RAM) user to call API operations or perform routine O&M. To create a RAM user, log on to the RAM console. 
String accessKeyId = "yourAccessKeyId";
String accessKeySecret = "yourAccessKeySecret";
// Specify the name of the bucket. 
String bucketName = "examplebucket";

// Create an OSSClient instance. 
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Configure transfer acceleration for the bucket. 
// If enabled is set to true, transfer acceleration is enabled. If enabled is set to false, transfer acceleration is disabled. 
boolean enabled = true;
ossClient.setBucketTransferAcceleration(bucketName, enabled);

// Shut down the OSSClient instance. 
ossClient.shutdown();
```

For more information about how to enable the transfer acceleration feature, see [PutBucketTransferAcceleration](/intl.en-US/API Reference/Bucket operations/Transfer acceleration/PutBucketTransferAcceleration.md) in OSS API Reference.

## Query the status of transfer acceleration

The following code provides an example on how to query the transfer acceleration status of a bucket named examplebucket:

```
// Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if the bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
String endpoint = "yourEndpoint";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to access OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine O&M. To create a RAM user, log on to the RAM console. 
String accessKeyId = "yourAccessKeyId";
String accessKeySecret = "yourAccessKeySecret";
// Specify the name of the bucket. 
String bucketName = "examplebucket";

// Create an OSSClient instance. 
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Query the transfer acceleration status of the bucket. 
// If the returned value is true, the transfer acceleration feature is enabled for the bucket. If the returned value is false, the transfer acceleration feature is disabled for the bucket. 
TransferAcceleration  result = ossClient.getBucketTransferAcceleration(bucketName);
System.out.println("Is transfer acceleration enabled:"+ result.isEnabled());

// Shut down the OSSClient instance. 
ossClient.shutdown();
```

For more information about the operation called to query the transfer acceleration status of a bucket, see [GetBucketTransferAcceleration](/intl.en-US/API Reference/Bucket operations/Transfer acceleration/GetBucketTransferAcceleration.md) in OSS API Reference.

