# Delete objects

This topic describes how to delete a single object or batch delete objects.

**Warning:** Deleted objects cannot be recovered. Exercise caution when you delete objects.

## Delete a single object

The following code provides an example on how to delete an object named exampleobject.txt from a bucket named examplebucket:

```
let OSS = require('ali-oss')

let client = new OSS({
  // Set yourregion to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourregion to https://oss-cn-hangzhou.aliyuncs.com. 
  region: 'yourRegion',
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
  accessKeyId: 'yourAccessKeyId',
  accessKeySecret: 'yourAccessKeySecret',
  // Specify the bucket name. 
  bucket: 'examplebucket',
});

async function deleteFile () {
  try {
    // Specify the full path of the object that you want to delete. The full path of the object cannot contain bucket names. 
    let result = await client.delete('exampleobject.txt');
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

deleteFile();
            
```

## Delete multiple objects in a batch

You can batch delete multiple specified objects or objects whose names contain a specified prefix.

-   Delete multiple specified objects

    The following code provides an example on how to delete multiple specified objects from a bucket named examplebucket.

    The result can be returned in the following two modes. Select a return mode based on your requirements.

    -   verbose: A list of objects that is deleted is returned. This is the default return mode.
    -   quiet: A list of objects that failed to be deleted is returned.
    ```
    let OSS = require('ali-oss')
    
    let client = new OSS({
      // Set yourregion to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourregion to https://oss-cn-hangzhou.aliyuncs.com. 
      region: 'yourRegion',
      // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
      accessKeyId: 'yourAccessKeyId',
      accessKeySecret: 'yourAccessKeySecret',
      // Specify the bucket name. 
      bucket: 'examplebucket',
    });
    
    async function deleteMulti () {
      try {
        // Specify the full paths of the objects to delete and set the return mode to verbose. The full paths of the objects cannot contain bucket names. 
        //let result = await client.deleteMulti(['exampleobject-1', 'exampleobject-2', 'testfolder/sampleobject.txt']);
        //console.log(result);
        // Specify the full paths of the objects to delete and set the return mode to quiet. The full paths of the objects cannot contain bucket names. 
        let result = await client.deleteMulti(['exampleobject-1', 'exampleobject-2', 'testfolder/sampleobject.txt'], {quiet: true});
        console.log(result);
      } catch (e) {
        console.log(e);
      }
    }
    
    deleteMulti();
                
    ```

-   Delete multiple objects whose names contain a specified prefix

    The following code provides an example on how to delete objects whose names contain the file prefix and return the result in quiet mode:

    ```
    const OSS = require('ali-oss');
    
    const client = new OSS({
      // Set yourregion to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourregion to https://oss-cn-hangzhou.aliyuncs.com. 
      region: 'yourRegion',
      // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
      accessKeyId: 'yourAccessKeyId',
      accessKeySecret: 'yourAccessKeySecret',
      // Specify the bucket name. 
      bucket: 'examplebucket',
    });
    
    // Handle request failures, prevent the interruption of promise.all, and return failure causes and the names of objects that failed to be deleted. 
    async function handleDel(name, options) {
      try {
        await client.delete(name);
      } catch (error) {
        error.failObjectName = name;
        return error;
      }
    }
    
    // Delete objects whose names contain the specified prefix. 
    async function deletePrefix(prefix) {
      const list = await client.list({
        prefix: prefix,
      });
    
      list.objects = list.objects || [];
      const result = await Promise.all(list.objects.map((v) => handleDel(v.name)));
      console.log(result);
    }
    deletePrefix('file')
    ```


