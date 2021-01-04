# Versioning

The versioning state of a bucket applies to all of the objects in the bucket. Versioning allows you to restore objects in a bucket to any previous point in time, and protects your data from being accidentally overwritten or deleted.

A bucket can be in any one of the following versioning states: unversioned \(default\), versioning-enabled, or versioning-suspended.

For more information about versioning, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md) in OSS Developer Guide.

## Configure versioning for a bucket

The following code provides an example on how to configure versioning for a bucket to Enabled or Suspended:

```
// This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create your RAM user, log on to https://ram.console.aliyun.com.
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

For more information about how to configure versioning for a bucket, see [PutBucketVersioning](/intl.en-US/API Reference/Bucket operations/Versioning/PutBucketVersioning.md).

## Obtain the versioning state of a bucket

The following code provides an example on how to obtain the versioning state of a bucket:

```
// This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create your RAM user, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Obtain the versioning state of the bucket.
BucketVersioningConfiguration versionConfiguration = ossClient.getBucketVersioning("<yourBucketName>");
System.out.println("bucket versioning status: " + versionConfiguration.getStatus());

// Shut down the OSSClient instance.
ossClient.shutdown();
                
```

For more information about how to obtain the versioning state of a bucket, see [GetBucketVersioning](/intl.en-US/API Reference/Bucket operations/Versioning/GetBucketVersioning.md).

