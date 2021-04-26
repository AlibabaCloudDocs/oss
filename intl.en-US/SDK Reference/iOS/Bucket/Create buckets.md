# Create buckets

A bucket is a container for objects stored in OSS. Every object is contained in a bucket. This topic describes how to create a bucket.

## Background information

When you create a bucket, take note of the following items:

-   You can use an Alibaba Cloud account to create up to 100 buckets in the same region.
-   Specify a globally unique name for the bucket you want to create. Otherwise, the bucket fails to be created.
-   Bucket names must comply with bucket naming conventions. For more information about bucket naming conventions, see [bucket](/intl.en-US/Developer Guide/Terms.md).
-   After a bucket is created, the name and the region of the bucket cannot be modified.
-   You can specify the access control list \(ACL\) of a bucket when you create the bucket. For more information, see [Set the ACL for a bucket](/intl.en-US/Developer Guide/Buckets/Set the ACL for a bucket.md). If you do not specify the ACL of a bucket when you create the bucket, the ACL of the bucket is automatically set to private.
-   You can specify the storage class of a bucket when you create the bucket. For more information, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md). If you do not specify the storage class of a bucket when you create the bucket, the storage class of the bucket is automatically set to Standard.

## Examples

The following code provides an example on how to create a bucket named examplebucket and set the ACL of the bucket to public read:

```
id<OSSCredentialProvider> credentialProvider = [[OSSAuthCredentialProvider alloc] initWithAuthServerUrl:@"<StsServer>"];
OSSClientConfiguration *cfg = [[OSSClientConfiguration alloc] init];
cfg.maxRetryCount = 3;
cfg.timeoutIntervalForRequest = 15;

// When an OSSClient is released, all tasks in the session are canceled and the session becomes invalid. Therefore, make sure that the OSSClient is not released before the request is complete. 
// Set Endpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set Endpoint to https://oss-cn-hangzhou.aliyuncs.com. 

OSSClient *client = [[OSSClient alloc] initWithEndpoint:@"Endpoint" credentialProvider:credentialProvider clientConfiguration:cfg];

OSSCreateBucketRequest * create = [OSSCreateBucketRequest new];
create.bucketName = @"examplebucket";
create.xOssACL = @"public-read";

OSSTask * createTask = [client createBucket:create];

[createTask continueWithBlock:^id(OSSTask *task) {
    if (!task.error) {
        NSLog(@"create bucket success!");
    } else {
        NSLog(@"create bucket failed, error: %@", task.error);
    }
    return nil;
}];            
```

