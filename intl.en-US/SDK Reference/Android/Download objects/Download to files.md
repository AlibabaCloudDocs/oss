# Download to files

This topic describes how to download an object to a file.

The following code provides an example on how to download an object to a specified local file.

```
// Download the object.
// objectKey is equivalent to objectName and indicates the complete path of the object that you want to download from OSS. The path must include the file extension of the object. For example, you can set objectKey to abc/efg/123.jpg.
GetObjectRequest get = new GetObjectRequest("BucketName", "objectKey");

oss.asyncGetObject(get, new OSSCompletedCallback<GetObjectRequest, GetObjectResult>() {
    @Override
    public void onSuccess(GetObjectRequest request, GetObjectResult result) {
// Read the object content.
        long length = result.getContentLength();
        byte[] buffer = new byte[(int) length];
        int readCount = 0;
        while (readCount < length) {
            try{
                readCount += result.getObjectContent().read(buffer, readCount, (int) length - readCount);
            }catch (Exception e){
                OSSLog.logInfo(e.toString());
            }
        }
// Store the downloaded object in the specified file path.
        try {
            FileOutputStream fout = new FileOutputStream("download_filePath");
            fout.write(buffer);
            fout.close();
        } catch (Exception e) {
            OSSLog.logInfo(e.toString());
        }
    }

    @Override
    public void onFailure(GetObjectRequest request, ClientException clientException,
                          ServiceException serviceException)  {

    }
});
```

