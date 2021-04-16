# Restore Archive objects

You must restore an Archive object before you read the object. This topic describes how to restore an Archive object.

The status of an archived object throughout the restoration process is described as follows:

1.  By default, the archived object is in the frozen state.
2.  After you submit a restore request, the object is in the restoring state. It takes about one minute to complete the restore task.
3.  After the server completes the restore task, the object enters the restored state and you can read the object. By default, the restored state lasts 24 hours and can be extended by another 24 hours after you call RestoreObject again. During one restoration process, RestoreObject can be called on an object for up to seven times. In other words, an object can remain in the restored state for up to seven days.
4.  After the restored state expires, the object returns to the frozen state.

The following code provides an example on how to restore an Archive object named exampleobject.txt:

```
OSSRestoreObjectRequest *request = [OSSRestoreObjectRequest new];
// Specify the bucket name.
request.bucketName = @"examplebucket";
// Specify the full path of the Archive object. The full path of the object cannot contain bucket names.
request.objectKey = @"exampleobject.txt";

OSSTask *restoreObjectTask = [client restoreObject:request];
[restoreObjectTask continueWithBlock:^id _Nullable(OSSTask * _Nonnull task) {
    if (!task.error) {
        NSLog(@"restore object success");
    } else {
        NSLog(@"restore object failed, error: %@", task.error);
    }
    return nil;
}];
```

For more information about the Archive storage class, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md). For more information about the parameters that you can configure when you restore an Archive object, see [RestoreObject](/intl.en-US/API Reference/Object operations/Basic operations/RestoreObject.md).

