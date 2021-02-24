# Download objects

This topic describes how to preview or download an object by using the URL of the object.

**Note:** You can use the `signatureUrl` method in the browser to generate an object URL to preview or download an object. By default, the validity period of the URL is half an hour, which is 1800 seconds.

## Preview an object by using the URL of the object

The following code provides an example on how to preview an object by using the URL of the object:

```
const OSS = require('ali-oss');

const client = new OSS({
  // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
  region: '<Your region>',
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>',
});

let url;
// object-key indicates the complete path of the object you want to download, and must include the extension of the object. For example, you can set object-key to abc/efg/123.jpg.
url = client.signatureUrl('object-key');
console.log(url);

// In this example, the validity period of a URL is set to 3600 seconds. By default, if you do not set the validity period, it is set to 1800 seconds.
url = client.signatureUrl('object-key', {expires: 3600});
console.log(url);        
```

## Download an object by using the URL of the object

The following code provides an example on how to download an object by using the URL of the object:

```
const OSS = require('ali-oss');

const client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>',
});

// Configure the response header to automatically download an object by using the URL, and set the name of the downloaded object.
const filename = 'test.js '// filename is the custom name of the downloaded object.
const response = {
  'content-disposition': `attachment; filename=${encodeURIComponent(filename)}`
}
// object-key indicates the complete path of the object you want to download, and must include the extension of the object. For example, you can set object-key to abc/efg/123.jpg.
const url = client.signatureUrl('object-key', { response });
console.log(url);
```

