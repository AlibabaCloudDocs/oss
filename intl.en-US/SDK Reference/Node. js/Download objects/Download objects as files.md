# Download objects as files

This topic describes how to download objects from a bucket.

The following code provides an example on how to download an object named exampleobject.txt from the testfolder directory in a bucket named examplebucket to the D:\\localpath path in a local disk.

```
let OSS = require('ali-oss');

let client = new OSS({
  // Set yourregion to the endpoint of the region where the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com.
  region: 'yourRegion',
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
  accessKeyId: 'yourAccessKeyId',
  accessKeySecret: 'yourAccessKeySecret',
  // Specify the bucket name.
  bucket: 'examplebucket'
});

async function get () {
  try {
    // Specify a name for the object and the full path to where the object is downloaded. The full path of the object cannot contain bucket names.
    // If a local file with the same name exists, the downloaded object overwrites the local file.
    // If the path for the object is not specified, the downloaded object is saved to the path of the project to which the sample program belongs.
    let result = await client.get('exampleobject.txt', 'D:\\localpath\\examplefile.txt');
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

get();        
```

