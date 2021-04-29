# Upload progress bars

You can use a progress bar to indicate the progress of an object that is being uploaded or downloaded. The PutObject operation is used in the example to describe how to display the progress bar of an object that is being uploaded.

The following code provides an example on how to display the progress bar when you upload a local file named examplefile.txt as an object named exampleobject.txt in a bucket named examplebucket:

```
OSSPutObjectRequest * put = [OSSPutObjectRequest new];
// Specify the bucket name. 
put.bucketName = @"examplebucket";
// Specify the full path of the object. The full path of the object cannot contain bucket names. 
put.objectKey = @"exampleobject.txt";
// Specify the full path of the local file to upload. 
// If the path of the local file is not specified, the file is uploaded from the path of the project to which the sample program belongs. 
put.uploadingFileURL = [NSURL fileURLWithPath:@"D:\\localpath\\examplefile.txt"];
// Configure the callback function to display the progress bar. 
put.uploadProgress = ^(int64_t bytesSent, int64_t totalByteSent, int64_t totalBytesExpectedToSend) {
    // Specify the length of the segments that indicate the sizes of data being uploaded, data that is uploaded, and the total data to upload. 
    NSLog(@"%lld, %lld, %lld", bytesSent, totalByteSent, totalBytesExpectedToSend);
};
OSSTask * putTask = [client putObject:put];
[putTask continueWithBlock:^id(OSSTask *task) {
    if (!task.error) {
        NSLog(@"upload object success!");
    } else {
        NSLog(@"upload object failed, error: %@" , task.error);
    }
    return nil;
}];
```

