# Simple upload

To implement simple upload, you can call PutObject to upload a single object. You can use simple upload to upload strings and local files in synchronous or asynchronous mode. You can perform MD5 verification to ensure data integrity in simple upload.

## Upload a string

**Note:** For the complete code used to upload a string, visit [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/PutObjectSample.cs).

The following code provides an example on how to upload a string to an object named exampleobject.txt in a bucket named examplebucket:

```
using System.Text;
using Aliyun.OSS;

// Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
var endpoint = "yourEndpoint";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to Object Storage Service (OSS) because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
var accessKeyId = "yourAccessKeyId";
var accessKeySecret = "yourAccessKeySecret";
// Specify the bucket name. 
var bucketName = "examplebucket";
// Specify the full path of the object. The full path of the object cannot contain bucket names. 
var objectName = "exampleobject.txt";
// Specify the string to upload. 
var objectContent = "More than just cloud.";

// Create an OSSClient instance. 
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    byte[] binaryData = Encoding.ASCII.GetBytes(objectContent);
    MemoryStream requestContent = new MemoryStream(binaryData);
    // Upload the string to the object. 
    client.PutObject(bucketName, objectName, requestContent);
    Console.WriteLine("Put object succeeded");
}
catch (Exception ex)
{
    Console.WriteLine("Put object failed, {0}", ex.Message);
}
```

## Upload a local file

**Note:** For the complete code used to upload a local file, visit [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/PutObjectSample.cs).

The following code provides an example on how to upload a local file named examplefile.txt as an object with the same name in a bucket named examplebucket:

```
using Aliyun.OSS;

// Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
var endpoint = "yourEndpoint";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
var accessKeyId = "yourAccessKeyId";
var accessKeySecret = "yourAccessKeySecret";
// Specify the bucket name. 
var bucketName = "examplebucket";
// Specify the full path of the object. The full path of the object cannot contain bucket names. 
var objectName = "exampleobject.txt";
// Specify the full path of the local file to upload. If the path of the local file is not specified, the local file is uploaded to the path of the project to which the sample program belongs. 
var localFilename = "D:\\localpath\\examplefile.txt";

// Create an OSSClient instance. 
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    // Upload the local file. 
    client.PutObject(bucketName, objectName, localFilename);
    Console.WriteLine("Put object succeeded");
}
catch (Exception ex)
{
    Console.WriteLine("Put object failed, {0}", ex.Message);
}
```

## Upload a local file and perform MD5 verification

**Note:** For the complete code used to upload a local file and perform MD5 verification, visit [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/PutObjectSample.cs).

To ensure that the data sent by the client is the same as that received by the OSS server, you can add the Content-MD5 header in the request as object metadata. OSS uses the value of this header to perform MD5 verification.

**Note:** Performance may decline if MD5 verification is used.

The following code provides an example on how to perform MD5 verification when you upload a local file:

```
using Aliyun.OSS;
using Aliyun.OSS.Util;

// Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
var endpoint = "yourEndpoint";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
var accessKeyId = "yourAccessKeyId";
var accessKeySecret = "yourAccessKeySecret";
// Specify the bucket name.
var bucketName = "examplebucket";
// Specify the full path of the object. The full path of the object cannot contain bucket names.
var objectName = "exampleobject.txt";
// Specify the full path of the local file to upload. If the path of the local file is not specified, the file is uploaded to the path of the project to which the sample program belongs.
var localFilename = "D:\\localpath\\examplefile.txt";

// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    // Calculate the MD5 hash of local file.
    string md5;
    using (var fs = File.Open(localFilename, FileMode.Open))
    {
        md5 = OssUtils.ComputeContentMd5(fs, fs.Length);
    }
    var objectMeta = new ObjectMetadata
    {
        ContentMd5 = md5
    };
    // Upload the local file.
    client.PutObject(bucketName, objectName, localFilename, objectMeta);
    Console.WriteLine("Put object succeeded");
}
catch (Exception ex)
{
    Console.WriteLine("Put object failed, {0}", ex.Message);
}
```

## Upload a local file in asynchronous mode

**Note:** For the complete code used to upload a local file in asynchronous mode, visit [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/PutObjectSample.cs).

The following code provides an example on how to upload a local file named examplefile.txt as an object named exampleobject.txt in a bucket named examplebucket in asynchronous mode. When you upload an object in asynchronous mode, you must implement a function to process callback information.

```
using System;
using System.IO;
using System.Threading;

using Aliyun.OSS;
using Aliyun.OSS.Common;

// Upload the local file in asynchronous mode.
namespace AsyncPutObject
{
    class Program
    {
        // Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
        static string endpoint = "yourEndpoint";
        // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
        static string accessKeyId = "yourAccessKeyId";
        static string accessKeySecret = "yourAccessKeySecret";
        // Specify the bucket name.
        static string bucketName = "examplebucket";
        // Specify the full path of the object. The full path of the object cannot contain bucket names.
        static string objectName = "exampleobject.txt";
        // Specify the full path of the local file to upload. If the path of the local file is not specified, the file is uploaded to the path of the project to which the sample program belongs.
        static string localFilename = "D:\\localpath\\examplefile.txt";

        static AutoResetEvent _event = new AutoResetEvent(false);

        // Create an OSSClient instance.
        static OssClient client = new OssClient(endpoint, accessKeyId, accessKeySecret);

        private static void PutObjectCallback(IAsyncResult ar)
        {
            try
            {
                client.EndPutObject(ar);
                Console.WriteLine(ar.AsyncState as string);
                Console.WriteLine("Put object succeeded");
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
            finally
            {
                _event.Set();
            }
        }

        public static void AsyncPutObject()
        {
            try
            {
                using (var fs = File.Open(localFilename, FileMode.Open))
                {
                    var metadata = new ObjectMetadata();
                    // Add user metadata for the object.
                    metadata.UserMetadata.Add("mykey1", "myval1");
                    metadata.UserMetadata.Add("mykey2", "myval2");
                    metadata.CacheControl = "No-Cache";
                    metadata.ContentType = "text/txt";
                    string result = "Notice user: put object finish";

                    // Upload the local file in asynchronous mode.
                    client.BeginPutObject(bucketName, objectName, fs, metadata, PutObjectCallback, result.ToCharArray());

                    _event.WaitOne();
                }
            }
            catch (OssException ex)
            {
                Console.WriteLine("Failed with error code:{0}; Error info:{1}.\nRequestID:{2}\tHostID:{3}",
                    ex.ErrorCode, ex.Message, ex.RequestId, ex.HostId);
            }
            catch (Exception ex)
            {
                Console.WriteLine("Failed with error info: {0}", ex.Message);
            }
        }

        static void Main(string[] args)
        {
            Program.AsyncPutObject();
        }
    }
} 
```

