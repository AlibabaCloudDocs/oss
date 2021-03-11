# IMG

OSS Image Processing \(IMG\) is a secure, cost-effective, and highly reliable image processing service that can process large amounts of data. After you upload source images to OSS, you can call RESTful API operations to process the images anytime, anywhere, and on any Internet device.

For more information about the IMG parameters, see [Parameters](/intl.en-US/Developer Guide/Data Processing/Image Processing/Overview.md).

## Usage

-   Anonymous access

    ```
    String url = oss.presignPublicObjectURL(testBucket, testObject);
            OSSLog.logDebug("signPublicURL", "get url: " + url);
    Add the x-oss-process:operation parameter to the generated URL, in which operation indicates the operation performed on the image.
    ```

-   Authorized access

    When you process images by using OSS SDKs, you can call the `request.setxOssProcess()` operation to configure IMG parameters in the following code. Examples:

    ```
    // Construct an image download request.
    GetObjectRequest get = new GetObjectRequest("<bucketName>", "example.jpg");
    
    // Process the image.
    request.setxOssProcess("image/resize,m_fixed,w_100,h_100");
    
    OSSAsyncTask task = oss.asyncGetObject(get, new OSSCompletedCallback<GetObjectRequest, GetObjectResult>() {
        @Override
        public void onSuccess(GetObjectRequest request, GetObjectResult result) {
            // The request is successful.
            InputStream inputStream = result.getObjectContent();
    
            byte[] buffer = new byte[2048];
            int len;
    
            try {
                while ((len = inputStream.read(buffer)) ! = -1) {
                    // Process the downloaded data.
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    
        @Override
        public void onFailure(GetObjectRequest request, ClientException clientExcepion, ServiceException serviceException) {
            // Handle the exceptions. You can add your own code.
        }
    });
    ```

    **Note:** To perform other operations on the image, replace the parameters of `request.setxOssProcess()`.

-   Access by using SDKs

    ```
    GetObjectRequest request = new GetObjectRequest("bucket-name", "image-name");
    request.setxOssProcess("image/resize,m_lfit,w_100,h_100");  // Configure IMG parameters.
    OSSAsyncTask task = ossClient.asyncGetObject(request, getCallback);
    ```


## IMG persistence

The following code provides an example on how to save processed images:

```
// fromBucket indicates the name of the source bucket. toBucket indicates the name of the destination bucket.
// fromObjectKey indicates the names of the source object. toObjectkey indicates the destination object. Their names must be a complete path including the file suffix. Example: abc/efg/123.jpg.
// action indicates the IMG operation.
ImagePersistRequest request = new ImagePersistRequest(fromBucket,fromObjectKey,toBucket,toObjectkey,action);

        OSSAsyncTask task = mOss.asyncImagePersist(request, new OSSCompletedCallback<ImagePersistRequest, ImagePersistResult>() {
            @Override
            public void onSuccess(ImagePersistRequest request, ImagePersistResult result) {
                // sucess callback
            }

            @Override
            public void onFailure(ImagePersistRequest request, ClientException clientException, ServiceException serviceException) {
                  // errror callback
            }
        });
```

