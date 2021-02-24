# Resumable download

You may fail to download a large object if the network is unstable or other exceptions occur. In some cases, you may still fail to download the object even after multiple attempts. To solve this problem, OSS provides the resumable download feature.

The following code provides an example on how to perform resumable download:

```
using Aliyun.OSS;
using Aliyun.OSS.Common;

var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var bucketName = "<yourBucketName>";
var objectName = "<yourObjectName>";
var localFilename = "<yourLocalFilename>";
var downloadFilename = "<yourDownloadFilename>";
var checkpointDir = "<yourCheckpointDir>";
// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    // Configure multiple parameters by using DownloadObjectRequest.
    DownloadObjectRequest request = new DownloadObjectRequest(bucketName, objectName, downloadFilename)
    {
        // Specify the size of each part to download.
        PartSize = 8 * 1024 * 1024,
        // Specify the number of concurrent threads.
        ParallelThreadCount = 3,
        // checkpointDir is a file used to store the information about the resumable upload progress. If a part fails to be downloaded, the download can be continued based on the progress information recorded in this file. If you set checkpointDir to null, resumable upload does not take effect and objects are uploaded again when they fail to be uploaded.
        CheckpointDir = checkpointDir,
    };
    // Start the resumable download.
    client.ResumableDownloadObject(request);
    Console.WriteLine("Resumable download object:{0} succeeded", objectName);
}
catch (OssException ex)
{
    Console.WriteLine("Failed with error code: {0}; Error info: {1}. \nRequestID:{2}\tHostID:{3}",
        ex.ErrorCode, ex.Message, ex.RequestId, ex.HostId);
}
catch (Exception ex)
{
    Console.WriteLine("Failed with error info: {0}", ex.Message);
}
```

For more information about resumable download, see [Resumable download](/intl.en-US/Developer Guide/Objects/Download files/Resumable download.md).

