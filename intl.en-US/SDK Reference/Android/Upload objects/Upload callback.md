# Upload callback

This topic describes how to use the upload callback function.

After a file is uploaded to OSS, OSS can return the callback information to the application server. To get the callback information, you can add callback-related parameters in the upload request. Note that it takes more time for the client to wait for the response than in simple upload tasks if the callback function is enabled because OSS takes time to respond the callback request.

The following code enables the upload callback function:

```language-java
PutObjectRequest put = new PutObjectRequest(testBucket, testObject, uploadFilePath);

put.setCallbackParam(new HashMap<String, String>() {
    {
        put("callbackUrl", "110.75.82.106/callback");
        put("callbackHost", "yourCallbackHost");
        put("callbackBodyType", "application/json");
        put("callbackBody", "{\"mimeType\":${mimeType},\"size\":${size}}");
    }
});

OSSAsyncTask task = oss.asyncPutObject(put, new OSSCompletedCallback<PutObjectRequest, PutObjectResult>() {
    @Override
    public void onSuccess(PutObjectRequest request, PutObjectResult result) {
        Log.d("PutObject", "UploadSuccess");

        // This parameter has value only when the servercallback parameter is set.
        String serverCallbackReturnJson = result.getServerCallbackReturnBody();

        Log.d("servercallback", serverCallbackReturnJson);
    }

    @Override
    public void onFailure(PutObjectRequest request, ClientException clientExcepion, ServiceException serviceException) {
        // Handle exceptions.
    }
});
		
```

If you want to configure custom parameters in the callback information, see the following code:

```
put.setCallbackParam(new HashMap<String, String>() {
    {
        put("callbackUrl", "http://182.92.192.125/leibin/notify.php");
        put("callbackHost", "yourCallbackHost");
        put("callbackBodyType", "application/json");
        put("callbackBody", "{\"object\":${object},\"size\":${size},\"my_var1\":${x:var1},\"my_var2\":${x:var2}}");
    }
});

put.setCallbackVars(new HashMap<String, String>() {
    {
        put("x:var1", "value1");
        put("x:var2", "value2");
    }
});
		
```

For more information about the callback function, see [Callback](/intl.en-US/API Reference/Object operations/Basic operations/Callback.md).

