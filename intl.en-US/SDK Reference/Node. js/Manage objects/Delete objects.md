# Delete objects

This topic describes how to delete a single or multiple objects and objects whose names contain a specified prefix.

## Delete a single object

The following code provides an example on how to use `delete` to delete an object:

```
let OSS = require('ali-oss')

let client = new OSS({
  // This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
  region: '<Your region>',
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create your RAM user, log on to the RAM console.
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>',
});

async function delete () {
  try {
    let result = await client.delete('object-name');
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

delete();
            
```

## Delete multiple objects

The following code provides an example on how to use `deleteMulti` to delete multiple objects and use quiet to specify whether to return the deletion results:

```
let OSS = require('ali-oss')

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>',
});

async function deleteMulti () {
  try {
    let result = await client.deleteMulti(['obj-1', 'obj-2', 'obj-3']);
    console.log(result);
    let result = await client.deleteMulti(['obj-1', 'obj-2', 'obj-3'], {
    quiet: true
  });
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

deleteMulti();
            
```

## Delete objects whose names contain a specified prefix

The following code provides an example on how to delete objects whose names contain a specified prefix:

```
const OSS = require('ali-oss');

const client = new OSS({
  bucket: '<Your BucketName>',
  region: '<Your Region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
});

// Handle request failures, prevent the interruption of promise.all, and return the failure reason and the name of the failed object.
async function handleDel(name, options) {
  try {
    await client.delete(name);
  } catch (error) {
    error.failObjectName = name;
    return error;
  }
}

// Delete the objects whose names contain a specified prefix.
async function deletePrefix(prefix) {
  const list = await client.list({
    prefix: prefix,
  });

  list.objects = list.objects || [];
  const result = await Promise.all(list.objects.map((v) => handleDel(v.name)));
  console.log(result);
}
deletePrefix('prefix')
```

