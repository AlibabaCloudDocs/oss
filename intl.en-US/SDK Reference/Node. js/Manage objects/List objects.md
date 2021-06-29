# List objects

This topic describes how to list all objects in a bucket, a specified number of objects in a bucket, and objects whose names contain a specified prefix in a bucket.

## Methods

You can call the `list` or `listV2` method to list up to 1,000 objects in a bucket at a time. You can configure parameters to list objects in different ways. For example, you can configure parameters to list objects from a specified start position, list objects and subfolders in a specified folder, and list objects by page. The list and listV2 methods are different in the following aspects:

-   By default, when you use the `list` method to list objects, the information about the object owners is returned.
-   When you use `listV2` to list objects, you must configure the fetch-owner parameter to specify whether to include the information about the object owners in the returned results.

    **Note:** We recommend that you use the `listV2` method to list objects in versioned buckets.


The following tables describe the parameters that you can configure when you use the `list` and `listV2` methods to list objects.

-   Use the list method to list objects

    The following table describes the parameters that you can configure when you use the list method to list objects.

    |Parameter|Type|Description|
    |:--------|----|:----------|
    |prefix|string|The prefix that the names of listed objects must contain.|
    |delimiter|string|The character used to group objects by name.|
    |marker|string|The name of the object from which the list operation begins. If this parameter is specified, objects whose names are alphabetically greater than the marker parameter value are returned.|
    |max-keys|number \| string|The maximum number of returned objects.|
    |encoding-type|'url' \| ''|Specifies that the object names in the response are URL-encoded.|

-   Use the listV2 method to list objects

    The following table describes the parameters that you can configure when you use the listV2 method to list objects.

    |Parameter|Type|Description|
    |:--------|----|:----------|
    |prefix|string|The prefix that the names of listed objects must contain.|
    |continuation-token|string|The position from which the object list is obtained.|
    |delimiter|string|The character used to group objects by name.|
    |max-keys|number \| string|The maximum number of returned objects.|
    |start-after|string|The name of the object from which the list operation begins. If this parameter is specified, objects whose names are alphabetically greater than the start-after parameter value are returned.|
    |fetch-owner|boolean|Specifies whether to include the information about object owners in the response.|
    |encoding-type|'url' \| ''|Specifies that the object names in the response are URL-encoded.|


## Simple list

The following code provides an example on how to list objects in a specified bucket. By default, up to 100 objects are listed by an operation.

-   Use the list method to list objects

    ```
    let OSS = require('ali-oss');
    
    let client = new OSS({
      // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
      region: '<Your region>',
      // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
      accessKeyId: '<Your AccessKeyId>',
      accessKeySecret: '<Your AccessKeySecret>',
      // Specify the bucket name.
      bucket: '<Your bucket name>',
    });
    
    async function list () {
        // By default, if no parameter is specified, up to 100 objects can be returned.
        let result = await client.list();
        console.log(result);
    }
    
    list();
    ```

-   Use the listV2 method to list objects

    ```
    let OSS = require('ali-oss');
    
    let client = new OSS({
      // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
      region: '<Your region>',
      // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
      accessKeyId: '<Your AccessKeyId>',
      accessKeySecret: '<Your AccessKeySecret>',
      // Specify the bucket name.
      bucket: '<Your bucket name>',
    });
    
    async function list () {
        // By default, if no parameter is specified, up to 100 objects can be returned.
        let result = await client.listV2();
        console.log(result);
    }
    
    list();
    ```


## List a specified number of objects

The following code provides an example on how to list a specified number of objects in a bucket by configuring the max-keys parameter.

-   Use the list method to list objects

    ```
    let OSS = require('ali-oss');
    
    let client = new OSS({
      // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
      region: '<Your region>',
      // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
      accessKeyId: '<Your AccessKeyId>',
      accessKeySecret: '<Your AccessKeySecret>',
      // Specify the bucket name.
      bucket: '<Your bucket name>',
    });
    
    async function list () {
        let result = await client.list({
            // Specify that the first 10 objects in alphabetical order are returned.
          "max-keys": 10
      });
        console.log(result);
    }
    
    list();
    ```

-   Use the listV2 method to list objects

    ```
    let OSS = require('ali-oss');
    
    let client = new OSS({
      // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
      region: '<Your region>',
      // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
      accessKeyId: '<Your AccessKeyId>',
      accessKeySecret: '<Your AccessKeySecret>',
      // Specify the bucket name.
      bucket: '<Your bucket name>',
    });
    
    async function list () {
        let result = await client.listV2({
            // Specify that the first 10 objects in alphabetical order are returned.
          "max-keys": 10
      });
        console.log(result);
    }
    
    list();
    ```


## List objects whose names contain a specified prefix

The following code provides an example on how to list objects whose names contain a specified prefix in a bucket by configuring the prefix parameter.

-   Use the list method to list objects

    ```
    let OSS = require('ali-oss');
    
    let client = new OSS({
      // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
      region: '<Your region>',
      // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
      accessKeyId: '<Your AccessKeyId>',
      accessKeySecret: '<Your AccessKeySecret>',
      // Specify the bucket name.
      bucket: '<Your bucket name>',
    });
    
    async function list () {
      let result = await client.list({
        // List 10 objects.
        "max-keys": 10,
        // List objects whose names contain the prefix foo/.
        prefix: 'foo/'
      });
      console.log(result);
    }
    
    list();
    ```

-   Use the listV2 method to list objects

    ```
    let OSS = require('ali-oss');
    
    let client = new OSS({
      // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
      region: '<Your region>',
      // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
      accessKeyId: '<Your AccessKeyId>',
      accessKeySecret: '<Your AccessKeySecret>',
      // Specify the bucket name.
      bucket: '<Your bucket name>',
    });
    
    async function list () {
      let result = await client.listV2({
        // List 10 objects.
        "max-keys": 10,
        // List objects whose names contain the prefix foo/.
        prefix: 'foo/'
      });
      console.log(result);
    }
    
    list();
    ```


## List objects whose names are alphabetically greater than the specified object name

The following code provides an example on how to list objects whose names are alphabetically greater than the string specified by the marker or startAfter parameter.

-   Use the list method to list objects

    The name of the object from which the list operation begins is specified by the marker parameter.

    ```
    let OSS = require('ali-oss');
    
    let client = new OSS({
      // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
      region: '<Your region>',
      // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
      accessKeyId: '<Your AccessKeyId>',
      accessKeySecret: '<Your AccessKeySecret>',
      // Specify the bucket name.
      bucket: '<Your bucket name>',
    });
    
    // List the objects whose names are alphabetically greater than the object name test. By default, up to 100 objects are listed.
    const marker = 'test'
    async function list () {
      let result = await client.list({
        marker
      });
      console.log(result);
    }
    
    list();
    ```

-   Use the listV2 method to list objects

    The name of the object from which the list operation begins is specified by the startAfter parameter.

    ```
    let OSS = require('ali-oss');
    
    let client = new OSS({
      // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
      region: '<Your region>',
      // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
      accessKeyId: '<Your AccessKeyId>',
      accessKeySecret: '<Your AccessKeySecret>',
      // Specify the bucket name.
      bucket: '<Your bucket name>',
    });
    
    async function list () {
      let result = await client.listV2({
        // List the objects and subfolders whose names are alphabetically greater than a/b in the a/ folder.
        delimiter: '/',
        prefix: 'a/',
        'start-after': 'a/b'
      });
      console.log(result.objects, result.prefixes);
    }
    
    list();
    ```


## List all objects by page

The following code provides an example on how to list all objects in a specified bucket by page. You can configure the max-keys parameter to specify the maximum number of objects that can be listed on each page.

-   Use the list method to list objects

    ```
    let OSS = require('ali-oss');
    
    let client = new OSS({
      // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
      region: '<Your region>',
      // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
      accessKeyId: '<Your AccessKeyId>',
      accessKeySecret: '<Your AccessKeySecret>',
      // Specify the bucket name.
      bucket: '<Your bucket name>',
    });
    
    let marker = null;
    // List up to 20 objects on each page.
    const maxKeys = 20;
    async function list () {
      do {
        let result = await client.list({
              marker,
          'max-keys': maxKeys
          });
        marker = result.nextMarker;
            console.log(result);
      }while(marker)
    }
    
    list();
    ```

-   Use the listV2 method to list objects

    ```
    let OSS = require('ali-oss');
    
    let client = new OSS({
      // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
      region: '<Your region>',
      // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
      accessKeyId: '<Your AccessKeyId>',
      accessKeySecret: '<Your AccessKeySecret>',
      // Specify the bucket name.
      bucket: '<Your bucket name>',
    });
    
    async function list () {
      let continuationToken = null;
      // List up to 20 objects on each page.
      const maxKeys = 20;
      do {
        let result = await client.listV2({
              'continuation-token': continuationToken
          'max-keys': maxKeys
          });
        continuationToken = result.nextContinuationToken;
            console.log(result);
      }while(continuationToken)
    }
    
    list();
    ```


## List objects and their owner information

The following code provides an example on how to list objects and their owner information:

```
let OSS = require('ali-oss');

let client = new OSS({
  // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
  region: '<Your region>',
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  // Specify the bucket name.
  bucket: '<Your bucket name>',
});

// By default, the information about the object owners is not listed. To include the object owner information in the returned results, you must set the fetch-owner parameter to true.
async function list () {
  let result = await client.listV2({
    'fetch-owner': true
  });
  console.log(result.objects);
}

list();
```

## List objects and subfolders in a specified folder

OSS does not use a hierarchical structure for objects, but instead uses a flat structure. All elements are stored as objects in buckets. A folder is an object whose size is 0 and whose name ends with a forward slash \(/\). You can upload and download this object. By default, objects whose names end with a forward slash \(/\) is displayed as a folder in the OSS console.

You can use the delimiter and prefix parameters to list objects by folder.

-   If you set prefix to a folder name in the request, objects and subfolders whose names contain the prefix are listed.
-   If you also set delimiter to a forward slash \(/\) in the request, the objects and subfolders whose names start with the specified prefix in the folder are listed. Each subfolder is listed as a single result element in commonPrefixes. The objects and folders in these subfolders are not listed.

The following objects are stored in the bucket:

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

The following code provides an example on how to use the `list` or `listV2` method to list objects and subfolders in a specified folder.

-   Use the list method to list objects

    ```
    let OSS = require('ali-oss');
    
    let client = new OSS({
      // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
      region: '<Your region>',
      // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
      accessKeyId: '<Your AccessKeyId>',
      accessKeySecret: '<Your AccessKeySecret>',
      // Specify the bucket name.
      bucket: '<Your bucket name>',
    });
    
    // Call the listDir function and configure different prefixes to list the required objects.
    async function listDir(dir) {
      const result = await client.list({
        prefix: dir,
        // Set the delimiter to a forward slash (/).
        delimiter: '/'
      });
    
      result.prefixes.forEach(subDir => {
        console.log('SubDir: %s', subDir);
      });
      result.objects.forEach(obj => {
        console.log('Object: %s', obj.name);
      });
    }
    listDir('foo/');
    // SubDir: foo/bar/
    // SubDir: foo/hello/
    // Object: foo/x
    // Object: foo/y
    
    listDir('foo/bar/');
    // Object: foo/bar/a
    // Object: foo/bar/b
    
    listDir('foo/hello/C/');
    // Object: foo/hello/C/1
    // Object: foo/hello/C/2
    // ...
    // Object: foo/hello/C/9999
    ```

-   Use the listV2 method

    ```
    let OSS = require('ali-oss');
    
    let client = new OSS({
      // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
      region: '<Your region>',
      // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
      accessKeyId: '<Your AccessKeyId>',
      accessKeySecret: '<Your AccessKeySecret>',
      // Specify the bucket name.
      bucket: '<Your bucket name>',
    });
    // Call the listV2Dir function and configure different prefixes to list the required objects.
    async function listV2Dir(dir) {
      const result = await client.listV2({    
        prefix: dir,
        // Set the delimiter to a forward slash (/).
        delimiter: '/'
      });
    
      result.prefixes.forEach(subDir => {
        console.log('SubDir: %s', subDir);
      });
      result.objects.forEach(obj => {
        console.log('Object: %s', obj.name);
      });
    }
    listDir('foo/');
    // SubDir: foo/bar/
    // SubDir: foo/hello/
    // Object: foo/x
    // Object: foo/y
    
    listDir('foo/bar/');
    // Object: foo/bar/a
    // Object: foo/bar/b
    
    listDir('foo/hello/C/');
    // Object: foo/hello/C/1
    // Object: foo/hello/C/2
    // ...
    // Object: foo/hello/C/9999
    ```


