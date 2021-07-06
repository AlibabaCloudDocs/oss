# Determine whether an object exists

This topic describes how to determine whether an object exists.

The following code provides an example on how to check whether an object named exampleobject.txt exists in a bucket named examplebucket:

```
# -*- coding: utf-8 -*-
import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to Object Storage Service (OSS) because the account has permissions on all API operations. We recommend that you use a Resource Access Management (RAM) user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# The endpoint of the China (Hangzhou) region is used in this region. Specify the actual endpoint. 
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', 'examplebucket')

# Specify the full path of the object. The full path of the object cannot contain bucket names. 
exist = bucket.object_exists('exampleobject.txt')
# If the returned value is true, the specified object exists. If the returned value is false, the specified object does not exist. 
if exist:
    print('object exist')
else:
    print('object not exist')
        
```

