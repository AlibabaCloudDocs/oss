# Streaming download

If you have a large object that takes a long time to download in its entirety, you can use streaming download to download the object incrementally until the object is downloaded in full.

OSS SDK for iOS does not provide any streaming download APIs. Instead, it provides the multipart callback feature that is similar to the `didRecieveData` function in the `NSURLSession` library. Note: The download result does not contain the actual data if multipart callback is configured.

The following code provides an example on how to perform streaming download:

```
OSSGetObjectRequest * request = [OSSGetObjectRequest new];
// Configure the required fields. objectKey is equivalent to objectName and indicates the complete path of the object that you want to download from OSS. The path must include the file extension of the object. For example, you can set objectKey to abc/efg/123.jpg.
request.bucketName = @"<bucketName>";
request.objectKey = @"<objectKey>";
// Configure the multipart callback function.
request.onRecieveData = ^(NSData * data) {
    NSLog(@"Recieve data, length: %ld", [data length]);
};
OSSTask * getTask = [client getObject:request];
[getTask continueWithBlock:^id(OSSTask *task) {
    if (! task.error) {
        NSLog(@"download object success!") ;
    } else {
        NSLog(@"download object failed, error: %@" ,task.error);
    }
    return nil;
}];
// [getTask waitUntilFinished];
// [request cancel];
```

