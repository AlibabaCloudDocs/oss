# Delete buckets

A bucket is a container for objects stored in OSS. Every object is contained in a bucket. This topic describes how to delete a bucket.

**Note:**

-   Before you delete a bucket, you must delete all objects in the bucket, LiveChannel objects, and fragments generated by multipart uploads. For more information about deleting a LiveChannel object, see [LiveChannel](/intl.en-US/API Reference/LiveChannel-related operations/DeleteLiveChannel.md).
-   To delete the fragments generated by multipart uploads, use [Bucket.ListMultipartUploads](/intl.en-US/API Reference/Object operations/Multipart upload/ListMultipartUploads.md) to list all the fragments, and then use [Bucket.AbortMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/AbortMultipartUpload.md) to delete the fragments.

The following code provides an example on how to delete a bucket named examplebucket:

```
OSSDeleteBucketRequest * delete = [OSSDeleteBucketRequest new];
delete.bucketName = @"examplebucket";
OSSTask * deleteTask = [client deleteBucket:delete];
[deleteTask continueWithBlock:^id(OSSTask *task) {
    if (!task.error) {
        NSLog(@"delete bucket success!");
    } else {
        NSLog(@"delete bucket failed, error: %@", task.error);
    }
    return nil;
}];
```

For more information about how to delete buckets, see [DeleteBucket](/intl.en-US/API Reference/Bucket operations/Basic operations/DeleteBucket.md).

