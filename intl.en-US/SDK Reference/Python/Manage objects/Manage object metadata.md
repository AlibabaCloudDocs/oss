# Manage object metadata

Object metadata includes HTTP headers and user metadata.

For more information, see [Manage object metadata](/intl.en-US/Developer Guide/Objects/Manage files/Manage object metadata.md).

## Configure HTTP headers

Run the following code to configure HTTP headers:

```
# -*- coding: utf-8 -*-
import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

bucket.put_object('<yourObjectName>', '{"age": 1}', headers={'Content-Type': 'application/json; charset=utf-8'})
            
```

For more information about HTTP headers, see [RFC2616](https://tools.ietf.org/html/rfc2616).

## Configure user metadata

Run the following code to configure user metadata:

```
# -*- coding: utf-8 -*-

import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

bucket.put_object('<yourObjectName>', 'a novel', headers={'x-oss-meta-author': 'O. Henry', 'Content-Type': 'application/json; charset=utf-8'})

            
```

**Note:** OSS uses parameters prefixed with x-oss-meta- as headers for user metadata. For more information, see [PutObject](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.md).

## Modify object metadata

Run the following code to modify object metadata:

```
# -*- coding: utf-8 -*-
import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

bucket.update_object_meta('<yourObjectName>', {'x-oss-meta-author': 'O. Henry'})
# Each time you call bucket.update_object_meta, the existing user metadata is updated.
bucket.update_object_meta('<yourObjectName>', {'x-oss-meta-price': '100 dollar'})
            
```

The following code provides an example on how to modify object metadata such as Content-Type:

```
bucket.update_object_meta('<yourObjectName>', {'x-oss-meta-author': 'O. Henry'})
# Each time you call bucket.update_object_meta, the existing user metadata is updated.
bucket.update_object_meta('<yourObjectName>', {'Content-Type': 'text/plain'})
            
```

## Query object metadata

You can use the operations provided by OSS SDK for Python to query object metadata.

|Method|Description|Advantage|
|:-----|:----------|:--------|
|get\_object\_meta|Queries part of object metadata such as the ETag, Size, and LastModified \(the last modified time\) values of the object.|More lightweight and faster|
|head\_object|Queries all object metadata.|Not supported|

Run the following code to query object metadata:

```
# -*- coding: utf-8 -*-

import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

// Query parts of the object metadata.
simplifiedmeta = bucket.get_object_meta("<yourObjectName>")
print(simplifiedmeta.headers['Last-Modified']) 
print(simplifiedmeta.headers['Content-Length']) 
print(simplifiedmeta.headers['ETag']) 

// Query all object metadata.
objectmeta = bucket.head_object("<yourObjectName>")
print(objectmeta.headers['Content-Type']) 
print(objectmeta.headers['Last-Modified']) 
print(objectmeta.headers['x-oss-object-type'])
            
```

