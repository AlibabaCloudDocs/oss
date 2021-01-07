# Append upload

You can use AppendObject to append content to appendable objects that are uploaded.

## Usage notes

When you call AppendObject to upload an object, take note of the following items:

-   If the object to append does not exist, an append object is created.
-   If the object to append is an existing append object and the specified position from which the append operation starts is not equal to the current object size, the PositionNotEqualToLength error is returned. If the object to append is a non-append object, the ObjectNotAppendable error is returned.
-   The CopyObject operation cannot be performed on append objects.

For more information about how to perform append upload, see [AppendObject](/intl.en-US/API Reference/Object operations/Basic operations/AppendObject.md).

## Sample codes

The following code provides an example on how to perform append upload:

```
# -*- coding: utf-8 -*-
import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
bucket = oss2.Bucket(auth, 'https://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Set the position from which the first append operation starts to 0.
# <yourObjectName> indicates the complete path of the object you want to append, and must include the extension of the object. Example: example/test.txt
result = bucket.append_object('<yourObjectName>', 0, 'content of first append')
# If you have appended content to the object, you can obtain the position from which the current append operation starts from the next_position field in the response returned by the last operation or by using the bucket.head_object method.
bucket.append_object('<yourObjectName>', result.next_position, 'content of second append')
        
```

For more information about the naming conventions for buckets, see [bucket](/intl.en-US/Developer Guide/Terms.md). For more information about the naming conventions for objects, see [object](/intl.en-US/Developer Guide/Terms.md).

