# Manage symbolic links

This topic describes how to create a symbolic link and obtain the name of the object to which the symbolic link points.

## Create a symbolic link

A symbolic link is a special object that points to an object. It is similar to a shortcut used in Windows. You can use a symbolic link to customize object metadata.

The following code provides an example on how to create a symbolic link named examplesymlink that points to an object named exampleobject.txt in a bucket named examplebucket:

```
OSSPutSymlinkRequest *request = [OSSPutSymlinkRequest new];
// Specify the bucket name.
request.bucketName = @"examplebucket";
// Specify the name of the symbolic link that you want to create.
request.objectKey = @"examplesymlink";
// Specify the full path of the object to which the symbolic link points. The full path of the object cannot contain bucket names.
request.targetObjectName = @"exampleobject.txt";

OSSTask *putSymlinkTask = [client putSymlink:request];
[putSymlinkTask continueWithBlock:^id _Nullable(OSSTask * _Nonnull task) {
    if (!task.error) {
        NSLog(@"put symlink success");
    } else {
        NSLog(@"put symlink failed, error: %@", task.error);
    }
    return nil;
}];
```

## Query the name of the object to which a symbolic link points

To query the object to which a symbolic link points, you must have read permissions on the symbolic link.

The following code is used to obtain the name of the target file pointed to by the soft link examplesymlink in the examplebucket bucket.

```
OSSGetSymlinkRequest *request = [OSSGetSymlinkRequest new];
// Specify the bucket name.
request.bucketName = @"examplebucket";
// Specify the name of the symbolic link that you want to create.
request.objectKey = @"examplesymlink";

OSSTask *getSymlinkTask = [client getSymlink:request];
[getSymlinkTask continueWithBlock:^id _Nullable(OSSTask * _Nonnull task) {
    if (!task.error) {
        OSSGetSymlinkResult *result = task.result;
        NSLog(@"get symlink: %@", result.httpResponseHeaderFields[@"x-oss-symlink-target"]);
    } else {
        NSLog(@"get symlink failed, error: %@", task.error);
    }
    return nil;
}];
```

For more information about obtaining soft links, see [GetSymlink](/intl.en-US/API Reference/Object operations/Symbolic link/GetSymlink.md).

