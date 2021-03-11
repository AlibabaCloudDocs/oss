# Versioning

The versioning state of a bucket applies to all objects in the bucket. Versioning allows you to restore objects in a bucket to a previous point in time, and protects your data from being accidentally overwritten or deleted.

A bucket can be in one of the following versioning states: unversioned \(default\), versioning-enabled, or versioning-suspended.

For more information about versioning, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md) in OSS Developer Guide.

## Set the versioning state of a bucket

The following code provides an example on how to set versioning for a bucket to Enabled or Suspended:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you log on with the AccessKey pair of an Alibaba Cloud account because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Set the versioning state of the bucket to Enabled.
BucketVersioningConfiguration configuration = new BucketVersioningConfiguration();
configuration.setStatus(BucketVersioningConfiguration.ENABLED);
SetBucketVersioningRequest request = new SetBucketVersioningRequest(bucketName, configuration);
ossClient.setBucketVersioning(request);

// Shut down the OSSClient instance.
ossClient.shutdown();
                
```

For more information about how to set the versioning state of a bucket, see [PutBucketVersioning](/intl.en-US/API Reference/Bucket operations/Versioning/PutBucketVersioning.md).

## Query the versioning state of a bucket

The following code provides an example on how to obtain the versioning state of a bucket:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you log on with the AccessKey pair of an Alibaba Cloud account because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Query the versioning status of the bucket.
BucketVersioningConfiguration versionConfiguration = ossClient.getBucketVersioning("<yourBucketName>");
System.out.println("bucket versioning status: " + versionConfiguration.getStatus());

// Shut down the OSSClient instance.
ossClient.shutdown();
                
```

For more information about how to query the versioning state of a bucket, see [GetBucketVersioning](/intl.en-US/API Reference/Bucket operations/Versioning/GetBucketVersioning.md).

