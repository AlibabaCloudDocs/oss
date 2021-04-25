# Streaming download

If you need to download a large object or it takes a long time to download an object at a time, you can use streaming download to download the object in increments.

The following code provides an example on how to use streaming download to download an object named exampleobject.txt from a bucket named examplebucket:

```
# -*- coding: utf-8 -*-
import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
auth = oss2.Auth('yourAccessKeyId', 'yourAccessKeySecret')
# Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
# Specify the bucket name. 
bucket = oss2.Bucket(auth, 'yourEndpoint', 'examplebucket')

# The value returned by bucket.get_object is a file-like and iterable object. 
# Specify the full path of the object. The full path of the object cannot contain bucket names. 
object_stream = bucket.get_object('exampleobject.txt')
print(object_stream.read())

# You must call read() to read the object from the stream returned by get_object before you can calculate the CRC-64 value of the object. 
if object_stream.client_crc != object_stream.server_crc:
    print("The CRC checksum between client and server is inconsistent!")       
```

The following code provides an example on how to download the data of an object named exampleobject.txt from a stream to a file named examplefile.txt in the local directory D:\\localpath:

```
import shutil
import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
auth = oss2.Auth('yourAccessKeyId', 'yourAccessKeySecret')
# Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
# Specify the bucket name. 
bucket = oss2.Bucket(auth, 'yourEndpoint', 'examplebucket')

# object_stream is a file-like object. You can use the shutil.copyfileobj method to download the data from a stream to the local file. 
# Specify the full path of the object. The full path of the object cannot contain bucket names. 
object_stream = bucket.get_object('exampleobject.txt')
# Download the object to a local file in the specified path. If the specified local file exists, the downloaded object overwrites the local file. Otherwise, the local file is created. 
# If the path for the object is not specified, the downloaded object is saved to the path of the project to which the sample program belongs. 
with open('D:\\localpath\\examplefile.txt', 'wb') as local_fileobj:
    shutil.copyfileobj(object_stream, local_fileobj)        
```

The following code provides an example on how to copy the data of an object named exampleobject.txt from a stream to an object named exampleobjectnew.txt:

```
import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
auth = oss2.Auth('yourAccessKeyId', 'yourAccessKeySecret')
# Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
# Specify the bucket name. 
bucket = oss2.Bucket(auth, 'yourEndpoint', 'examplebucket')

# object_stream is an iterable object. You can copy the data of the object from a stream to another object in the same bucket. 
# Specify the full path of the source object. The full path of the object cannot contain bucket names. 
object_stream = bucket.get_object('exampleobject.txt')
# Specify the full path of the destination object. The full path of the object cannot contain bucket names. 
bucket.put_object('exampleobjectnew.txt', object_stream)       
```

