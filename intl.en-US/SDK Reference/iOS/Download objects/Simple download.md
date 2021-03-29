# Simple download

This topic describes how to use simple download.

You can download an object to a local file or as NSData:

```
OSSGetObjectRequest * request = [OSSGetObjectRequest new];

// Configure required fields.
request.bucketName = @"<bucketName>";
request.objectKey = @"<objectKey>";

// Configure optional fields.
request.downloadProgress = ^(int64_t bytesWritten, int64_t totalBytesWritten, int64_t totalBytesExpectedToWrite) {
    // The length of the segment being downloaded, the total length of the segments downloaded, and the total length of the segments to be downloaded.
    NSLog(@"%lld, %lld, %lld", bytesWritten, totalBytesWritten, totalBytesExpectedToWrite);
};
// request.range = [[OSSRange alloc] initWithStart:0 withEnd:99]; // Download the foremost 99 Bytes.
// request.downloadToFileURL = [NSURL fileURLWithPath:@"<filepath>"]; // Specify the destination path if you need to directly download the object to a file.

OSSTask * getTask = [client getObject:request];

[getTask continueWithBlock:^id(OSSTask *task) {
    if (! task.error) {
        NSLog(@"download object success!") ;
        OSSGetObjectResult * getResult = task.result;
        NSLog(@"download result: %@", getResult.downloadedData);
    } else {
        NSLog(@"download object failed, error: %@" ,task.error);
    }
    return nil;
}];

// [getTask waitUntilFinished];

// [request cancel];
        
```

