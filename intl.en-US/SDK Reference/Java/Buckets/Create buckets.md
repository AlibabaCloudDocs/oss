# Create buckets

A bucket is a container that stores objects in OSS. Every object is contained in a bucket. This topic describes how to create a bucket.

When you create a bucket, specify whether to enable the hierarchical namespace feature.

-   Create a bucket for which the hierarchical namespace feature is not enabled

    The following code provides an example on how to create a bucket named examplebucket and for which the hierarchical namespace feature is not enabled when you create the bucket.

    You can specify the storage class and access control list \(ACL\) when you create a bucket. For more information, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md) and [Set the ACL for a bucket](/intl.en-US/Developer Guide/Buckets/Set the ACL for a bucket.md).

    ```
    // Set yourEndpoint to the endpoint of the region where the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com.
    String endpoint = "yourEndpoint";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "yourAccessKeyId";
    String accessKeySecret = "yourAccessKeySecret";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Create a CreateBucketRequest object.
    CreateBucketRequest createBucketRequest = new CreateBucketRequest("examplebucket");
    
    // The following code provides an example on how to specify the storage class, ACL, and the type of disaster recovery when you create a bucket:
    // In this example, the storage class of the bucket is Standard.
    /createBucketRequest.setStorageClass(StorageClass.Standard);
    // By default, the type of disaster recovery is DataRedundancyType.LRS, which indicates locally redundant storage (LRS). To set the type of redundant storage to zone-redundant storage, specify DataRedundancyType.ZRS.
    //createBucketRequest.setDataRedundancyType(DataRedundancyType.ZRS);
    // Set the ACL of the bucket to public read. The default ACL is private.
    //createBucketRequest.setCannedACL(CannedAccessControlList.PublicRead);
    
    // Create a bucket.
    ossClient.createBucket(createBucketRequest);
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
    ```

-   Create a bucket for which the hierarchical namespace feature is enabled

    After hierarchical namespace is enabled, you can manage the directory in the bucket, such as creating a directory. For more information about the hierarchical namespace feature, see [Hierarchical namespace]().

    The following code provides an example on how to create a bucket named examplebucket and for which the hierarchical namespace feature is enabled when you create the bucket.

    ```
    // Set yourEndpoint to the endpoint of the region where the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com.
    String endpoint = "yourEndpoint";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "yourAccessKeyId";
    String accessKeySecret = "yourAccessKeySecret";
    // Specify the bucket name.
    String bucketName = "examplebucket";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Create a bucket and enable the hierarchical namespace feature.
    CreateBucketRequest request = new CreateBucketRequest(bucketName).withHnsStatus(HnsStatus.Enabled);
    ossClient.createBucket(request);
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
    ```


For more information about the bucket naming conventions, see [bucket](/intl.en-US/Developer Guide/Terms.md).

