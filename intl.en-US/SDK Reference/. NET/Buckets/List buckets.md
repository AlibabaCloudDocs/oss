# List buckets

A bucket is a container for objects stored in OSS. Every object is contained in a bucket. This topic describes how to list buckets.

For the complete code used to list buckets, visit [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/ListBucketsSample.cs).

The following code provides an example on how to list all buckets:

```
using Aliyun.OSS;

// Initialize an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);

// List all buckets of the user.
public void ListBuckets()
{
    try
    {
        var buckets = client.ListBuckets();

        Console.WriteLine("List bucket succeeded");
        foreach (var bucket in buckets)
        {
            Console.WriteLine("Bucket name:{0}, Location:{1}, Owner:{2}", bucket.Name, bucket.Location, bucket.Owner);
        }
    }
    catch (Exception ex)
    {
        Console.WriteLine("List bucket failed. {0}", ex.Message);
    }
}
        
```

For more information about how to list buckets, see [GetBucket \(ListObjects\)](/intl.en-US/API Reference/Bucket operations/Basic operations/GetBucket (ListObjects).md).

