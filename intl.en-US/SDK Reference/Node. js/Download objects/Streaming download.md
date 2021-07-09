# Streaming download

If a large object takes a long time to download, you can use streaming download to download the object in increments until the entire object is downloaded.

The following code provides an example on how to stream download an object named exampleobject.txt from a bucket named examplebucket to the D:\\localpath path. After the object is downloaded, the local file is named examplefile.txt.

**Note:** When you use `getStream` to download an object, `Readable Stream` is returned and used to stream the object content.

```
const OSS = require('ali-oss');
const fs = require('fs');

const client = new OSS({
  // Set yourRegion to the endpoint of the region where the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourRegion to oss-cn-hangzhou. 
  region: 'yourRegion',
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to access Object Storage Service (OSS) because the account has permissions on all API operations. We recommend that you use a Resource Access Management (RAM) user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
  accessKeyId: 'yourAccessKeyId',
  accessKeySecret: 'yourAccessKeySecret',
  // Specify the name of the bucket. 
  bucket: 'examplebucket',
});

async function getStream () {
  try {
    // Specify the full path of the object. The full path of the object cannot contain bucket names. 
    const result = await client.getStream('exampleobject.txt');
    console.log(result);
    // Specify the full path of the local file. If the specified local file exists, the downloaded object replaces the local file. Otherwise, the local file is created. 
    // If the path of the local file is not specified, the downloaded object is saved to the path of the project to which the sample program belongs. 
    const writeStream = fs.createWriteStream('D:\\localpath\\examplefile.txt'); 
    result.stream.pipe(writeStream);
  } catch (e) {
    console.log(e);
  }
}

getStream()
        
```

