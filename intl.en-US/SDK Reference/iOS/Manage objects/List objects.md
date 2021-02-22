# List objects

This topic describes how to list all objects in a bucket.

Use the following code to list all objects in a bucket:

```
OSSGetBucketRequest * getBucket = [OSSGetBucketRequest new];
getBucket.bucketName = @"<bucketName>";

// Set optional parameters.
// getBucket.marker = @"";
// getBucket.maxKeys = @"";
// getBucket.prefix = @"";
// getBucket.delimiter = @"";

OSSTask * getBucketTask = [client getBucket:getBucket];

[getBucketTask continueWithBlock:^id(OSSTask *task) {
    if (! task.error) {
        OSSGetBucketResult * result = task.result;
        NSLog(@"get bucket success!") ;
        for (NSDictionary * objectInfo in result.contents) {
            NSLog(@"list object: %@", objectInfo);
        }
    } else {
        NSLog(@"get bucket failed, error: %@", task.error);
    }
    return nil;
}];
        
```

The following table lists the configurable parameters and their descriptions returned by the list operation:

|Name|Description|
|:---|:----------|
|delimiter|The delimiter used to separate object names. CommonPrefixes specify a set of substrings that start with a specified prefix and end with the first specified delimiter.|
|marker|The initial object in the list.|
|maxkeys|The maximum number of objects that can be listed. The default value is 100. The maximum value is 1,000.|
|prefix|The prefix that must be included in the returned objects. Note that if you use a prefix to make a query, the returned key values still contain the prefix.|

For more information about optional parameters, see [GetBucket \(ListObjects\)](/intl.en-US/API Reference/Bucket operations/Basic operations/GetBucket (ListObjects).md).

