# Single-connection bandwidth throttling

This topic describes how to ensure other applications have sufficient bandwidth when you perform upload and download operations on OSS. You can include the x-oss-traffic-limit parameter in your requests and set bandwidth limits.

## Configure bandwidth throttling for simple uploads and downloads

The following code provides an example on how to configure bandwidth throttling for simple upload and download of objects:

```
const OSS = require('ali-oss')

const client = new OSS({
  bucket: '<Your BucketName>',
  // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
  region: '<Your Region>',
  // Security risks may arise if you log on with the AccessKey pair of an Alibaba Cloud account because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
});

// Configure the speed limit by using the request header.
const headers = {  
 'x-oss-traffic-limit': 8 * 1024 * 100 // Set the minimum bandwidth to 100 KB/s.
}

// Configure bandwidth throttling for the object to be uploaded.
async function put() {
  const result = await client.putStream('<file name>', '<file stream>', {
    headers,
    timeout: 60000 // Set the default timeout duration to 60000. Unit:ms. When the speed of uploading objects exceeds 60000 ms, the error message is reported. We recommend that you modify the timeout duration.
  })
  console.log(result)
}
put()

// Configure bandwidth throttling for the object to be downloaded.
async function get() {
  const result = await client.get('<file name>', {
    headers,
    timeout: 60000 // Set the default timeout duration to 60000. Unit:ms. When the speed of downloading objects exceeds 60000 ms, the error message is reported. We recommend that you modify the timeout duration.
  })
  console.log(result)
}
get()
```

## Configure bandwidth throttling when you use signed URLs to upload or download objects

The following code provides an example on how to configure single-connection bandwidth throttling when you use signed URLs to upload or download objects:

```
const OSS = require('ali-oss')

const client = new OSS({
  bucket: '<Your BucketName>',
  // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
  region: '<Your Region>',
  // Security risks may arise if you log on with the AccessKey pair of an Alibaba Cloud account because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
});

// Configure bandwidth throttling for the object to be uploaded by using URL Query.
async function putByQuery() {
  const url = client.signatureUrl('<file name>', {
    trafficLimit: 8 * 1024 * 100, // Set the minimum bandwidth to 100 KB/s.
    method: 'PUT' // Configure the PUT request method.
  })

  const result = await client.urllib.request(url, {
    method: 'PUT',
    stream: '<file stream>'
    timeout: 60000 // Set the default timeout duration to 60000. Unit:ms. We recommend that you modify the timeout duration after you configure bandwidth throttling. Otherwise, the request fails.
  });

  console.log(result)
}
putByQuery()

// Configure bandwidth throttling for the object to be uploaded by using URL Query.
async function getByQuery() {
  const url = client.signatureUrl('<file name>', {
    trafficLimit: 8 * 1024 * 100, // Set the minimum bandwidth to 100 KB/s.
  })

  const result = await client.urllib.request(url, {
    timeout: 60000 // Set the default timeout duration to 60000. Unit:ms. We recommend that you modify the timeout duration after you configure bandwidth throttling. Otherwise, the request fails.
  });

  console.log(result)
}
getByQuery()
```

