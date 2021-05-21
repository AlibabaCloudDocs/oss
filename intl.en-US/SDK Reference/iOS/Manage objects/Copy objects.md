# Copy objects

You can copy an object from a source bucket to a destination bucket within the same region.

Use the following code to copy an object:

```
OSSCopyObjectRequest * copy = [OSSCopyObjectRequest new];
copy.bucketName = @<bucketName>;
// objectKey is equivalent to objectName and indicates the complete path of the object that you want to copy from the source bucket. The path must include the file extension of the object. For example, you can set objectKey to abc/efg/123.jpg.
copy.objectKey = @"<objectKey>";
copy.sourceCopyFrom = [NSString stringWithFormat:@"/%@/%@", @"<bucketName>, @"<objectKey_copyFrom>"];

OSSTask * task = [client copyObject:copy];

[task continueWithBlock:^id(OSSTask *task) {
    if (! task.error) {
        // ...
    }
    return nil;s
}];
		
```

**Note:**

-   The source and destination objects must belong to the same region.
-   During the copy operation, if the source and destination objects have the same IP address, you can modify the Object Meta of the existing objects.
-   You cannot use CopyObject to copy an object whose size exceeds 1 GB. To copy an object larger than 1 GB, use multipart upload.

