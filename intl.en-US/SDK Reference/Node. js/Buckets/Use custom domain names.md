# Use custom domain names

After you add a Canonical Name \(CNAME\) record to map a custom domain name to a specified bucket, you can use the custom domain name to access Object Storage Service \(OSS\) resources. This topic describes how to use a custom domain name to access a bucket.

For example, you have a website with a domain name my-domain.com. Users access the images on your website by using URLs in the following format: `http://img.my-domain.com/x.jpg`. After you migrate the data of your website to an OSS bucket, you can map the domain name to the bucket to access the images by using the same URLs. For more information, see [Map custom domain names](/intl.en-US/Developer Guide/Buckets/Map custom domain names.md) in OSS Developer Guide.

When you use OSS SDK for Node.js, you can use a custom domain name as the endpoint of a bucket to access the bucket. In this case, you must set `cname` to true.

```
let OSS = require('ali-oss')

let client = new OSS({
  endpoint: 'http://img.my-domain.com', // Use the custom domain name as the endpoint to access the bucket. 
  accessKeyId: 'yourAccessKeyId',  
  accessKeySecret: 'yourAccessKeySecret',
  cname: true
});

client.useBucket('examplebucket')
        
```

**Note:** If a custom domain name is mapped to a bucket, the domain name cannot be used to call the list\_buckets operation.

