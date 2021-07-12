# Set logging

OSS allows you to set access logging for a bucket. After the configuration, accesses to the bucket are logged.

The logs are then stored in a specified bucket on OSS in the format of:

```
<TargetPrefix><SourceBucket>YYYY-mm-DD-HH-MM-SS-UniqueString
        
```

You must specify the `TargetPrefix`. A logging rule consists the following:

-   enable: Whether to enable access logging.
-   target\_bucket: The bucket where logs are stored.
-   target\_prefix: The prefix of the log object

For more information, see [Set access logging](/intl.en-US/Developer Guide/Manage logs/Log storage.md).

## Enable bucket logging

Use `Bucket#logging=` to enable logging:

```
require 'aliyun/oss'

client = Aliyun::OSS::Client.new(
  endpoint: 'endpoint',
  access_key_id: 'AccessKeyId', access_key_secret: 'AccessKeySecret')

bucket = client.get_bucket('my-bucket')

bucket.logging = BucketLogging.new(
  enable: true, target_bucket: 'logging_bucket', target_prefix: 'my-log')
            
```

## View bucket logging settings

Use `Bucket#logging` to view logging settings:

```
require 'aliyun/oss'

client = Aliyun::OSS::Client.new(
  endpoint: 'endpoint',
  access_key_id: 'AccessKeyId', access_key_secret: 'AccessKeySecret')

bucket = client.get_bucket('my-bucket')
log = bucket.logging
puts log.to_s
            
```

## Disable bucket logging

Use `Bucket#logging=` to disable logging.

```
require 'aliyun/oss'

client = Aliyun::OSS::Client.new(
  endpoint: 'endpoint',
  access_key_id: 'AccessKeyId', access_key_secret: 'AccessKeySecret')

bucket = client.get_bucket('my-bucket')
bucket.logging = BucketLogging.new(enable: false)
            
```

