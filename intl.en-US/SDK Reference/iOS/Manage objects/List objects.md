# List objects

This topic describes how to list objects in a bucket, including all objects, a specified number of objects, and objects whose names contain a specified prefix.

**Note:** For more information about how to list objects, see [GetBucket \(ListObjects\)](/intl.en-US/API Reference/Bucket operations/Basic operations/GetBucket (ListObjects).md).

## List a specified number of objects

The following code provides an example on how to list 20 objects in a bucket named examplebucket:

```
OSSGetBucketRequest * getBucket = [OSSGetBucketRequest new];
// Specify the bucket name. 
getBucket.bucketName = @"examplebucket";
// Specify the maximum number of returned objects. By default, the value of this parameter is set to 100. The maximum value you can specify is 1000. 
getBucket.maxKeys = 20;

OSSTask * getBucketTask = [client getBucket:getBucket];
[getBucketTask continueWithBlock:^id(OSSTask *task) {
    if (!task.error) {
        OSSGetBucketResult * result = task.result;
        NSLog(@"get bucket success!");
        for (NSDictionary * objectInfo in result.contents) {
            NSLog(@"list object: %@", objectInfo);
        }
    } else {
        NSLog(@"get bucket failed, error: %@", task.error);
    }
    return nil;
}];
```

## List objects whose names contain a specified prefix

The following code provides an example on how to list objects whose names contain the "file" prefix in a bucket named examplebucket:

```
OSSGetBucketRequest * getBucket = [OSSGetBucketRequest new];
// Specify the bucket name. 
getBucket.bucketName = @"examplebucket";
// Specify the prefix. 
getBucket.prefix = @"file";

OSSTask * getBucketTask = [client getBucket:getBucket];
[getBucketTask continueWithBlock:^id(OSSTask *task) {
    if (!task.error) {
        OSSGetBucketResult * result = task.result;
        NSLog(@"get bucket success!");
        for (NSDictionary * objectInfo in result.contents) {
            NSLog(@"list object: %@", objectInfo);
        }
    } else {
        NSLog(@"get bucket failed, error: %@", task.error);
    }
    return nil;
}];
```

## List objects whose names are alphabetically after the object specified by marker

The following code provides an example on how to list objects whose names are alphabetically after the object named exampleobject.txt in a bucket named examplebucket:

```
OSSGetBucketRequest * getBucket = [OSSGetBucketRequest new];
// Specify the bucket name. 
getBucket.bucketName = @"examplebucket";
// Specify the value of marker. Objects whose names are alphabetically after the object specified by marker are returned. 
// If the object specified by marker does not exist in the bucket, objects whose names are alphabetically after the value of marker are returned. 
getBucket.marker = @"exampleobject.txt";

OSSTask * getBucketTask = [client getBucket:getBucket];
[getBucketTask continueWithBlock:^id(OSSTask *task) {
    if (!task.error) {
        OSSGetBucketResult * result = task.result;
        NSLog(@"get bucket success!");
        for (NSDictionary * objectInfo in result.contents) {
            NSLog(@"list object: %@", objectInfo);
        }
    } else {
        NSLog(@"get bucket failed, error: %@", task.error);
    }
    return nil;
}];
```

## List all objects by page

The following code provides an example on how to list all objects in a bucket named examplebucket by page. Up to 20 objects can be listed on each page.

```
interface ... {
    NSString *_marker;
    BOOL _isCompleted;
}
@end

@implementation ...

// List all objects by page.
- (void)getAllObjects {
    do {
        OSSTask *task = [self getObjectList];
        // Wait until NextMarker is returned for the preceding request. Set the marker parameter in the current request to the value of NextMarker returned in the response to the preceding request. You do not need to set marker in the first request.
        // In this example, a loop function is used to list objects by page. Therefore, a request can be sent only after the NextMarker value for the preceding request is returned. You can determine whether to wait for the NextMarker value returned for the preceding request.
        [task waitUntilFinished];
    } while (!_isCompleted);
}

//  List objects on a page.
- (OSSTask *)getObjectList {
    OSSGetBucketRequest * getBucket = [OSSGetBucketRequest new];
    // Specify the bucket name.
    getBucket.bucketName = @"examplebucket";
    // Specify the maximum number of objects that can be listed on each page. By default, the value of this parameter is set to 100. The maximum value you can specify is 1000.
    getBucket.maxKeys = 20;
    // marker is a global attribute.
    getBucket.marker = self.marker;

    OSSTask * getBucketTask = [client getBucket:getBucket];
    [getBucketTask continueWithBlock:^id(OSSTask *task) {
        if (!task.error) {
            OSSGetBucketResult * result = task.result;
            NSLog(@"get bucket success!");
            for (NSDictionary * objectInfo in result.contents) {
                NSLog(@"list object: %@", objectInfo);
            }
            if (result.isTruncated) {
                _marker = result.nextMarker;
            } else {
                _isCompleted = YES;
            }
        } else {
            _isCompleted = YES;
            NSLog(@"get bucket failed, error: %@", task.error);
        }
        return nil;
    }];
    return getBucketTask;
}
@end

@implementation ...

// List all objects by page. 
- (void)getAllObjects {
    do {
        OSSTask *task = [self getObjectList];
        // Wait until NextMarker is returned for the preceding request. Set the marker parameter in the current request to the value of NextMarker returned in the response to the preceding request. You do not need to set marker in the first request. 
        // In this example, a loop function is used to list objects by page. Therefore, a request can be sent only after the NextMarker value for the preceding request is returned. You can determine whether to wait for the NextMarker value returned for the preceding request. 
        [task waitUntilFinished];
    } while (!_isCompleted);
}

// List objects on a page.
- (OSSTask *)getObjectList {
    OSSGetBucketRequest * getBucket = [OSSGetBucketRequest new];
    // Specify the bucket name. 
    getBucket.bucketName = @"examplebucket";
    // Specify the maximum number of objects that can be listed on each page. By default, the value of this parameter is set to 100. The maximum value you can specify is 1000. 
    getBucket.maxKeys = 20;
    // marker is a global attribute. 
    getBucket.marker = self.marker;

    OSSTask * getBucketTask = [client getBucket:getBucket];
    [getBucketTask continueWithBlock:^id(OSSTask *task) {
        if (!task.error) {
            OSSGetBucketResult * result = task.result;
            NSLog(@"get bucket success!");
            for (NSDictionary * objectInfo in result.contents) {
                NSLog(@"list object: %@", objectInfo);
            }
            if (result.isTruncated) {
                _marker = result.nextMarker;
            } else {
                _isCompleted = YES;
            }
        } else {
            _isCompleted = YES;
            NSLog(@"get bucket failed, error: %@", task.error);
        }
        return nil;
    }];
    return getBucketTask;
}
@end
```

## List objects whose names contain a specified prefix by page

The following code provides an example on how to list objects whose names contain the "file" prefix in a bucket named examplebucket by page. Up to 20 objects can be listed on each page.

```
@interface ... {
    NSString *_marker;
    BOOL _isCompleted;
}
@end

@implementation ...

// List all objects by page. 
- (void)getAllObjects {
    do {
        OSSTask *task = [self getObjectList];
        // Wait until NextMarker is returned for the preceding request. Set the marker parameter in the current request to the value of NextMarker returned in the response to the preceding request. You do not need to set marker in the first request. 
        // In this example, a loop function is used to list objects by page. Therefore, a request can be sent only after the NextMarker value for the preceding request is returned. You can determine whether to wait for the NextMarker value returned for the preceding request. 
        [task waitUntilFinished];
    } while (!_isCompleted);
}

// List objects on a page. 
- (OSSTask *)getObjectList {
    OSSGetBucketRequest * getBucket = [OSSGetBucketRequest new];
    // Specify the bucket name. 
    getBucket.bucketName = @"examplebucket";
    // Specify the maximum number of objects that can be listed on each page. By default, the value of this parameter is set to 100. The maximum value you can specify is 1000. 
    getBucket.maxKeys = 20;
    // Specify the prefix.
    getBucket.prefix = @"file";
    // marker is a global attribute. 
    getBucket.marker = self.marker;

    OSSTask * getBucketTask = [client getBucket:getBucket];
    [getBucketTask continueWithBlock:^id(OSSTask *task) {
        if (!task.error) {
            OSSGetBucketResult * result = task.result;
            NSLog(@"get bucket success!");
            for (NSDictionary * objectInfo in result.contents) {
                NSLog(@"list object: %@", objectInfo);
            }
            if (result.isTruncated) {
                _marker = result.nextMarker;
            } else {
                _isCompleted = YES;
            }
        } else {
            _isCompleted = YES;
            NSLog(@"get bucket failed, error: %@", task.error);
        }
        return nil;
    }];
    return getBucketTask;
}
@end
```

