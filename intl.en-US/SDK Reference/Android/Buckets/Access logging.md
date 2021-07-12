# Access logging

You can enable access logging to record bucket access to log files, which are stored in a specified bucket.

The log file format is as follows:

```
<TargetPrefix><SourceBucket>YYYY-mm-DD-HH-MM-SS-UniqueString
```

For more information about access log files, see [Set access logging](/intl.en-US/Developer Guide/Manage logs/Log storage.md).

## Enable access logging

Run the following code to enable bucket access logging:

```
PutBucketLoggingRequest request = new PutBucketLoggingRequest();
//Specify the source bucket for which to enable access logging.
request.setBucketName("<yourSourceBucketName>");
//Specify the destination bucket for storing access logs.
//You can specify the same bucket or different ones to function as the source and destination buckets. The source and destination bucket can both be the same or different buckets.
request.setTargetBucketName("<yourTargetBucketName>");
//Set the directory where access logs are stored.
request.setTargetPrefix("<yourTargetPrefix>");

OSSAsyncTask task = oss.asyncPutBucketLogging(request, new OSSCompletedCallback<PutBucketLoggingRequest, PutBucketLoggingResult>() {
    @Override
    public void onSuccess(PutBucketLoggingRequest request, PutBucketLoggingResult result) {
        OSSLog.logInfo("code::"+result.getStatusCode());
    }

    @Override
    public void onFailure(PutBucketLoggingRequest request, ClientException clientException, ServiceException serviceException) {
         OSSLog.logError("error: "+serviceException.getRawMessage());
    }
});
task.waitUntilFinished();
```

For more information about how to enable access logging for a bucket, see [PutBucketLogging](/intl.en-US/API Reference/Bucket operations/Logging/PutBucketLogging.md).

## View access logging configurations

Run the following code to view the access logging configurations for a bucket:

```
GetBucketLoggingRequest request = new GetBucketLoggingRequest();
request.setBucketName("<yourSourceBucketName>");
OSSAsyncTask task = oss.asyncGetBucketLogging(request, new OSSCompletedCallback<GetBucketLoggingRequest, GetBucketLoggingResult>() {
    @Override
    public void onSuccess(GetBucketLoggingRequest request, GetBucketLoggingResult result) {
        OSSLog.logInfo("info: " + result.getTargetBucketName()+"*"+result.getTargetPrefix());

    }

    @Override
    public void onFailure(GetBucketLoggingRequest request, ClientException clientException, ServiceException serviceException) {
        OSSLog.logError("error: "+serviceException.getRawMessage());

    }
});
task.waitUntilFinished();
```

For more information about how to view access logging configurations of a bucket, see [GetBucketLogging](/intl.en-US/API Reference/Bucket operations/Logging/GetBucketLogging.md).

## Disable access logging

Run the following code to disable access logging for a bucket:

```
DeleteBucketLoggingRequest request = new DeleteBucketLoggingRequest();
request.setBucketName("<yourSourceBucketName>");
OSSAsyncTask task = oss.asyncDeleteBucketLogging(request, new OSSCompletedCallback<DeleteBucketLoggingRequest, DeleteBucketLoggingResult>() {
    @Override
    public void onSuccess(DeleteBucketLoggingRequest request, DeleteBucketLoggingResult result) {
        OSSLog.logInfo("code::"+result.getStatusCode());

    }

    @Override
    public void onFailure(DeleteBucketLoggingRequest request, ClientException clientException, ServiceException serviceException) {
        OSSLog.logError("error: "+serviceException.getRawMessage());

    }
});

task.waitUntilFinished();
            
```

For more information about how to disable access logging for a bucket, see [DeleteBucketLogging](/intl.en-US/API Reference/Bucket operations/Logging/DeleteBucketLogging.md).

