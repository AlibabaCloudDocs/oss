# List buckets

A bucket is a container for objects stored in Object Storage Service \(OSS\). Every object is contained in a bucket. This topic describes how to list buckets that belong to an Alibaba Cloud account, including all buckets, buckets whose names contain a specific prefix, and a specific number of buckets.

## List all buckets that belong to an Alibaba Cloud account

The following code provides an example on how to list all buckets that belong to an Alibaba Cloud account:

```
OSSGetServiceRequest * getService = [OSSGetServiceRequest new];
    
OSSTask * getServiceTask = [client getService:getService];
[getServiceTask continueWithBlock:^id(OSSTask *task) {
    if (!task.error) {
        OSSGetServiceResult * result = task.result;
        NSLog(@"buckets: %@", result.buckets);
        NSLog(@"owner: %@, %@", result.ownerId, result.ownerDispName);
        [result.buckets enumerateObjectsUsingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
            NSDictionary * bucketInfo = obj;
            NSLog(@"BucketName: %@", [bucketInfo objectForKey:@"Name"]);
            NSLog(@"CreationDate: %@", [bucketInfo objectForKey:@"CreationDate"]);
            NSLog(@"Location: %@", [bucketInfo objectForKey:@"Location"]);
        }];
    } else {
        NSLog(@"get service failed, error: %@", task.error);
    }
    return nil;
}];
```

## List buckets whose names contain a specific prefix

The following code provides an example on how to list buckets whose names contain the "bucket" prefix:

```
OSSGetServiceRequest * getService = [OSSGetServiceRequest new];
getService.prefix = @"bucket";

OSSTask * getServiceTask = [client getService:getService];
[getServiceTask continueWithBlock:^id(OSSTask *task) {
    if (!task.error) {
        OSSGetServiceResult * result = task.result;
        NSLog(@"buckets: %@", result.buckets);
        NSLog(@"owner: %@, %@", result.ownerId, result.ownerDispName);
        [result.buckets enumerateObjectsUsingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
            NSDictionary * bucketInfo = obj;
            NSLog(@"BucketName: %@", [bucketInfo objectForKey:@"Name"]);
            NSLog(@"CreationDate: %@", [bucketInfo objectForKey:@"CreationDate"]);
            NSLog(@"Location: %@", [bucketInfo objectForKey:@"Location"]);
        }];
    } else {
        NSLog(@"get service failed, error: %@", task.error);
    }
    return nil;
}];
```

## List buckets whose names are alphabetically after the bucket specified by marker

The marker parameter specifies a bucket name. Buckets whose names are alphabetically after the marker value are listed. The following code provides an example on how to list buckets whose names are alphabetically after examplebucket:

```
OSSGetServiceRequest * getService = [OSSGetServiceRequest new];
getService.marker = @"examplebucket";

OSSTask * getServiceTask = [client getService:getService];
[getServiceTask continueWithBlock:^id(OSSTask *task) {
    if (!task.error) {
        OSSGetServiceResult * result = task.result;
        NSLog(@"buckets: %@", result.buckets);
        NSLog(@"owner: %@, %@", result.ownerId, result.ownerDispName);
        [result.buckets enumerateObjectsUsingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
            NSDictionary * bucketInfo = obj;
            NSLog(@"BucketName: %@", [bucketInfo objectForKey:@"Name"]);
            NSLog(@"CreationDate: %@", [bucketInfo objectForKey:@"CreationDate"]);
            NSLog(@"Location: %@", [bucketInfo objectForKey:@"Location"]);
        }];
    } else {
        NSLog(@"get service failed, error: %@", task.error);
    }
    return nil;
}];
```

## List a specific number of buckets

The following code provides an example on how to list up to 20 buckets:

```
OSSGetServiceRequest * getService = [OSSGetServiceRequest new];
getService.maxKeys = 20;
    
OSSTask * getServiceTask = [client getService:getService];
[getServiceTask continueWithBlock:^id(OSSTask *task) {
    if (!task.error) {
        OSSGetServiceResult * result = task.result;
        NSLog(@"buckets: %@", result.buckets);
        NSLog(@"owner: %@, %@", result.ownerId, result.ownerDispName);
        [result.buckets enumerateObjectsUsingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
            NSDictionary * bucketInfo = obj;
            NSLog(@"BucketName: %@", [bucketInfo objectForKey:@"Name"]);
            NSLog(@"CreationDate: %@", [bucketInfo objectForKey:@"CreationDate"]);
            NSLog(@"Location: %@", [bucketInfo objectForKey:@"Location"]);
        }];
    } else {
        NSLog(@"get service failed, error: %@", task.error);
    }
    return nil;
}];
```

