# Copy objects

This topic describes how to copy objects. When you copy an object, you can set HTTP headers and metadata for the destination object.

**Note:** For more information about object metadata, see [Manage object metadata](/intl.en-US/Developer Guide/Objects/Manage object metadata.md) in OSS Developer Guide.

The following code provides an example on how to copy objects within the same bucket:

```
const OSS = require('ali-oss');
const client = new OSS({
  // Set yourRegion to the endpoint of the region where the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourRegion to oss-cn-hangzhou. 
  region: 'yourRegion',
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to access OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
  accessKeyId: 'yourAccessKeyId',
  accessKeySecret: 'yourAccessKeySecret',
  // Specify the name of the bucket. 
  bucket: 'examplebucket',
  // Specify whether to enable HTTPS. If secure is set to true, HTTPS is enabled. 
  //secure: true
})
// Copy the objects from the same bucket. 
// Specify the paths of the source object and the destination object. The paths cannot contain bucket names. 
client.copy('dstexampleobject.txt', 'srcexampleobject.txt',
// Set HTTP headers and metadata for the destination object. 
//{
    // Specify the headers parameter to set HTTP headers for the destination object. If the headers parameter is not specified, the HTTP headers of the destination object are the same as those of the source object. The HTTP headers of the source object are copied. 
    //headers:{
        //'Cache-Control': 'no-cache',
    //},
    // Specify the meta parameter to set metadata for the destination object. If the meta parameter is not specified, the metadata of the object is the same as that of the source object. The metadata of the source object is copied. 
    //meta:{
        //location: 'hangzhou',
        //year: 2015,
        //people: 'mary'
    //}
//}
).then((res) => {
    console.log(res);
}).catch(e => {
  console.log(e);
})
```

The following code provides an example on how to copy objects from two different buckets that are in the same region:

```
const OSS = require('ali-oss')

const client = new OSS({
  // Set yourRegion to the endpoint of the region where the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourRegion to oss-cn-hangzhou. 
  region: 'yourRegion',
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to access OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
  accessKeyId: 'yourAccessKeyId',
  accessKeySecret: 'yourAccessKeySecret',
  // Specify the name of the bucket. 
  bucket: 'dstexamplebucket',
});

async function copy () {
  try {
    // Copy objects from two different buckets that are in the same region, and the address format of the source object is '/bucketname/objectname'. 
    const result = await client.copy('dstexampleobjectnew.txt', '/srcexamplebucket/srcexampleobject.txt',
    // Set HTTP headers and metadata for the destination object. 
    //{
        // Specify the headers parameter to set HTTP headers for the destination object. If the headers parameter is not specified, the HTTP headers of the destination object are the same as those of the source object. The HTTP headers of the source object are copied. 
        //headers:{
            //'Cache-Control': 'no-cache',
        //},
        // Specify the meta parameter to set metadata for the destination object. If the meta parameter is not specified, the metadata of the destination object is the same as that of the source object. The metadata of the source object is copied. 
        //meta:{
            //location: 'hangzhou',
            //year: 2015,
            //people: 'mary'
        //}
    //}
    );
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

copy()      
```

