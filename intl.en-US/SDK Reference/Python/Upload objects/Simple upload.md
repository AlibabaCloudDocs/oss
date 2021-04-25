# Simple upload

You can use the `put_object` operation to upload a single object. You can use simple upload to upload the following types of data: strings, bytes, Unicode characters, network streams, and local files.

**Note:** For more information about how to perform simple upload, see [PutObject](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.md).

## Usage notes

If you upload an object whose name is the same as that of an accessible existing object, the existing object is overwritten by the uploaded object.

The following table lists the common parameters that you must configure when you upload an object.

|Parameter|Description|
|---------|-----------|
|bucket\_name|The bucket name. The bucket name must comply with the following conventions:

-   The name can contain only lowercase letters, digits, and hyphens \(-\).
-   The name must start and end with a lowercase letter or digit.
-   The name must be 3 to 63 bytes in length. |
|object\_name|The full path of the uploaded object. The full path of the object cannot contain bucket names. The object name must comply with the following conventions:

-   The name can contain only UTF-8 characters.
-   The name must be 1 to 1,023 bytes in length.
-   The name cannot start with a forward slash \(/\) or a backslash \(\\\). |

## Upload strings

The following code provides an example on how to upload a string to an object named exampleobject.txt in a bucket named examplebucket:

```
# -*- coding: utf-8 -*-
import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations.
auth = oss2.Auth('yourAccessKeyId', 'yourAccessKeySecret')
# Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com.
# Specify the bucket name.
bucket = oss2.Bucket(auth, 'yourEndpoint', 'examplebucket')

# Upload an object.
# To specify the storage class and access control list (ACL) of the uploaded object, specify the x-oss-storage-class and x-oss-object-acl headers in put_object.
# headers = dict()
# headers["x-oss-storage-class"] = "Standard"
# headers["x-oss-object-acl"] = oss2.OBJECT_ACL_PRIVATE
# Specify the full path of the object and the string to upload. The full path of the object cannot contain bucket names.
# result = bucket.put_object('exampleobject.txt', 'Hello OSS', headers=headers)
result = bucket.put_object('exampleobject.txt', 'Hello OSS')

# Display the returned HTTP status code.
print('http status:  {0}'.format(result.status))
# Display the request ID. A request ID uniquely identifies a request. We recommend that you add this parameter to logs.
print('request_id:  {0}'.format(result.request_id))
# Display the ETag value returned by the put_object method. The ETag value of an object is used to identify the object content.
print('ETag:  {0}'.format(result.etag))
# Display the HTTP response headers.
print('date: {0}'.format(result.headers['date']))                       
```

## Upload bytes

The following code provides an example on how to upload bytes to an object named exampleobject.txt in a bucket named examplebucket:

```
# -*- coding: utf-8 -*-

import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
auth = oss2.Auth('yourAccessKeyId', 'yourAccessKeySecret')
# Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
# Specify the bucket name. 
bucket = oss2.Bucket(auth, 'yourEndpoint', 'examplebucket')

# Specify the full path of the object and the bytes to upload. The full path of the object cannot contain bucket names. 
bucket.put_object('exampleobject.txt', b'Hello OSS')            
```

## Upload Unicode characters

The following code provides an example on how to upload Unicode characters to an object named exampleobject.txt in a bucket named examplebucket. When you upload Unicode characters, OSS converts the characters into UTF-8-encoded bytes.

```
# -*- coding: utf-8 -*-
import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
auth = oss2.Auth('yourAccessKeyId', 'yourAccessKeySecret')
# Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
# Specify the bucket name. 
bucket = oss2.Bucket(auth, 'yourEndpoint', 'examplebucket')

# Specify the full path of the object and the Unicode characters to upload. The full path of the object cannot contain bucket names.
bucket.put_object('exampleobject.txt', u'Hello OSS')            
```

## Upload network streams

The following code provides an example on how to upload a network stream to an object named exampleobject.txt in a bucket named examplebucket. OSS uploads network streams as iterable objects by using chunked encoding.

```
# -*- coding: utf-8 -*-
import oss2
import requests
# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
auth = oss2.Auth('yourAccessKeyId', 'yourAccessKeySecret')
# Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
# Specify the bucket name. 
bucket = oss2.Bucket(auth, 'yourEndpoint', 'examplebucket')

# The requests.get method returns an iterable object. OSS SDK for Python uploads the object by using chunked encoding. 
# Specify the address of the network stream. 
input = requests.get('http://www.aliyun.com')
# Specify the full path of the object. The full path of the object cannot contain bucket names.
bucket.put_object('exampleobject.txt', input)            
```

## Upload local files

The following code provides an example on how to upload a local file named examplefile.txt as an object in a bucket named examplebucket. OSS uploads local files as file objects and opens the files in binary mode, such as the rb mode, during upload.

```
# -*- coding: utf-8 -*-
import oss2
import os
# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
auth = oss2.Auth('yourAccessKeyId', 'yourAccessKeySecret')
# Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
# Specify the bucket name.  
bucket = oss2.Bucket(auth, 'yourEndpoint', 'examplebucket')

# The object must be opened in binary mode. 
# Specify the full path of the local file. If the path of the local file is not specified, the file is uploaded to the path of the project to which the sample program belongs. 
with open('D:\\localpath\\examplefile.txt', 'rb') as fileobj:
    # Use the seek method to read data from byte 1000 of the file. The data is uploaded from the byte 1000 of the local file until the entire file is uploaded. 
    fileobj.seek(1000, os.SEEK_SET)
    # Use the tell method to obtain the current position. 
    current = fileobj.tell()
    # Specify the full path of the object to which the local file is uploaded. The full path of the object cannot contain bucket names.
    bucket.put_object('exampleobject.txt', fileobj)        
```

OSS SDK for Python also provides a more convenient method to upload local files. The following code provides an example of this method:

```
# Specify the full paths of the local file and object. The full path of the object cannot contain bucket names. 
# If the path of the local file is not specified, the file is uploaded to the path of the project to which the sample program belongs. 
bucket.put_object_from_file('exampleobject.txt', 'D:\\localpath\\examplefile.txt')            
```

