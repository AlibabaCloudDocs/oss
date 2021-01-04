# Versioning

The versioning state of a bucket applies to all of the objects in the bucket. Versioning allows you to restore objects in a bucket to any previous point in time, and protects your data from being accidentally overwritten or deleted.

A bucket can be in any one of the following versioning states: unversioned \(default\), versioning-enabled, or versioning-suspended. For more information about versioning, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md) in OSS Developer Guide.

## Configure versioning for a bucket

The following code provides an example on how to configure versioning for a bucket to Enabled or Suspended:

```
# -*- coding: utf-8 -*-
import oss2
from oss2.models import BucketVersioningConfig

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create your RAM user, log on to the RAM console.
# This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Initialize the versioning configurations for the bucket.
config = BucketVersioningConfig()
# Configure the versioning state to Enabled or Suspended.
config.status = oss2.BUCKET_VERSIONING_ENABLE

# Configure versioning for the bucket.
result = bucket.put_bucket_versioning(config)
# View the HTTP status code.
print('http response status:', result.status)
```

For more information about how to configure versioning for a bucket, see [PutBucketVersioning](/intl.en-US/API Reference/Bucket operations/Versioning/PutBucketVersioning.md).

## Obtain the versioning state of a bucket

The following code provides an example on how to obtain the versioning state of a bucket:

```
# -*- coding: utf-8 -*-
import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create your RAM user, log on to the RAM console.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Obtain the versioning state of the bucket.
versioning_info = bucket.get_bucket_versioning()
# View the versioning state of the bucket. If versioning has been enabled, Enabled or Suspended is returned. If versioning has not been enabled, None is returned.
print('bucket versioning status:', versioning_info.status)
```

For more information about how to obtain the versioning state of a bucket, see [GetBucketVersioning](/intl.en-US/API Reference/Bucket operations/Versioning/GetBucketVersioning.md).

