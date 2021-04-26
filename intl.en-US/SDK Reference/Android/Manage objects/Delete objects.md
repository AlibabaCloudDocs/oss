# Delete objects

This topic describes how to delete a single object or batch delete objects.

**Warning:** Deleted objects cannot be recovered. Exercise caution when you delete objects.

## Usage notes

To delete an object, you must have write permissions on the bucket in which the object is stored.

## Delete a single object

The following code provides an example on how to delete an object named exampleobject.txt from a bucket named examplebucket:

```
// Create the delete request. 
// Specify the bucket name and the full path of the object that you want to delete. The full path of the object cannot contain bucket names.DeleteObjectRequest delete = new DeleteObjectRequest("examplebucket", "exampleobject.txt");// Delete the object asynchronously.OSSAsyncTask deleteTask = oss.asyncDeleteObject(delete, new OSSCompletedCallback<DeleteObjectRequest, DeleteObjectResult>() {
    @Override
    public void onSuccess(DeleteObjectRequest request, DeleteObjectResult result) {
        Log.d("asyncDeleteObject", "success!"); 
    }

    @Override
    public void onFailure(DeleteObjectRequest request, ClientException clientExcepion, ServiceException serviceException) {
        // Handle exceptions.
        if (clientExcepion != null) {
            // Handle client exceptions, such as network exceptions.
            clientExcepion.printStackTrace();
        }
        if (serviceException != null) {
            // Handle server exceptions.
            Log.e("ErrorCode", serviceException.getErrorCode());
            Log.e("RequestId", serviceException.getRequestId());
            Log.e("HostId", serviceException.getHostId());
            Log.e("RawMessage", serviceException.getRawMessage());
        }
    }
}); 
DeleteObjectRequest delete = new DeleteObjectRequest("examplebucket", "exampleobject.txt");
// Delete the object asynchronously. 
OSSAsyncTask deleteTask = oss.asyncDeleteObject(delete, new OSSCompletedCallback<DeleteObjectRequest, DeleteObjectResult>() {
    @Override
    public void onSuccess(DeleteObjectRequest request, DeleteObjectResult result) {
        Log.d("asyncDeleteObject", "success!");
    }

    @Override
    public void onFailure(DeleteObjectRequest request, ClientException clientExcepion, ServiceException serviceException) {
        // Handle exceptions. 
        if (clientExcepion != null) {
            // Handle client exceptions, such as network exceptions.
            clientExcepion.printStackTrace();
        }
        if (serviceException != null) {
            // Handle server exceptions.
            Log.e("ErrorCode", serviceException.getErrorCode());
            Log.e("RequestId", serviceException.getRequestId());
            Log.e("HostId", serviceException.getHostId());
            Log.e("RawMessage", serviceException.getRawMessage());
        }
    }
});
```

## Delete multiple objects in a batch

You can batch delete up to 1,000 objects each time.

The result can be returned in the following two modes. Select a return mode based on your requirements.

-   verbose: If isQuiet is not specified or is set to false, a list of all deleted objects is returned. This is the default return mode.
-   quiet: If isQuiet is set to true, a list of objects that failed to be deleted is returned.

The following code provides an example on how to delete multiple specified objects from a bucket named examplebucket and return the result in the quiet mode:

```
// Specify the full paths of the objects that you want to delete. The full paths of the objects cannot contain bucket names. 
List<String> objectKeys = new ArrayList<String>();
objectKeys.add("exampleobject.txt");
objectKeys.add("testfolder/sampleobject.txt");

// Set the return mode to quiet to return a list of objects that failed to be deleted. 
DeleteMultipleObjectRequest request = new DeleteMultipleObjectRequest("examplebucket", objectKeys, true);

mOss.asyncDeleteMultipleObject(request, new OSSCompletedCallback<DeleteMultipleObjectRequest, DeleteMultipleObjectResult>() {
    @Override
    public void onSuccess(DeleteMultipleObjectRequest request, DeleteMultipleObjectResult result) {
        Log.i("DeleteMultipleObject", "success");
    }

    @Override
    public void onFailure(DeleteMultipleObjectRequest request, ClientException clientException, ServiceException serviceException) {
        // Handle exceptions. 
        if (clientException != null) {
            // Handle client exceptions, such as network exceptions. 
            clientException.printStackTrace();
        }
        if (serviceException != null) {
            // Handle server exceptions.
            Log.e("ErrorCode", serviceException.getErrorCode());
            Log.e("RequestId", serviceException.getRequestId());
            Log.e("HostId", serviceException.getHostId());
            Log.e("RawMessage", serviceException.getRawMessage());
        }
    }
});
```

