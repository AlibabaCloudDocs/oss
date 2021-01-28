# Append upload

You can use AppendObject to append content to appendable objects that are uploaded.

## Usage notes

When you call AppendObject to upload an object, take note of the following items:

-   If the object to append does not exist, an append object is created.
-   If the object to append is an existing append object and the specified position from which the append operation starts is not equal to the current object size, the PositionNotEqualToLength error is returned. If the object to append is a non-append object, the ObjectNotAppendable error is returned.
-   The CopyObject operation cannot be performed on append objects.

For more information about how to perform append upload, see [AppendObject](/intl.en-US/API Reference/Object operations/Basic operations/AppendObject.md).

## Sample code

The following code provides an example on how to perform append upload:

```
let OSS = require('ali-oss')

let client = new OSS({
  // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
  region: '<Your region>',
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  // Specify the name of the bucket to which you want to upload the object by using append upload.
  bucket: '<Your bucket name>',
});

async function append () {
  // Perform the first append operation. The position from which the next append operation starts is included in the response.
  // object-name indicates the complete path of the uploaded object excluding the bucket name. Example: destfolder/examplefile.txt.
  // local-file indicates the complete path of the local file including the suffix. Example: /users/local/examplefile.txt.
  let result = await client.append('object-name', 'local-file')
  
  // Perform the second append operation. The position from which the next append operation starts is the value of Content-Length, which indicates the size of the object after the current append operation is performed.  
  result = await client.append('object-name', 'local-file', {
    position: result.nextAppendPosition
  })
}

append();
```

For more information about the naming conventions for buckets, see [bucket](/intl.en-US/Developer Guide/Terms.md). For more information about the naming conventions for objects, see [object](/intl.en-US/Developer Guide/Terms.md).

