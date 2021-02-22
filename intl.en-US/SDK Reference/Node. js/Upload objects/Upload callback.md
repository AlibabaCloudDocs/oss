# Upload callback

This topic describes how to use upload callback.

For the complete code used to implement upload callback, visit [GitHub](https://github.com/ali-sdk/ali-oss#putname-file-options).

The following code provides an example on how to use upload callback:

```
const oss = require('ali-oss');

const store = oss({
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to https://ram.console.aliyun.com.
  accessKeyId: '<your access key>',
  accessKeySecret: '<your access secret>',
  bucket: '<your bucket name>',
  // Obtain the region where the current bucket is located.
  region: 'oss-cn-hangzhou'
});
const options = {
  callback: {
    // Specify the IP address of the server to which you want to send the callback request, for example, http://oss-demo.aliyuncs.com:23450 or http://127.0.0.1:9090.
    url: '<callbackUrl>',
    // (Optional) Configure the value of the host field carried in the callback request header.
    //host: '<callbackHost>',
    // Configure Content-Type for the callback request.
    body: 'bucket=${bucket}&object=${object}&var1=${x:var1}',
    contentType: 'application/x-www-form-urlencoded',
    // Configure custom parameters for the callback request.
    customValue: {
      var1: 'value1',
      var2: 'value2'
    }
  }
}
const result = await store.put('<object name>', options)
```

For more information about the parameters for upload callback, see [Callback](/intl.en-US/API Reference/Object operations/Basic operations/Callback.md). For more information about the principles of upload callback, see [Upload callback](/intl.en-US/Developer Guide/Objects/Upload files/Upload callback.md) in OSS Developer Guide.

