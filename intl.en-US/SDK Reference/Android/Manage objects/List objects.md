# List objects

This topic describes how to list all objects or objects with a specified prefix in a bucket.

## List all objects in a bucket

Run the following code to list all objects in a bucket:

```
ListObjectsRequest listObjects = new ListObjectsRequest("<bucketName>");
 
// Configure the callback information returned when the request is successful and fails. Send the list request by calling the asynchronous interface.
OSSAsyncTask task = oss.asyncListObjects(listObjects, new OSSCompletedCallback<ListObjectsRequest, ListObjectsResult>() {
	@Override
	public void onSuccess(ListObjectsRequest request, ListObjectsResult result) {
		Log.d("AyncListObjects", "Success!");
		for (int i = 0; i < result.getObjectSummaries().size(); i++) {
			Log.d("AyncListObjects", "object: " + result.getObjectSummaries().get(i).getKey() + " "
					+ result.getObjectSummaries().get(i).getETag() + " "
					+ result.getObjectSummaries().get(i).getLastModified());
		}
	}

	@Override
	public void onFailure(ListObjectsRequest request, ClientException clientExcepion, ServiceException serviceException) {
		// Handle the exceptions returned for the request.
		if (clientExcepion != null) {
			// A local exception (such as network exception) occurs.
			clientExcepion.printStackTrace();
		}
		if (serviceException != null) {
			// A service exception occurs.
			Log.e("ErrorCode", serviceException.getErrorCode());
			Log.e("RequestId", serviceException.getRequestId());
			Log.e("HostId", serviceException.getHostId());
			Log.e("RawMessage", serviceException.getRawMessage());
		}
	}
});
// Wait until the task is complete.
task.waitUntilFinished();
```

## List objects with a specified prefix in a bucket

Run the following code to list all object prefixed with "file" in a bucket:

```language-java
 ListObjectsRequest listObjects = new ListObjectsRequest("<bucketName>");
 
// Set the prefix.
listObjects.setPrefix("file");

// Configure the callback information returned when the request is successful and fails. Send the list request by calling the asynchronous interface.
OSSAsyncTask task = oss.asyncListObjects(listObjects, new OSSCompletedCallback<ListObjectsRequest, ListObjectsResult>() {
	@Override
	public void onSuccess(ListObjectsRequest request, ListObjectsResult result) {
		Log.d("AyncListObjects", "Success!");
		for (int i = 0; i < result.getObjectSummaries().size(); i++) {
			Log.d("AyncListObjects", "object: " + result.getObjectSummaries().get(i).getKey() + " "
					+ result.getObjectSummaries().get(i).getETag() + " "
					+ result.getObjectSummaries().get(i).getLastModified());
		}
	}

	@Override
	public void onFailure(ListObjectsRequest request, ClientException clientExcepion, ServiceException serviceException) {
		// Handle the exceptions returned for the request.
		if (clientExcepion != null) {
			// A local exception (such as network exception) occurs.
			clientExcepion.printStackTrace();
		}
		if (serviceException != null) {
			// A service exception occurs.
			Log.e("ErrorCode", serviceException.getErrorCode());
			Log.e("RequestId", serviceException.getRequestId());
			Log.e("HostId", serviceException.getHostId());
			Log.e("RawMessage", serviceException.getRawMessage());
		}
	}
});
task.waitUntilFinished();

```

The following table describes the parameters in the preceding code.

|Parameter|Description|
|:--------|:----------|
|delimiter|Specifies the character used to group object names. All objects whose name includes the specified prefix and the delimiter character for the first time are classified into a group of elements: CommonPrefixes.|
|marker|Indicates that objects with names after the marker in alphabetical order are returned.|
|maxkeys|Specifies the maximum number of the returned objects for the current request. The value of this parameter cannot be larger than 1,000.Default value: 100 |
|prefix|Indicates that the keys of the returned objects must be prefixed with the value of this parameter.**Note:** If you include the prefix in the query request, it is also included in the returned key. |

