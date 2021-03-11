# Signed URLs

OSS SDK for iOS supports the signed URLs that have a validity period or public URLs. This way, URLs can be forwarded to a third party for authorized access.

## Generate a signed URL that has validity period for private resources

If the bucket or object ACL is Private, you must call the following API operations to obtain the signed URL:

```
NSString * constrainURL = nil;

// sign constrain url
OSSTask * task = [client presignConstrainURLWithBucketName:@"<bucket name>"
                                             withObjectKey:@"<object key>"
                                    withExpirationInterval: 30 * 60];
if (!task.error) {
    constrainURL = task.result;
} else {
    NSLog(@"error: %@", task.error);
}
```

## Generate a signed public URL

If the bucket or object ACL is Public Read, you must call the following API operations to obtain the URL of the publicly accessible object:

```
NSString * publicURL = nil;

// sign public url
task = [client presignPublicURLWithBucketName:@"<bucket name>"
                                withObjectKey:@"<object key>"];
if (!task.error) {
    publicURL = task.result;
} else {
    NSLog(@"sign url error: %@", task.error);
}
            
```

