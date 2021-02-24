# Manage object ACLs

This topic describes how to manage the access control list \(ACL\) of an object.

The following table describes the ACLs that you can configure for an object.

|Permission|Description|Value|
|:---------|:----------|:----|
|Inherited from bucket|The ACL of an object is the same as that of the bucket in which the object is stored.|CannedAccessControlList.Default|
|Private|Only the object owner or authorized users can read and write the object.|CannedAccessControlList.Private|
|Public read|Only the object owner or authorized users can write the object. Other users, including anonymous users can only read the object. Exercise caution when you configure this ACL.|CannedAccessControlList.PublicRead|
|Public read/write|Any users, including anonymous users can read and write the object. Exercise caution when you configure this operation.|CannedAccessControlList.PublicReadWrite|

The ACL of an object takes precedence over the ACL of the bucket in which the object is stored. For example, if the ACL of a bucket is private and the ACL of an object that is stored in this bucket is public, all users can read and write the object. If the ACL of an object is not configured, the ACL of the object is the same as that of the bucket in which the object is stored.

## Configure the ACL of an object

The following code provides an example on how to configure the ACL of an object:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Configure the ACL of the specified object to public read.
ossClient.setObjectAcl("<yourBucketName>", "<yourObjectName>", CannedAccessControlList.PublicRead);

// Shut down the OSSClient instance.
ossClient.shutdown();
			
```

For more information, see [PutObjectACL](/intl.en-US/API Reference/Object operations/ACL/PutObjectACL.md).

## Query the ACL of an object

The following code provides an example on how to query the ACL of a specified object:

```
// The China (Hangzhou) region is used in this example as the endpoint. Specify the actual endpoint based on your requirements
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you log on with the AccessKey pair of an Alibaba Cloud account because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

// Query the ACL of the object.
ObjectAcl objectAcl = ossClient.getObjectAcl("<yourBucketName>", "<yourObjectName>");
System.out.println(objectAcl.getPermission().toString());

// Shut down the OSSClient instance.
ossClient.shutdown();
			
```

For more information, see [GetObjectACL](/intl.en-US/API Reference/Object operations/ACL/GetObjectACL.md).

