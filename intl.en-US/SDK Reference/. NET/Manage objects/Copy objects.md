# Copy objects

## Copy a small-sized object

For the complete code of copying a small-sized object, see [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/CopyObjectSample.cs).

Note the following limits before copying a small-sized object:

-   You must have the read and write permission on the source object.
-   You cannot copy an object from a region to another region. For example, you cannot copy an object from a bucket in Hangzhou region to a bucket in Qingdao region.
-   The object size is limited to a maximum of 1 GB.

Run the following code to copy a small-sized object:

```
using Aliyun.OSS;
using Aliyun.OSS.Common;
var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var sourceBucket = "<yourSourceBucketName>";
var sourceObject = "<yourSourceObjectName>";
var targetBucket = "<yourDestBucketName>";
var targetObject = "<yourDestObjectName>";
// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    var metadata = new ObjectMetadata();
    metadata.AddHeader("mk1", "mv1");
    metadata.AddHeader("mk2", "mv2");
    var req = new CopyObjectRequest(sourceBucket, sourceObject, targetBucket, targetObject)
    {
        // If the value of NewObjectMetadata is null, the COPY mode is used (the metadata of the source object is copied). If the value of NewObjectMetadata is not null, the REPLACE mode is used (the metadata of the source object is replaced).
        NewObjectMetadata = metadata 
    };
    // Copy an object.
    client.CopyObject(req);
    Console.WriteLine("Copy object succeeded");
}
catch (OssException ex)
{
    Console.WriteLine("Failed with error code: {0}; Error info: {1}. \nRequestID: {2} \tHostID: {3}",
        ex.ErrorCode, ex.Message, ex.RequestId, ex.HostId);
}
catch (Exception ex)
{
    Console.WriteLine("Failed with error info: {0}", ex.Message);
}
```

## Copy a large-sized object

For the complete code of copying large-sized objects, see [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/MultipartUploadSample.cs).

-   UploadPartCopy

    To copy an object larger than 1 GB, you need to use UploadPartCopy.

    1.  Use InitiateMultipartUploadRequest to initiate an UploadPartCopy event.
    2.  Use the UploadPartCopy method to perfome a multipart copy.
    3.  Use the CompleteMultipartUpload method to complete the object copy.
    Run the following code for UploadPartCopy task:

    ```
    using Aliyun.OSS;
    var endpoint = "<yourEndpoint>";
    var accessKeyId = "<yourAccessKeyId>";
    var accessKeySecret = "<yourAccessKeySecret>";
    var sourceBucket = "<yourSourceBucketName>";
    var sourceObject = "<yourSourceObjectName>";
    var targetBucket = "<yourDestBucketName>";
    var targetObject = "<yourDestObjectName>";
    var uploadId = "";
    var partSize = 50 * 1024 * 1024;
    // Create an OSSClient instance.
    var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
    try
    {
        // Initiate a copy task. You can use the InitiateMultipartUploadRequest method to specify the metadata information about the target object.
        var request = new InitiateMultipartUploadRequest(targetBucket, targetObject);
        var result = client.InitiateMultipartUpload(request);
        // Print the uploadId.
        uploadId = result.UploadId;
        Console.WriteLine("Init multipart upload succeeded, Upload Id: {0}", result.UploadId);
        // Calculate the number of parts.
        var metadata = client.GetObjectMetadata(sourceBucket, sourceObject);
        var fileSize = metadata.ContentLength;
        var partCount = (int)fileSize / partSize;
        if (fileSize % partSize ! = 0)
        {
            partCount++;
        }
        // Start the UploadPartCopy task.
        var partETags = new List<PartETag>();
        for (var i = 0; i < partCount; i++)
        {
            var skipBytes = (long)partSize * i;
            var size = (partSize < fileSize - skipBytes) ? partSize : (fileSize - skipBytes);
            // Create UploadPartCopyRequest. You can specify conditions with UploadPartCopyRequest.
            var uploadPartCopyRequest = new UploadPartCopyRequest(targetBucket, targetObject, sourceBucket, sourceObject, uploadId)
                {
                    PartSize = size,
                    PartNumber = i + 1,
                    // Beginindex is used to locate the location that the last UploadPartCopy starts.
                    BeginIndex = skipBytes
                };
            // Call the uploadPartCopy method to copy each part.
            var uploadPartCopyResult = client.UploadPartCopy(uploadPartCopyRequest);
            Console.WriteLine("UploadPartCopy : {0}", i);
            partETags.Add(uploadPartCopyResult.PartETag);
        }
        // Complete the UploadPartCopy task.
        var completeMultipartUploadRequest =
        new CompleteMultipartUploadRequest(targetBucket, targetObject, uploadId);
        // The partETags refers to the list of partETags saved during the multipart upload. After OSS receives the part list submitted by the user, it verifies the validity of each data part one by one. After the verification is passed, OSS conbine these parts into a complete object.
        foreach (var partETag in partETags)
        {
            completeMultipartUploadRequest.PartETags.Add(partETag);
        }
        var completeMultipartUploadResult = client.CompleteMultipartUpload(completeMultipartUploadRequest);
        Console.WriteLine("CompleteMultipartUpload succeeded");
    }
    catch (OssException ex)
    {
        Console.WriteLine("Failed with error code: {0}; Error info: {1}. \nRequestID: {2} \tHostID: {3}",
            ex.ErrorCode, ex.Message, ex.RequestId, ex.HostId);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Failed with error info: {0}", ex.Message);
    }
    ```

-   Resumable copy

    If the copy is interrupted, you can use resumable copy to continue the copy. For the complete code of resumable copy, see [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/ResumableSample.cs).

    Run the following code for resumable copy:

    ```
    using Aliyun.OSS;
    var endpoint = "<yourEndpoint>";
    var accessKeyId = "<yourAccessKeyId>";
    var accessKeySecret = "<yourAccessKeySecret>";
    var sourceBucket = "<yourSourceBucketName>";
    var sourceObject = "<yourSourceObjectName>";
    var targetBucket = "<yourDestBucketName>";
    var targetObject = "<yourDestObjectName>";
    var checkpointDir = @"<yourCheckpointDir>";
    // Create an OSSClient instance.
    var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
    try
    {
        var request = new CopyObjectRequest(sourceBucket, sourceObject, targetBucket, targetObject);
        // The checkpointDir directory stores the intermediate state information of a resumable upload. The information will be used when a failed upload task is resumed. // If the checkpointDir directory is null, resumable copy will not take effect, and the object that previously failed to be copied will be copied all over again.
        client.ResumableCopyObject(request, checkpointDir);
        Console.WriteLine("Resumable copy new object:{0} succeeded", request.DestinationKey);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Resumable copy new object failed, {0}", ex.Message);
    }
    ```


