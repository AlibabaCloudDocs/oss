# Bind custom domain names

After you add a CNAME record to bind the custom domain name to a specified bucket, you can use a custom domain name to access OSS resources. This topic describes how to use a custom domain name to bind the bucket.

If your domain name is my-domain.com, you can access all images in the `http://img.my-domain.com/image.jpg` format. After you migrate the images to OSS, you can still use the original URL to access the images by binding the custom domain name. For more information, see [Bind custom domain names](/intl.en-US/Developer Guide/Buckets/Bind custom domain names.md) in OSS Developer Guide.

To use your custom domain name as the endpoint of a bucket when you use OSS SDK for Node.js, set `cname` to true.

The following code provides an example on how to bind the http://img.my-domain.com custom domain name to the examplebucket bucket.

```
let OSS = require('ali-oss')

let client = new OSS({
  endpoint: 'http://img.my-domain.com', // Use a custom domain name as the endpoint.
  accessKeyId: 'yourAccessKeyId',  
  accessKeySecret: 'yourAccessKeySecret',
  cname: true
});

client.useBucket('examplebucket')
        
```

**Note:** The custom domain name is bound to a specified bucket. Therefore, the list\_buckets operation cannot be used when CNAME is used.

