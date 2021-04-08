# Progress bars

You can use a progress bar to indicate the progress of a task initiated to upload or download an object. GetObject is used in the example to describe how to display the progress bar of an object download task.

The following code provides an example on how to display the progress bar of the task initiated to download the exampleobject.txt object from examplebucket:

```
// Construct a download request.
// Specify the name of the bucket and the full path of the object. The full path of the object cannot contain bucket names.
GetObjectRequest get = new GetObjectRequest("examplebucket", "exampleobject.txt");
// Set the callback function for the progress bar and display the progress bar.
get.setProgressListener(new OSSProgressCallback<GetObjectRequest>() {
    @Override
    public void onProgress(GetObjectRequest request, long currentSize, long totalSize) {
        Log.d("GetObject", "currentSize: " + currentSize + " totalSize: " + totalSize);
    }
});
// Asynchronously download the object.
OSSAsyncTask task = oss.asyncGetObject(get, new OSSCompletedCallback<GetObjectRequest, GetObjectResult>() {
    @Override
    public void onSuccess(GetObjectRequest request, GetObjectResult result) {
        Log.d("GetObject", "UploadSuccess");
    }

    @Override
    public void onFailure(GetObjectRequest request, ClientException clientException, ServiceException serviceException) {
        // Request exceptions.
        if (clientException != null) {
            // Client-side exceptions such as network errors.
            clientException.printStackTrace();
        }
        if (serviceException != null) {
            // Server-side exceptions.
            Log.e("ErrorCode", serviceException.getErrorCode());
            Log.e("RequestId", serviceException.getRequestId());
            Log.e("HostId", serviceException.getHostId());
            Log.e("RawMessage", serviceException.getRawMessage());
        }
    }
});
```

