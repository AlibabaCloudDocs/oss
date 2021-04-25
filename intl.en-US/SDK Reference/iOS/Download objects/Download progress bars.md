# Download progress bars

You can use a progress bar to indicate the progress of an object that is being uploaded or downloaded. The GetObject operation is used in the example to describe how to display the progress bar of an object that is being downloaded.

The following code provides an example on how to display the progress of an object named exampleobject.txt that is being downloaded from a bucket named examplebucket:

```
OSSGetObjectRequest * request = [OSSGetObjectRequest new];
// Specify the bucket name. 
request.bucketName = @"examplebucket";
// Specify the full path of the object that you want to download. The full path of the object cannot contain bucket names. 
request.objectKey = @"exampleobject.txt";
// Configure the callback function to display the progress bar. 
request.downloadProgress = ^(int64_t bytesWritten, int64_t totalBytesWritten, int64_t totalBytesExpectedToWrite) {
    // Specify the length of the segments that indicate the sizes of data being downloaded, data that is downloaded, and the total data to download. 
    NSLog(@"%lld, %lld, %lld", bytesWritten, totalBytesWritten, totalBytesExpectedToWrite);
};

OSSTask * getTask = [client getObject:request];
[getTask continueWithBlock:^id(OSSTask *task) {
    if (!task.error) {
        NSLog(@"download object success!");
    } else {
        NSLog(@"download object failed, error: %@" ,task.error);
    }
    return nil;
}];
```

