# Determine whether buckets exist

A bucket is a container for objects stored in OSS. Every object is contained in a bucket. This topic describes how to determine whether a bucket exists.

The following code provides an example on how to call `Client#bucket_exists?` to determine whether a bucket exists:

```
require 'aliyun/oss'

client = Aliyun::OSS::Client.new(
  endpoint: 'endpoint',
  access_key_id: 'AccessKeyId', access_key_secret: 'AccessKeySecret')

puts client.bucket_exists?(' my-bucket')
        
```

