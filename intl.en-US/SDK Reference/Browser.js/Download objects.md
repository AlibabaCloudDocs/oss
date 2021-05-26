# Download objects

You can use the `signatureUrl` method in a browser to generate an object URL for previews or downloads. You can also use the download attribute in the <a\> tag of an HTML page or window.open of a web API to obtain an object URL.

## Obtain the URL to preview an object

The following code provides an example on how to obtain the URL of an object named exampleobject.txt in a bucket named examplebucket.

**Note:** To preview an object in a browser, set Content-Disposition to inline and use the custom domain name that is mapped to the bucket to access the object. For more information about specific operations, see [Configure HTTP headers for objects](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure HTTP headers for objects.md) and [CNAME](/intl.en-US/SDK Reference/Browser.js/CNAME.md).

```
const OSS = require('ali-oss');

const client = new OSS({ 
  // Set yourRegion to the endpoint of the region where the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourRegion to oss-cn-hangzhou. 
  region: 'yourRegion',
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to Object Storage Service (OSS) because the account has permissions on all API operations. We recommend that you use a Resource Access Management (RAM) user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
  accessKeyId: 'yourAccessKeyId',
  accessKeySecret: 'yourAccessKeySecret',
  // Specify the bucket name. 
  bucket: 'examplebucket',
});

let url;
// Specify the full path of the object. The full path of the object cannot contain bucket names. By default, the validity period of the object URL is 1,800 seconds, which equals to 30 minutes. 
url = client.signatureUrl('exampleobject.txt');
console.log(url);

// Set the validity period of the URL to 3600 seconds. If you do not set the validity period, default value 1800 is used. 
//url = client.signatureUrl('exampleobject.txt', {expires: 3600});
//console.log(url);        
```

## Obtain the URL to download an object

The following code provides an example on how to obtain the URL to download an object named exampleobject.txt in a bucket named examplebucket. By default, the validity period of the URL is 1,800 seconds.

```
const OSS = require('ali-oss');

const client = new OSS({
  // Set yourRegion to the endpoint of the region where the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourRegion to oss-cn-hangzhou. 
  region: 'yourRegion',
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
  accessKeyId: 'yourAccessKeyId',
  accessKeySecret: 'yourAccessKeySecret',
  // Specify the bucket name. 
  bucket: 'examplebucket',
});

// Set the response header to automatically download an object by using the URL, and set the name of the local file after the object is downloaded. 
const filename = 'examplefile.txt' // Customize the name of the local file after the object is downloaded. 
const response = {
  'content-disposition': `attachment; filename=${encodeURIComponent(filename)}`
}
// Specify the full path of the object. The full path of the object cannot contain bucket names. 
const url = client.signatureUrl('exampleobject.txt', { response });
console.log(url);
```

