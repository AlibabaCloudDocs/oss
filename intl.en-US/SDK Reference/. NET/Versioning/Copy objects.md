# Copy objects

This topic describes how to copy objects in a versioned bucket. You can use CopyObject to copy an object that is up to 1 GB in size and use UploadPartCopy to copy an object larger than 1 GB.

## Copy a small object

You can use CopyObject to copy an object that is up to 1 GB in size from a source bucket to a destination bucket within the same region.

-   By default, the x-oss-copy-source header in a CopyObject request specifies the current version of the object to copy. If the current version of the object to copy is a delete marker, 404 Not Found is returned to indicate that the object does not exist. You can add a version ID in the x-oss-copy-source header to copy a specified object version. Delete markers cannot be copied.
-   You can copy a previous version of an object to the same bucket. The copied previous version becomes the current version of the object. This way, you can restore an object to a specified previous version.
-   If versioning is enabled for the destination bucket, OSS generates a unique version ID for the destination object. The version ID is returned in the response as the x-oss-version-id header value. If versioning is disabled or suspended for the destination bucket, OSS generates a version whose version ID is null for the destination object and overwrites the original version whose version ID is null.
-   Append objects in a bucket with versioning enabled or suspended cannot be copied.

The following code provides an example on how to copy a small object:

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
var versionid = "<yourArchiveObjectVersionid>";
// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    var metadata = new ObjectMetadata();
    metadata.AddHeader("mk1", "mv1");
    metadata.AddHeader("mk2", "mv2");
    var req = new CopyObjectRequest(sourceBucket, sourceObject, targetBucket, targetObject)
    {
        // If the value of NewObjectMetadata is null, the COPY mode is used and the metadata of the source object is copied to the destination object. If the value of NewObjectMetadata is not null, the REPLACE mode is used and the metadata of the source object overwrites that of the destination object.
        NewObjectMetadata = metadata, 
        // Specify the version ID of the source object.
        SourceVersionId = versionid
    };
    // Copy the object.
    var result = client.CopyObject(req);
    Console.WriteLine("Copy object succeeded, vesionid:{0}", result.VersionId);
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

## Copy a large object

To copy an object larger than 1 GB, you must split the object into parts and copy them sequentially by using UploadPartCopy.

By default, the UploadPartCopy operation uploads a part by copying data from the current version of an existing object. You can add versionId in the request header x-oss-copy-source as the condition to upload a part by copying data from a specified version of an existing object. Example: x-oss-copy-source : /SourceBucketName/SourceObjectName?versionId=111111.

**Note:** The value of SourceObjectName must be URL-encoded. The version ID of the copied object version is returned as the x-oss-copy-source-version-id header value in the response.

If versionId is not specified in the request and the current version of the source object is a delete marker, OSS returns 404 Not Found. If versionId is specified in the request and the specified version of the source object is a delete marker, OSS returns 400 Bad Request.

The following code provides an example on how to use multipart copy to copy large objects:

```
using Aliyun.OSS;
var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var sourceBucket = "<yourSourceBucketName>";
var sourceObject = "<yourSourceObjectName>";
var targetBucket = "<yourDestBucketName>";
var targetObject = "<yourDestObjectName>";
var sourceObjectVersionid = "<yourSourceObjectVersionid>";
var uploadId = "";
var partSize = 50 * 1024 * 1024;
// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    // Initiate a multipart copy task. You can call the InitiateMultipartUploadRequest operation to specify the metadata of the destination object.
    var request = new InitiateMultipartUploadRequest(targetBucket, targetObject);
    var result = client.InitiateMultipartUpload(request);
    // Display the value of UploadId.
    uploadId = result.UploadId;
    Console.WriteLine("Init multipart upload succeeded, Upload Id: {0}", result.UploadId);
    // Calculate the total number of parts.
    var request = new GetObjectMetadataRequest(sourceBucket, sourceObject)
    {
        // Specify the version ID of the source object.
        VersionId = sourceObjectVersionid
    };
    var metadata = client.GetObjectMetadata(request);
    var fileSize = metadata.ContentLength;
    var partCount = (int)fileSize / partSize;
    if (fileSize % partSize ! = 0)
    {
        partCount++;
    }
    // Start the multipart copy task.
    var partETags = new List<PartETag>();
    for (var i = 0; i < partCount; i++)
    {
        var skipBytes = (long)partSize * i;
        var size = (partSize < fileSize - skipBytes) ? partSize : (fileSize - skipBytes);
        // Create an UploadPartCopyRequest request. You can call the UploadPartCopyRequest operation to specify conditions.
        var uploadPartCopyRequest = new UploadPartCopyRequest(targetBucket, targetObject, sourceBucket, sourceObject, uploadId)
            {
                PartSize = size,
                PartNumber = i + 1,
                // Use BeginIndex to find the start position to copy the part.
                BeginIndex = skipBytes,
                // Specify the version ID of the source object.
                VersionId = sourceObjectVersionid
            };
        // Call the uploadPartCopy operation to copy each part.
        var uploadPartCopyResult = client.UploadPartCopy(uploadPartCopyRequest);
        Console.WriteLine("UploadPartCopy : {0}", i);
        partETags.Add(uploadPartCopyResult.PartETag);
    }
    // Complete the multipart copy task.
    var completeMultipartUploadRequest =
    new CompleteMultipartUploadRequest(targetBucket, targetObject, uploadId);
    // partETags is a list of partETag. OSS verifies the validity of each part after it receives partETags. After all parts are verified, OSS combines these parts into a complete object.
    foreach (var partETag in partETags)
    {
        completeMultipartUploadRequest.PartETags.Add(partETag);
    }
    var completeMultipartUploadResult = client.CompleteMultipartUpload(completeMultipartUploadRequest);
    Console.WriteLine("CompleteMultipartUpload succeeded, vesionid:{0}", completeMultipartUploadResult.VersionId);
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

For more information about multipart copy, see [UploadPartCopy](/intl.en-US/API Reference/Object operations/Multipart upload/UploadPartCopy.md).

