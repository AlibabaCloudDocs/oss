# Manage lifecycle rules

OSS allows you to configure lifecycle rules to delete expired objects and parts or convert the storage class of expired objects to IA or Archive. This way, storage costs are minimized. This topic describes how to manage lifecycle rules.

## Background information

Each lifecycle rule contains the following information:

-   Policy: specifies the mode to match objects and parts.
    -   Match by prefix: matches objects and parts by prefix. You can create multiple rules to configure different prefixes. Each prefix must be unique.
    -   Match by tag: matches objects by tag key and tag value. You can configure multiple tags for a single rule. OSS runs lifecycle rules for all objects that have these tags. Tags cannot be configured for parts to match lifecycle rules.

        **Note:** For more information, see [Object tagging](/intl.en-US/Developer Guide/Objects/Manage files/Configure object tagging.md).

    -   Match by prefix and tag: matches objects by specifying a prefix and one or more tags.
    -   Match by bucket: matches all objects and parts contained in the bucket. If you select this method, only a single rule can be created.
-   Object lifecycle policy: specifies the validity period or expiration date and the operation to perform on these objects when they expire.

    -   Validity period: specifies the number of days for unversioned and current objects and the operation to perform on these objects. Objects that meet the specified conditions are retained for the specified period of time after the objects are last modified. The specified operation is performed on these objects when they expire.
    -   Expiration date: specifies a date for unversioned and current objects and the operation to perform on these objects. All objects that are last modified before this date expire and the specified operation is performed on these objects.
    -   Validity period for the previous versions of objects: specifies the number of days for the previous versions of objects and the operation to perform on these objects. Objects that meet the specified conditions are retained for the specified period of time after the objects become previous versions. The specified operation is performed on these objects when they expire.
    **Note:** You can specify that the storage class of objects is converted to IA or Archive, or delete the objects when they expire. For more information, see [Configuration elements](/intl.en-US/Developer Guide/Objects/Object lifecycle/Configuration elements.md).

-   Part lifecycle policy: specifies the validity period or expiration date and the operation to perform on these parts when they expire.
    -   Validity period: specifies the number of days for which to retain parts after they are last modified and the delete operation to perform on these parts when they expire.
    -   Expiration date: specifies a date based on which to delete parts. Parts that are last modified before this date expire.

You can also configure lifecycle rules for parts that are uploaded to a bucket using uploadPart. The last modification time uses the time the multipart upload task is initiated.

For more information, see [Manage object lifecycles](/intl.en-US/Developer Guide/Objects/Object lifecycle/Lifecycle rules.md).

## Configure lifecycle rules

The following code provides an example on how to configure lifecycle rules:

```
using Aliyun.OSS;
using Aliyun.OSS.Common;
var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var bucketName = "<yourBucketName>";
// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    var setBucketLifecycleRequest = new SetBucketLifecycleRequest(bucketName);
    // Create the first lifecycle rule.
    LifecycleRule lcr1 = new LifecycleRule()
    {
        ID = "delete obsoleted files",
        Prefix = "obsoleted/",
        Status = RuleStatus.Enabled,
        ExpriationDays = 3,
        Tags = new Tag[1]
    };
    // Specify the tags.
    var tag1 = new Tag
    {
        Key = "project",
        Value = "projectone"
    };

    lcr1.Tags[0] = tag1;

    // Create the second lifecycle rule.
    LifecycleRule lcr2 = new LifecycleRule()
    {
        ID = "delete temporary files",
        Prefix = "temporary/",
        Status = RuleStatus.Enabled,
        ExpriationDays = 20,
        Tags = new Tag[1]         
    };
    // Specify the tags.
    var tag2 = new Tag
    {
        Key = "user",
        Value = "jsmith"
    };
    lcr2.Tags[0] = tag2;

    LifecycleRule lcr3 = new LifecycleRule();
    lcr3.ID = "only NoncurrentVersionTransition";
    lcr3.Prefix = "test1";
    lcr3.Status = RuleStatus.Enabled;
    lcr3.NoncurrentVersionTransitions = new LifecycleRule.LifeCycleNoncurrentVersionTransition[2]
    {
        // Specify that the storage class of objects is converted to Infrequent Access (IA) 90 days after they become previous versions.
        new LifecycleRule.LifeCycleNoncurrentVersionTransition(){
            StorageClass = StorageClass.IA,
            NoncurrentDays = 90
        },
        // Specify that the storage class of objects is converted to Archive 180 days after they become previous versions.
        new LifecycleRule.LifeCycleNoncurrentVersionTransition(){
            StorageClass = StorageClass.Archive,
            NoncurrentDays = 180
        }
    };
    setBucketLifecycleRequest.AddLifecycleRule(lcr1);
    setBucketLifecycleRequest.AddLifecycleRule(lcr2);
    setBucketLifecycleRequest.AddLifecycleRule(lcr3);

    // Configure the lifecycle rules.
    client.SetBucketLifecycle(setBucketLifecycleRequest);
    Console.WriteLine("Set bucket:{0} Lifecycle succeeded ", bucketName);
}
catch (OssException ex)
{
    Console.WriteLine("Failed with error code: {0}; Error info: {1}. \nRequestID:{2}\tHostID:{3}",
        ex.ErrorCode, ex.Message, ex.RequestId, ex.HostId);
}
catch (Exception ex)
{
    Console.WriteLine("Failed with error info: {0}", ex.Message);
}

            
```

## View lifecycle rules

The following code provides an example on how to view the lifecycle rules configured for a bucket:

```
using Aliyun.OSS;
using Aliyun.OSS.Common;
var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var bucketName = "<yourBucketName>";
// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    // View the lifecycle rules.
    var rules = client.GetBucketLifecycle(bucketName);
    Console.WriteLine("Get bucket:{0} Lifecycle succeeded ", bucketName);
    foreach (var rule in rules)
    {
        Console.WriteLine("ID: {0}", rule.ID);
        Console.WriteLine("Prefix: {0}", rule.Prefix);
        Console.WriteLine("Status: {0}", rule.Status);
        // View tags.
        foreach (var tag in rule.Tags)
        {
            Console.WriteLine("key:{0}, value:{1}", tag.Key, tag.Value);
        }
        // View the expiration configurations for the previous versions of the objects.
        foreach (var version in rule.NoncurrentVersionTransitions)
        {
            Console.WriteLine("expiration day:{0}, storage class:{1}", version.NoncurrentDays, version.StorageClass);
        }
        if (rule.ExpriationDays.HasValue)
            Console.WriteLine("ExpirationDays: {0}", rule.ExpriationDays);
    }
}
catch (OssException ex)
{
    Console.WriteLine("Failed with error code: {0}; Error info: {1}. \nRequestID:{2}\tHostID:{3}",
        ex.ErrorCode, ex.Message, ex.RequestId, ex.HostId);
}
catch (Exception ex)
{
    Console.WriteLine("Failed with error info: {0}", ex.Message);
}
```

## Delete lifecycle rules

The following code provides an example on how to delete the lifecycle rules configured for a bucket:

```
using Aliyun.OSS;
using Aliyun.OSS.Common;
var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var bucketName = "<yourBucketName>";
// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    // Delete the lifecycle rules.
    client.DeleteBucketLifecycle(bucketName);
    Console.WriteLine("Delete bucket:{0} Lifecycle succeeded ", bucketName);
}
catch (OssException ex)
{
    Console.WriteLine("Failed with error code: {0}; Error info: {1}. \nRequestID:{2}\tHostID:{3}",
        ex.ErrorCode, ex.Message, ex.RequestId, ex.HostId);
}
catch (Exception ex)
{
    Console.WriteLine("Failed with error info: {0}", ex.Message);
}
```

