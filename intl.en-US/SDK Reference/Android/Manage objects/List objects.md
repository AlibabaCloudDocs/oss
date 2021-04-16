# List objects

This topic describes how to list objects in a bucket, including all objects, a specified number of objects, and objects whose names contain a specified prefix.

## List a specified number of objects

The following code provides an example on how to list 20 objects in a bucket named examplebucket:

```
// Specify the bucket name.
ListObjectsRequest request = new ListObjectsRequest("examplebucket");
// Specify the maximum number of returned objects. By default, the value of this parameter is set to 100. The maximum value you can specify is 1000. 
request.setMaxKeys(20);

oss.asyncListObjects(request, new OSSCompletedCallback<ListObjectsRequest, ListObjectsResult>() {
    @Override
    public void onSuccess(ListObjectsRequest request, ListObjectsResult result) {
        for (OSSObjectSummary objectSummary : result.getObjectSummaries()) {
            Log.i("ListObjects", objectSummary.getKey());
        }
    }

    @Override
    public void onFailure(ListObjectsRequest request, ClientException clientException, ServiceException serviceException) {
        // Handle request exceptions.
        if (clientException != null) {
            // Handle client-side exceptions such as network errors.
            clientException.printStackTrace();
        }
        if (serviceException != null) {
            // Handle server-side exceptions.
            Log.e("ErrorCode", serviceException.getErrorCode());
            Log.e("RequestId", serviceException.getRequestId());
            Log.e("HostId", serviceException.getHostId());
            Log.e("RawMessage", serviceException.getRawMessage());
        }
    }
});
```

## List objects whose names contain a specified prefix

The following code provides an example on how to list objects whose names contain the "file" prefix in a bucket named examplebucket:

```
// Specify the bucket name.
ListObjectsRequest request = new ListObjectsRequest("examplebucket");
// Specify the prefix.
request.setPrefix("file");

oss.asyncListObjects(request, new OSSCompletedCallback<ListObjectsRequest, ListObjectsResult>() {
    @Override
    public void onSuccess(ListObjectsRequest request, ListObjectsResult result) {
        for (OSSObjectSummary objectSummary : result.getObjectSummaries()) {
            Log.i("ListObjects", objectSummary.getKey());
        }
    }

    @Override
    public void onFailure(ListObjectsRequest request, ClientException clientException, ServiceException serviceException) {
        // Handle request exceptions.
        if (clientException != null) {
            // Handle client-side exceptions such as network errors.
            clientException.printStackTrace();
        }
        if (serviceException != null) {
            // Handle server-side exceptions.
            Log.e("ErrorCode", serviceException.getErrorCode());
            Log.e("RequestId", serviceException.getRequestId());
            Log.e("HostId", serviceException.getHostId());
            Log.e("RawMessage", serviceException.getRawMessage());
        }
    }
});
```

## List objects whose names are alphabetically after than the object specified by marker

The following code provides an example on how to list objects whose names are alphabetically after the object named exampleobject.txt in a bucket named examplebucket:

```
// Specify the bucket name.
ListObjectsRequest request = new ListObjectsRequest("examplebucket");
// Specify the value of marker. Objects whose names are alphabetically after the object specified by marker are returned.
// If the object specified by marker does not exist in the bucket, objects whose names are alphabetically after the value of marker are returned.
request.setMarker("exampleobject.txt");

oss.asyncListObjects(request, new OSSCompletedCallback<ListObjectsRequest, ListObjectsResult>() {
    @Override
    public void onSuccess(ListObjectsRequest request, ListObjectsResult result) {
        for (OSSObjectSummary objectSummary : result.getObjectSummaries()) {
            Log.i("ListObjects", objectSummary.getKey());
        }
    }

    @Override
    public void onFailure(ListObjectsRequest request, ClientException clientException, ServiceException serviceException) {
        // Handle request exceptions.
        if (clientException != null) {
            // Handle client-side exceptions such as network errors.
            clientException.printStackTrace();
        }
        if (serviceException != null) {
            // Handle server-side exceptions.
            Log.e("ErrorCode", serviceException.getErrorCode());
            Log.e("RequestId", serviceException.getRequestId());
            Log.e("HostId", serviceException.getHostId());
            Log.e("RawMessage", serviceException.getRawMessage());
        }
    }
});
```

## List all objects by page

The following code provides an example on how to list all objects in a bucket named examplebucket by page. Up to 20 objects can be listed on each page.

```
private String marker = null;
private boolean isCompleted = false;

// List all objects in the bucket by page.
public void getAllObject() {
    do {
        OSSAsyncTask task = getObjectList();
        // Wait until NextMarker is returned for the preceding request. Set the marker parameter in the current request to the value of NextMarker returned in the response to the preceding request. You do not need to set marker in the first request.
        // In this example, a looping function is used to list objects by page. Therefore, a request can be sent only after the NextMarker value for the preceding request is returned. You can determine whether to wait for the NextMarker value returned for the preceding request.
        task.waitUntilFinished();
    } while (!isCompleted);
}

// List objects in a page.
public OSSAsyncTask getObjectList() {
    // Specify the bucket name.
    ListObjectsRequest request = new ListObjectsRequest("examplebucket");
    // Specify the maximum number of objects that can be listed on each page. By default, the value of this parameter is set to 100. The maximum value you can specify is 1000.
    request.setMaxKeys(20);
    request.setMarker(marker);

    OSSAsyncTask task = oss.asyncListObjects(request, new OSSCompletedCallback<ListObjectsRequest, ListObjectsResult>() {
        @Override
        public void onSuccess(ListObjectsRequest request, ListObjectsResult result) {
            for (OSSObjectSummary objectSummary : result.getObjectSummaries()) {
                Log.i("ListObjects", objectSummary.getKey());
            }
            // List objects in the last page.
            if (!result.isTruncated()) {
                isCompleted = true;
                return;
            }
            // Obtain the marker value for the next request.
            marker = result.getNextMarker();
        }

        @Override
        public void onFailure(ListObjectsRequest request, ClientException clientException, ServiceException serviceException) {
            isCompleted = true;
            // Handle request exceptions.
            if (clientException != null) {
                // Handle client-side exceptions such as network errors.
                clientException.printStackTrace();
            }
            if (serviceException != null) {
                // Handle server-side exceptions.
                Log.e("ErrorCode", serviceException.getErrorCode());
                Log.e("RequestId", serviceException.getRequestId());
                Log.e("HostId", serviceException.getHostId());
                Log.e("RawMessage", serviceException.getRawMessage());
            }
        }
    });
    return task;
}
```

## List objects whose names contain a specified prefix by page

The following code provides an example on how to list objects whose names contain the "file" prefix in a bucket named examplebucket by page. Up to 20 objects can be listed on each page.

```
private String marker = null;
private boolean isCompleted = false;

// List all objects by page,
public void getAllObject() {
    do {
        OSSAsyncTask task = getObjectList();
        // Wait until NextMarker is returned for the preceding request. Set the marker parameter in the current request to the value of NextMarker returned in  the preceding request. You do not need to set marker in the first request.
        // In this example, a looping function is used to list objects by page. Therefore, a request can be sent only after the NextMarker value for the preceding request is returned. You can determine whether to wait for the NextMarker value returned for the preceding request.
        task.waitUntilFinished();
    } while (!isCompleted);
}

// List objects in a page.
public OSSAsyncTask getObjectList() {
    // Specify the bucket name.
    ListObjectsRequest request = new ListObjectsRequest("examplebucket");
    // Specify the maximum number of objects that can be listed on each page. By default, the value of this parameter is set to 100. The maximum value you can specify is 1000.
    request.setMaxKeys(20);
    // Specify the prefix.
    request.setPrefix("file");
    request.setMarker(marker);

    OSSAsyncTask task = oss.asyncListObjects(request, new OSSCompletedCallback<ListObjectsRequest, ListObjectsResult>() {
        @Override
        public void onSuccess(ListObjectsRequest request, ListObjectsResult result) {
            for (OSSObjectSummary objectSummary : result.getObjectSummaries()) {
                Log.i("ListObjects", objectSummary.getKey());
            }
            // List objects in the last page.
            if (!result.isTruncated()) {
                isCompleted = true;
                return;
            }
            // Obtains the marker value for the next request.
            marker = result.getNextMarker();
        }

        @Override
        public void onFailure(ListObjectsRequest request, ClientException clientException, ServiceException serviceException) {
            isCompleted = true;
            // Handle request exceptions.
            if (clientException != null) {
                // Handle client-side exceptions such as network errors.
                clientException.printStackTrace();
            }
            if (serviceException != null) {
                // Handle server-side exceptions.
                Log.e("ErrorCode", serviceException.getErrorCode());
                Log.e("RequestId", serviceException.getRequestId());
                Log.e("HostId", serviceException.getHostId());
                Log.e("RawMessage", serviceException.getRawMessage());
            }
        }
    });
    return task;
}
```

## List objects whose names contain special characters

If the name of an object contains one of the following special characters, you must encode the object name before you transmit the object. Only URL encoding is supported in OSS.

-   Single quotation marks \('\)
-   Double quotations marks \("\)
-   Ampersands \(&\)
-   Angle brackets \(<\>\)
-   Chinese characters

The following code provides an example on how to list objects whose names contain special characters in a bucket named examplebucket:

```
// Specify the bucket name.
ListObjectsRequest request = new ListObjectsRequest("examplebucket");
// Specify the encoding type of the object names.
request.setEncodingType("url");

oss.asyncListObjects(request, new OSSCompletedCallback<ListObjectsRequest, ListObjectsResult>() {
    @Override
    public void onSuccess(ListObjectsRequest request, ListObjectsResult result) {
        for (OSSObjectSummary objectSummary : result.getObjectSummaries()) {
            Log.i("ListObjects", URLDecoder.decode(objectSummary.getKey(), "UTF-8"));
        }
    }

    @Override
    public void onFailure(ListObjectsRequest request, ClientException clientException, ServiceException serviceException) {
        // Handle request exceptions.
        if (clientException != null) {
            // Handle client-side exceptions such as network errors.
            clientException.printStackTrace();
        }
        if (serviceException != null) {
            // Handle server-side exceptions.
            Log.e("ErrorCode", serviceException.getErrorCode());
            Log.e("RequestId", serviceException.getRequestId());
            Log.e("HostId", serviceException.getHostId());
            Log.e("RawMessage", serviceException.getRawMessage());
        }
    }
});
```

For more information about how to list objects, see [GetBucket \(ListObjects\)](/intl.en-US/API Reference/Bucket operations/Basic operations/GetBucket (ListObjects).md).

