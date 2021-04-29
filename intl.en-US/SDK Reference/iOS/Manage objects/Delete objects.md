# Delete objects

This topic describes how to delete a single object or batch delete objects.

**Warning:** Deleted objects cannot be recovered. Exercise caution when you delete objects.

## Usage notes

To delete an object, you must have write permissions on the bucket in which the object is stored.

## Delete a single object

The following code provides an example on how to delete an object named exampleobject.txt from a bucket named examplebucket:

```
OSSDeleteObjectRequest * delete = [OSSDeleteObjectRequest new];
// Specify the bucket name. 
delete.bucketName = @"examplebucket";
// Specify the full path of the object that you want to delete. The full path of the object cannot contain bucket names. 
delete.objectKey = @"exampleobject.txt";

OSSTask * deleteTask = [client deleteObject:delete];

[deleteTask continueWithBlock:^id(OSSTask *task) {
    if (!task.error) {
        // ...
    }
    return nil;
}];

// [deleteTask waitUntilFinished];
        
```

## Delete multiple objects in a batch

You can batch delete up to 1,000 objects each time.

The result can be returned in the following two modes. Select a return mode based on your requirements.

-   verbose: If quiet is set to NO, a list of all deleted objects is returned.
-   quiet: If quiet is not specified or is set to YES, a list of objects that failed to be deleted is returned. This is the default return mode.

The following code provides an example on how to delete multiple specified objects from a bucket named examplebucket and return the result in the quiet mode:

```
OSSDeleteMultipleObjectsRequest *request = [OSSDeleteMultipleObjectsRequest new];
// Specify the bucket name. 
request.bucketName = @"examplebucket";
// Specify the full paths of the objects that you want to delete. The full paths of the objects cannot contain bucket names. 
request.keys = @[@"exampleobject.txt", @"testfolder/sampleobject.txt"];
// Set quiet to YES to return only a list of objects that failed to be deleted. 
request.quiet = YES;

OSSTask * deleteMultipleObjectsTask = [client deleteMultipleObjects:request];
[deleteMultipleObjectsTask continueWithBlock:^id(OSSTask *task) {
    if (!task.error) {
        OSSDeleteMultipleObjectsResult *result = task.result;
        NSLog(@"delete objects: %@", result.deletedObjects);
    } else {
        NSLog(@"delete objects failed, error: %@", task.error);
    }
    return nil;
}];
```

