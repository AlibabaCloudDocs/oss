# Data security

OSS SDK for iOS provides data integrity checks to ensure data security in uploads and downloads.

## Data integrity checks in uploads

Due to the complex environment of mobile networks, an error may occur when data is transferred between the client and server. OSS provides end-to-end data integrity checks based on MD5 hashes and 64-bit CRC values.

-   MD5 verification

    If you specify the Content-MD5 header in the request when you upload an object, OSS performs MD5 verification on the object to ensure data integrity. The object is uploaded only when the MD5 hash of the received object is consistent with the Content-MD5 value specified in the request.

    ```
    OSSPutObjectRequest * request = [OSSPutObjectRequest new];
    request.bucketName = BUCKET_NAME;
    ...
    request.contentMd5 = [base64Md5ForFilePath];                    
    ```

-   CRC verification

    The 64-bit CRC value of an object can be calculated when the object is being uploaded. The following code provides an example on how to perform CRC when you upload an object:

    ```
    // Construct a request to upload the object.
    OSSPutObjectRequest * request = [OSSPutObjectRequest new];
    request.bucketName = BUCKET_NAME;
    ///....
    request.crcFlag = OSSRequestCRCOpen;
    // Enable CRC verification.
    OSSTask * task = [_client putObject:request];
    [[task continueWithBlock:^id(OSSTask *task) {
        // If the data is inconsistent during upload, the CRC verification fails and the OSSClientErrorCodeInvalidCRC error is returned.
        XCTAssertNil(task.error);
        return nil;
    }] waitUntilFinished];
    ```


## Data integrity checks in downloads

OSS SDK for iOS provides end-to-end data integrity checks based on 64-bit CRC values in downloads.

If CRC verification is enabled, OSS automatically checks data integrity after it reads data from a stream.

The following code provides an example on how to enable CRC verification in downloads:

```
OSSGetObjectRequest * request = [OSSGetObjectRequest new];
request.bucketName = BUCKET_NAME;
// Enable CRC verification.
request.crcFlag = OSSRequestCRCOpen;

OSSTask * task = [testProxyClient getObject:request];

[[task continueWithBlock:^id(OSSTask *task) {
    // If an error occurs during transmission after CRC verification is enabled, OSSClientErrorCodeInvalidCRC is returned.
    XCTAssertNil(task.error);
    return nil;
}] waitUntilFinished];

// If the onReceiveData block is set, you must check whether the CRC value is the same.
OSSGetObjectRequest * request = [OSSGetObjectRequest new];
request.bucketName = BUCKET_NAME;
request.crcFlag = OSSRequestCRCOpen;
....
    
__block uint64_t localCrc64 = 0;    
NSMutableData *receivedData = [NSMutableData data];
request.onRecieveData = ^(NSData *data) {
    if (data)
    {
        NSMutableData *mutableData = [data mutableCopy];
        void *bytes = mutableData.mutableBytes;
        localCrc64 = [OSSUtil crc64ecma:localCrc64 buffer:bytes length:data.length];
        [receivedData appendData:data];
    }
};
    
__block uint64_t remoteCrc64 = 0;
OSSTask * task = [_client getObject:request];
[[task continueWithBlock:^id(OSSTask *task) {
    XCTAssertNil(task.error);
       OSSGetObjectResult *result = task.result;
    if (result.remoteCRC64ecma) 
    {
        NSScanner *scanner = [NSScanner scannerWithString:result.remoteCRC64ecma];
        [scanner scanUnsignedLongLong:&remoteCrc64];
        if (remoteCrc64 == localCrc64)
        {
            NSLog(@ "crc64 verification successful.") ;
        }
        else
        {
            NSLog(@"crc64 verification failed!") ;
        }
   }
   return nil;
}] waitUntilFinished];            
```

