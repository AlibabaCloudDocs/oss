# Resumable upload

Before you upload an object to Object Storage Service \(OSS\) by using resumable upload, you can specify a folder for the checkpoint file that stores resumable upload records. If an object fails to upload because the network is abnormal or the program stops responding, the upload task is resumed from the position recorded in the checkpoint file to upload the remaining bytes.

## Parameters

You can use ossClient.uploadFile to implement resumable upload. The following table describes the parameters you can configure for uploadFileRequest.

|Parameter|Description|
|---------|-----------|
|BucketName|The name of the bucket.|
|Key|The name of the object uploaded to OSS.|
|UploadFile|The path of the local file to upload to OSS.|
|TaskNum|The number of concurrent threads for the resumable upload task. Default value: 1.|
|PartSize|The size of each part. Valid values: 100 KB to 5 GB. Default value: 100 KB.|
|EnableCheckpoint|Specifies whether to enable resumable upload. Default value: false.|
|CheckpointFile|The checkpoint file that records the upload result of each part. This file stores information about upload progress. If a part fails to upload, the task can be continued based on the progress recorded in the checkpoint file. After the entire local file is uploaded, the checkpoint file is deleted. By default, if you do not specify this parameter, this checkpoint file shares the same directory as the object you want to upload. The directory is named $\{uploadFile\}.ucp.|
|Callback|Use upload callback. For more information about upload callback, see [Callback](/intl.en-US/API Reference/Object operations/Basic operations/Callback.md) and [Upload callback](/intl.en-US/SDK Reference/Java/Upload objects/Upload callback.md).|

## Examples

The following code provides an example on how to perform resumable upload by using multipart upload:

```
/// Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
String endpoint = "yourEndpoint";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
String accessKeyId = "yourAccessKeyId";
String accessKeySecret = "yourAccessKeySecret";

// Create an OSSClient instance. 
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

ObjectMetadata meta = new ObjectMetadata();
// Specify the type of content you want to upload. 
meta.setContentType("text/plain");

// Specify the access control list (ACL) of the object to upload. 
// meta.setObjectAcl(CannedAccessControlList.Private);

// Configure parameters by using UploadFileRequest. 
// Specify the name of the bucket and the full path of the object. The full path of the object cannot contain bucket names. 
UploadFileRequest uploadFileRequest = new UploadFileRequest("examplebucket","exampleobject.txt");

// Configure a single parameter by using UploadFileRequest. 
// Specify the bucket name. 
//uploadFileRequest.setBucketName("examplebucket");
// Specify the full path of the object. The full path of the object cannot contain bucket names. 
//uploadFileRequest.setKey("exampleobject.txt");
// Specify the full path of the local file to upload. If the path of the local file is not specified, the local file is uploaded from the path of the project to which the sample program belongs. 
uploadFileRequest.setUploadFile("D:\\localpath\\examplefile.txt");
// Specify the number of threads for the resumable upload task. Default value: 1. 
uploadFileRequest.setTaskNum(5);
// Specify the size of each part to be uploaded. 
uploadFileRequest.setPartSize(1 * 1024 * 1024);
// Specify whether to enable resumable upload. By default, resumable upload is disabled. 
uploadFileRequest.setEnableCheckpoint(true);
// Specify the checkpoint file that records the upload result of each part. The file stores the progress information generated in the upload process. 
uploadFileRequest.setCheckpointFile("yourCheckpointFile");
// Configure object meta. 
uploadFileRequest.setObjectMetadata(meta);
// Configure upload callback. The parameter type is Callback. 
//uploadFileRequest.setCallback("yourCallbackEvent");

// Start resumable upload. 
ossClient.uploadFile(uploadFileRequest);

// Shut down the OSSClient instance. 
ossClient.shutdown();            
```

For more information about resumable upload, see [Multipart upload and resumable upload](/intl.en-US/Developer Guide/Objects/Upload files/Multipart upload and resumable upload.md). For the complete code, visit [GitHub](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/UploadSample.java).

