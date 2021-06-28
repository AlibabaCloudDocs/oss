# Delete objects

This topic describes how to delete a single object or multiple objects.

**Warning:** You cannot recover deleted objects. Exercise caution when you delete objects.

## Delete a single object

The following code provides an example on how to delete an object:

```
using Aliyun.OSS;
// Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
var endpoint = "yourEndpoint";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to Object Storage Service (OSS) because the account has permissions on all API operations. We recommend that you use a Resource Access Management (RAM) user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
var accessKeyId = "yourAccessKeyId";
var accessKeySecret = "yourAccessKeySecret";
var bucketName = "examplebucket";
var objectName = "exampleobject.txt";
// Create an OSSClient instance. 
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    // Delete the object. 
    client.DeleteObject(bucketName, objectName);
    Console.WriteLine("Delete object succeeded");
}
catch (Exception ex)
{
    Console.WriteLine("Delete object failed. {0}", ex.Message);
}
```

## Delete multiple objects

You can delete up to 1,000 objects each time.

The results can be returned in the following modes. Select a return mode.

-   Verbose: If quietMode is not specified or is set to false, a list of all deleted objects is returned. This is the default return mode.
-   Quiet: If quietMode is set to true, a list of objects that fail to delete is returned.

The following code provides an example on how to delete multiple specified objects from a bucket named examplebucket:

```
using Aliyun.OSS;
// Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
var endpoint = "yourEndpoint";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
var accessKeyId = "yourAccessKeyId";
var accessKeySecret = "yourAccessKeySecret";
var bucketName = "examplebucket";
// Create an OSSClient instance. 
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    // Specify the paths of multiple objects you want to delete. The paths cannot contain bucket names. 
    var keys = new List<string>();
    keys.Add("exampleobject.txt");
    keys.Add("testdir/sampleobject.txt");
    // Leave quietMode empty or set quietMode to false to return the list of deleted objects. 
    var quietMode = false;
    var request = new DeleteObjectsRequest(bucketName, keys, quietMode);
    // Delete multiple objects. 
    var result = client.DeleteObjects(request);
    if ((!quietMode) && (result.Keys != null))
    {
        foreach (var obj in result.Keys)
        {
            Console.WriteLine("Delete successfully : {0} ", obj.Key);
        }
    }
    Console.WriteLine("Delete objects succeeded");
}
catch (Exception ex)
{
    Console.WriteLine("Delete objects failed. {0}", ex.Message);
}
```

For the complete code used to delete multiple objects, visit [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/DeleteObjectsSample.cs).

