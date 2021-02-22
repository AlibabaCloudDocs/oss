# Data security

OSS Android SDK provides methods to verify data integrity, which ensure data security when you uploading, downloading, and copying objects.

Due to the complex network environment for mobile devices, errors may occur when data is transmitted between the client and the OSS server. Therefore, OSS Android SDK provides two methods to verify data integrity: CRC end-to-end verification and MD5 verification.

-   CRC verification

    If you enable CRC verification when reading the download stream, data integrity is automatically verified after the stream is completely read.

    Run the following code to enable CRC verification:

    ```
    GetObjectRequest request = new GetObjectRequest(OSSTestConfig.ANDROID_TEST_BUCKET, testFile);
    // Enable CRC verification.
    request.setCRC64(OSSRequest.CRC64Config.YES);
    //....
    try{
        GetObjectResult result = oss.getObject(request);
        InputStream in = result.getObjectContent();
        ByteArrayOutputStream output = new ByteArrayOutputStream();
        byte[] buffer = new byte[BUFFER_SIZE];
        int len;
        while ((len = in.read(buffer)) > -1) {
                output.write(buffer, 0, len);
        }
        output.flush();
        in.close();
    }catch(ClientException e){
        //...
    }catch(InconsistentException e){
        //....
    }
                        
    ```

-   MD5 verification

    To check whether the object uploaded in the multipart upload method is the same as the local file, you can upload the parts with their Content-MD5 values. The OSS server checks the MD5 values of the uploaded parts and determines the upload task is successful only when the MD5 values are the same as the specified Content-MD5 values, ensuring that the uploaded object is the same as the local file.

    Run the following code to enable MD5 verification:

    ```
    UploadPartRequest uploadPart = new UploadPartRequest("<bucketName>", "<objectKey>", uploadId, currentIndex); 
    // Configure the part content.
    uploadPart.setPartContent(partData);  
    // Configure the MD5 value.
    uploadPart.setMd5Digest(BinaryUtil.calculateBase64Md5(data));
    ```


