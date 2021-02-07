# Resumable download

You may fail to download a large object if the network is unstable or if the program stops responding. In some cases, you may still fail to download the object even after multiple attempts. To solve this problem, OSS SDK for Java provides the resumable download feature.

In resumable download, objects that you want to download are split into multiple parts and downloaded separately. After all parts are downloaded, these parts are combined into a complete object.

You can use ossClient.downloadFile to perform resumable download. The following table describes the parameters that you can configure for DownloadFileRequest.

|Parameter|Description|Required|Default Value|Configuration method|
|:--------|:----------|:-------|:------------|:-------------------|
|bucketName|The name of the bucket.|Yes|None|Constructor|
|key|The name of the object to be downloaded.|Yes|None|Constructor|
|downloadFile|The path to the local file. The OSS object is downloaded to this file.|No|The name of the downloaded object.|Constructor or setUploadFile|
|partSize|The size of each part. Valid values: 1 byte to 5 GB.|No|Object size/10000|setPartSize|
|taskNum|The number of parts that can be downloaded simultaneously.|No|1|setTaskNum|
|enableCheckpoint|Specifies whether to enable resumable upload.|No|Disable|setEnableCheckpoint|
|checkpointFile|The file that records the results of multipart download. This parameter must be configured if you want to enable resumable upload. This file stores information about download progress. If you fail to download a part, the next download continues based on the recorded progress. After the object is downloaded, the checkpoint file is deleted.|No|downloadFile.ucp \(share the same directory with DownloadFile\)|setCheckpointFile|

The following code provides an example on how to perform resumable download:

```
// Set yourEndpoint to the endpoint of the region where the bucket is located. For example, if your bucket is in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com.
String endpoint = "yourEndpoint";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "yourAccessKeyId";
String accessKeySecret = "yourAccessKeySecret";
String bucketName = "yourBucketName";
String objectName = "yourObjectName";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Send a request to perform resumable download in which 10 parts can be downloaded simultaneously.
DownloadFileRequest downloadFileRequest = new DownloadFileRequest(bucketName, objectName);
downloadFileRequest.setDownloadFile("<yourDownloadFile>");
downloadFileRequest.setPartSize(1 * 1024 * 1024);
downloadFileRequest.setTaskNum(10);
downloadFileRequest.setEnableCheckpoint(true);
downloadFileRequest.setCheckpointFile("<yourCheckpointFile>");

// Download the object to your local file.
DownloadFileResult downloadRes = ossClient.downloadFile(downloadFileRequest);
// After the object is downloaded, the object metadata is returned.
downloadRes.getObjectMetadata();

// Shut down the OSSClient instance.
ossClient.shutdown();
```

