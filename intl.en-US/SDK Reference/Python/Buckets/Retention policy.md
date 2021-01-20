# Retention policy

You can configure a time-based retention policy for a bucket. A retention policy has a retention period that ranges from one day to 70 years. This topic describes how to create, query, and lock a retention policy.

## Background information

OSS supports the Write Once Read Many \(WORM\) strategy that prevents an object from being deleted or overwritten for a specified period of time.

If a retention policy is not locked within 24 hours after it is created, the retention policy becomes invalid. After the retention policy configured for a bucket is locked, you can upload objects to or read objects from the bucket. However, you cannot delete objects in the bucket or the retention policy within the retention period of the policy. The retention period of the policy cannot be shortened but only be extended. For more information about retention policies, see [Retention policy](/intl.en-US/Developer Guide/Data security/Retention policy.md).

## Create a retention policy

The following code provides an example on how to create a retention policy:

```
# -*- coding: utf-8 -*-
import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Create the retention policy and set the retention period to 1 days.
result = bucket.init_bucket_worm(1)
# Query the ID of the retention policy.
print(result.worm_id)
```

For more information about how to create a retention policy, see [InitiateBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/InitiateBucketWorm.md).

## Cancel an unlocked retention policy

The following code provides an example on how to cancel an unlocked retention policy:

```
# -*- coding: utf-8 -*-
import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Cancel the unlocked retention policy.
bucket.abort_bucket_worm()
```

For more information about how to cancel an unlocked retention policy, see [AbortBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/AbortBucketWorm.md).

## Lock a retention policy

The following code provides an example on how to lock a retention policy:

```
# -*- coding: utf-8 -*-
import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Lock the retention policy.
bucket.complete_bucket_worm('<yourWromId>')
```

For more information about how to lock a retention policy, see [CompleteBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/CompleteBucketWorm.md).

## Query a retention policy

The following code provides an example on how to query a retention policy:

```
# -*- coding: utf-8 -*-
import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Query the retention policy.
result = self.bucket.get_bucket_worm()

# Query the ID of the retention policy.
print(result.worm_id)
# Query the status of the retention policy. InProgress indicates that the retention policy is not locked. Locked indicates that the retention policy is locked.
print(result.state)
# Query the retention period of the retention policy.
print(result.retention_period_days)
# Query the created time of the retention policy.
print(result.creation_date)
```

For more information about how to query a retention policy, see [GetBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/GetBucketWorm.md).

## Extend the retention period of a retention policy

The following code provides an example on how to extend the retention period of a locked retention policy:

```
# -*- coding: utf-8 -*-
import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Extend the retention period of the locked retention policy.
bucket.extend_bucket_worm('<yourWormId>', 2)
```

For more information about how to extend the retention period of a locked retention policy, see [ExtendBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/ExtendBucketWorm.md).

