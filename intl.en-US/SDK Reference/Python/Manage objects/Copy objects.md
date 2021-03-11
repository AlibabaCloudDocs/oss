# Copy objects

You can copy an object from a source bucket to a destination bucket within the same region.

Object copy has the following limits:

-   You must have read and write permissions on the source object.
-   You cannot copy the object from a region to another region. For example, you cannot copy an object from a bucket in the China \(Hangzhou\) region to another bucket in the China \(Qingdao\) region.

## Simple copy

You can use simple copy for objects that are smaller than 1 GB in size. The following code provides an example on how to perform simple copy:

```
# -*- coding: utf-8 -*-
import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create your RAM user, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourDestinationBucketName>')

bucket.copy_object('<yourSourceBucketName>', '<yourSourceObjectName>', '<yourDestinationObjectName>')
            
```

## Copy a large object

To copy an object larger than 1 GB, you need to use UploadPartCopy. In other words, you need to split the object into multiple parts and copy each of them simultaneously. To enable UploadPartCopy, perform the following steps:

1.  Use bucket.init\_multipart\_upload to initiate a multipart copy task.
2.  Use bucket.upload\_part\_copy to copy parts. All parts, except for the last part, must be larger than 100 KB.
3.  Use bucket.complete\_multipart\_upload to complete the multipart copy task.

Run the following code for UploadPartCopy task.

```
# -*- coding: utf-8 -*-
import oss2
from oss2.models import PartInfo
from oss2 import determine_part_size

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create your RAM user, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

src_object = '<yourSourceObjectName>'
dst_object = '<yourDestinationObjectName>'

total_size = bucket.head_object(src_object).content_length
part_size = determine_part_size(total_size, preferred_size=100 * 1024)

# Initiate a multipart copy task.
upload_id = bucket.init_multipart_upload(dst_object).upload_id
parts = []

# Copy each part sequentially.
part_number = 1
offset = 0
while offset < total_size:
    num_to_upload = min(part_size, total_size - offset)
    byte_range = (offset, offset + num_to_upload - 1)

    result = bucket.upload_part_copy(bucket.bucket_name, src_object, byte_range,dst_object, upload_id, part_number)
    parts.append(PartInfo(part_number, result.etag))

    offset += num_to_upload
    part_number += 1

# Complete multipart copy.
bucket.complete_multipart_upload(dst_object, upload_id, parts)
            
```

For more information about UploadPartCopy, see [UploadPartCopy](/intl.en-US/API Reference/Object operations/Multipart upload/UploadPartCopy.md)

