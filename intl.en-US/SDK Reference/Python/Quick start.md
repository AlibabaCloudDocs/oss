# Quick start

This topic describes how to use OSS SDK for Python to perform routine operations such as create buckets, upload objects, and download objects.

## Create buckets

A bucket is a global namespace in OSS. A bucket is a container for objects stored in OSS. The following code provides an example on how to create a bucket:

```
# -*- coding: utf-8 -*-
import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Set the ACL of the bucket to private.
bucket.create_bucket(oss2.models.BUCKET_ACL_PRIVATE)
            
```

For more information about bucket naming conventions, see the "Naming conventions" section in [Terms](/intl.en-US/Developer Guide/Terms.md). For more information about how to create a bucket, see [Create buckets](/intl.en-US/Console User Guide/Manage buckets/Create buckets.md).

For more information about endpoints, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).

## Upload objects

The following code provides an example on how to upload an object to OSS:

```
# -*- coding: utf-8 -*-
import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# <yourObjectName> indicates the complete path of the object you want to upload to OSS, and must include the extension of the object name. Example: abc/efg/123.jpg.
# <yourLocalFile> consists of a local file path and an object name with extension. Example: /users/local/myfile.txt.
bucket.put_object_from_file('<yourObjectName>', '<yourLocalFile>')
            
```

## Download objects

The following code provides an example on how to download a specified object to a file:

```
# -*- coding: utf-8 -*-
import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
# <yourObjectName> indicates the complete path of the object you want to download from OSS, and must include the extension of the object name. Example: abc/efg/123.jpg.
# <yourLocalFile> consists of a local file path and an object name with an extension. Example: /users/local/myfile.txt.
bucket.get_object_to_file('<yourObjectName>', '<yourLocalFile>')
            
```

## List objects

The following code provides an example on how to list 10 objects in a specified bucket:

```
# -*- coding: utf-8 -*-
import oss2
from itertools import islice

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# oss2.ObjectIteratorr is used to traverse objects.
for b in islice(oss2.ObjectIterator(bucket), 10):
    print(b.key)
            
```

## Delete objects

The following code provides an example on how to delete an object:

```
# -*- coding: utf-8 -*-
import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# <yourObjectName> indicates the complete path of the object you want to delete from OSS, and must include the extension of the object name. Example: abc/efg/123.jpg.
bucket.delete_object('<yourObjectName>')
            
```

For more information about how to delete objects, see [Delete objects](/intl.en-US/SDK Reference/Python/Manage objects/Delete objects.md).

