# Local file upload

This topic describes how to call the put operation to upload a local file to Object Storage Service \(OSS\).

The following code provides an example on how to upload a local file named examplefile.txt to the examplebucket bucket. After the local file is uploaded, the name of the object stored in OSS is exampleobject.txt.

**Note:** The sample code in this topic uses the catch method. To obtain the syntax of this method, learn the Promise function defined in ECMAScript 6 and the async and await methods of this function.

```
const OSS = require('ali-oss')
var path = require("path")

const client = new OSS({
  // Set yourregion to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourregion to oss-cn-hangzhou. 
  region: 'yourregion',
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
  accessKeyId: 'yourAccessKeyId',
  accessKeySecret: 'yourAccessKeySecret',
  // Specify the bucket name. 
  bucket: 'examplebucket',
});

async function put () {
  try {
    // Specify the full paths of the object and the local file. The full path of the object cannot contain bucket names. 
    // If the path of the local file is not specified, the local file is uploaded from the path of the project to which the sample program belongs. 
    let result = await client.put('exampleobject.txt', path.normalize('/localpath/examplefile.txt'));
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

put();        
```

