# Create buckets

A bucket is a container for objects stored in OSS. Every object is contained in a bucket. This topic describes how to create a bucket.

For the complete code used to create a bucket, visit [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/CreateBucketSample.cs).

The following code provides an example on how to create a bucket:

```
using Aliyun.OSS;
// Initialize an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
// Create a bucket.
public void CreateBucket(string bucketName)
{
    try
    {
        var request = new CreateBucketRequest(bucketName);
        // Configure the ACL for buckets.
        request.ACL = CannedAccessControlList.PublicReadWrite;
        // Set the data redundancy type.
        request.DataRedundancyType = DataRedundancyType.ZRS;
        // Create a bucket. The bucket name must be globally unique.
        client.CreateBucket(request);
        Console.WriteLine("Create bucket succeeded");
    }
    catch (Exception ex)
    {
        Console.WriteLine("Create bucket failed. {0}", ex.Message);
    }
}
```

For more information about the naming conventions of buckets, see [bucket](/intl.en-US/Developer Guide/Terms.md). For more information about how to create a bucket, see [PutBucket](/intl.en-US/API Reference/Bucket operations/Basic operations/PutBucket.md).

