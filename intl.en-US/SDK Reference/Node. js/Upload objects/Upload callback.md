# Upload callback

This topic describes how to use upload callback.

**Note:** For the complete code used to implement upload callback, visit [GitHub](https://github.com/ali-sdk/ali-oss#putname-file-options). For more information about upload callback, see [Upload callback](/intl.en-US/Developer Guide/Objects/Upload files/Upload callback.md) in Object Storage Service \(OSS\) Developer Guide and [Callback](/intl.en-US/API Reference/Object operations/Basic operations/Callback.md) in API Reference.

The following code provides an example on how to use upload callback when you upload a local file named examplefile.txt to a bucket named examplebucket. After the local file is uploaded, the name of the object in OSS is exampleobject.txt.

```
const oss = require('ali-oss');
var path = require('path');

const client = oss({
  // Set yourregion to the endpoint of the region where the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourregion to oss-cn-hangzhou. 
  region: 'yourregion',
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
  accessKeyId: 'yourAccessKeyId',
  accessKeySecret: 'yourAccessKeySecret',
  // Specify the bucket name. 
  bucket: 'examplebucket'
});

const options = {
  callback: {
    // Set the address of the server to which the callback request is sent. Example: http://oss-demo.aliyuncs.com:23450. 
    url: 'http://oss-demo.aliyuncs.com:23450',
    // Optional. Set the value of the Host field included in the callback request header. 
    //host: 'yourCallbackHost',
    // Set the value of the body field included in the callback request. 
    body: 'bucket=${bucket}&object=${object}&var1=${x:var1}',
    // Set Content-Type for the callback request. 
    contentType: 'application/x-www-form-urlencoded',
    // Configure the custom parameters for the callback request. 
    customValue: {
      var1: 'value1',
      var2: 'value2'
    }
  }
}

async function put () {
  try {
    // Specify the full paths of the object and the local file. The full path of the object cannot contain bucket names. 
    // By default, if the path of the local file is not specified, the local file is uploaded from the path of the project to which the sample program belongs. 
    let result = await client.put('exampleobject.txt', path.normalize('/localpath/examplefile.txt'), options);
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

put();        
```

