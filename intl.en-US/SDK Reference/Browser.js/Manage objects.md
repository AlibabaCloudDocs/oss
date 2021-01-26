# Manage objects

A bucket may have a lot of objects. SDK provides a series of interfaces to help in managing the objects easily.

## View all objects

The following code uses `list` to enumerate all objects in the current bucket. Main parameters are described as follows:

-   Prefix: Specifies only list objects with specific prefixes.
-   Marker: Specifies list objects whose names are greater than a specified marker.
-   Delimiter: Obtains the common prefix of a group of objects.
-   Max-keys: Specifies the maximum number of objects returned.

```
let OSS = require('ali-oss');

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: 'Your bucket name'
});

async function list (dir) {
  try {
    // Without any parameters. A maximum of 1,000 objects can be returned by default.
    let result = await client.list();
    console.log(result);
    
    // Continue to list objects based on nextMarker
    if (result.isTruncated) {
      let result = await client.list({ marker : result.nextMarker });
    }

    // List all objects whose names contain the "my-" prefix
    let result = await client.list({
      prefix: 'my-'
    });
    console.log(result);

    // List all objects whose names are greater than "my-object" marker and contain the "my-" prefix
    let result = await client.list({
      prefix: 'my-',
      marker: 'my-object'
    });
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

list();
            
```

## Simulate the directory structure

OSS is an object-based storage service. It has no directories. All objects stored in a bucket are uniquely identified by the key of the object. These objects does not follow any hierarchical structure amongst themselves. Such structure allows OSS to store objects efficiently. However, you may want to put different types of objects under different “directories” to manage objects in a conventional way. OSS provides a “Public Prefix” feature that helps to create a stimulated directory structure. For more information about the public prefix concept, see [View the object list](/intl.en-US/Developer Guide/Objects/Manage files/List objects.md).

Assume that the bucket already contains the following objects:

```
foo/x
foo/y
foo/bar/a
foo/bar/b
foo/hello/C/1
foo/hello/C/2
...
foo/hello/C/9999
            
```

Next we implement a function called `listDir` to enlist all the objects and subdirectories under the specified directory:

```
let OSS = require('ali-oss');

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: 'Your bucket name'
});

async function listDir (dir) {
   try {
     let result = await client.list({
       prefix: dir,
       delimiter: '/'
   });
   
   result.prefixes.forEach(function (subDir) {
      console.log('SubDir: %s', subDir);
   });
   result.objects.forEach(function (obj) {
      console.log('Object: %s', obj.name);
   });
  } catch (e) {
    console.log(e);
  }
}
            
```

The execution results are as follows:

```
> listDir('foo/')
=> SubDir: foo/bar/
   SubDir: foo/hello/
   Object: foo/x
   Object: foo/y

> listDir('foo/bar/')
=> Object: foo/bar/a
   Object: foo/bar/b

> listDir('foo/hello/C/')
=> Object: foo/hello/C/1
   Object: foo/hello/C/2
   ...
   Object: foo/hello/C/9999
            
```

## Object Meta

When uploading an object to OSS, except for the object content, you have to specify certain attribute information for the object. This attribute information is called meta information. The metadata is stored together with the object during uploading.

This is because the object meta information is stored in HTTP headers during object uploading/downloading, the meta information cannot contain any complex characters according to HTTP.

**Note:** Therefore, the meta information must only contain simple printable ASCII characters and cannot contain line breaks. The maximum size of all meta information cannot exceed 8KB.

You can specify the object meta information by specifying the `meta` parameter when using the `put`, putStream and `multipartUpload`:

```
let OSS = require('ali-oss')

let client = new OSS({
  region: '<Your region>', 
  accessKeyId: '<Your AccessKeyId>', 
  accessKeySecret: '<Your AccessKeySecret>', 
  bucket: 'Your bucket name'
});

async function multipartUploadWithMeta () {
  try {
    let result = await client.multipartUpload('object-key', 'local-file', { 
    meta: {
       year: 2017,
       people: 'mary'
     }
   });
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

multipartUploadWithMeta();
            
```

## Delete an object

You can use `delete` to delete the object:

```
let OSS = require('ali-oss')

let client = new OSS({
  region: '<Your region>', 
  accessKeyId: '<Your AccessKeyId>', 
  accessKeySecret: '<Your AccessKeySecret>', 
  bucket: 'Your bucket name'
});

async function delete () {
  try {
    let result = await client.delete('object-key');
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

delete();
            
```

## Delete multiple objects

The following code uses `deleteMulti` to delete multiple objects. You can set the `quiet` parameter to determine whether to return the deletion results:

```
let OSS = require('ali-oss')

let client = new OSS({
  region: '<Your region>', 
  accessKeyId: '<Your AccessKeyId>', 
  accessKeySecret: '<Your AccessKeySecret>', 
  bucket: 'Your bucket name'
});

async function deleteMulti () {
  try {
    let result = await client.deleteMulti(['obj-1', 'obj-2', 'obj-3']);
    console.log(result);

    let  result = await client.deleteMulti(['obj-1', 'obj-2', 'obj-3'],{ 
    quiet: true
  });
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

deleteMulti();
            
```

