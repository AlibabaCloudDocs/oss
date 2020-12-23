# List objects

This topic describes how to list objects in a versioned bucket, including all objects, a specified number of objects, and objects whose names contain a specified prefix.

## List the versions of all objects in a bucket

The following code provides an example on how to list the versions of all objects including delete markers within a specified bucket:

```
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
const accessKeyId = "<yourAccessKeyId>";
const accessKeySecret = "<yourAccessKeySecret>";
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
const endpoint = "httpss://oss-cn-hangzhou.aliyuncs.com";
const bucket = "<yourBucketName>";

// Create an OSSClient instance.
const ossClient = new OSS({
    accessKeyId,
  accessKeySecret,
  endpoint,
  bucket
});
// List the versions of all objects including delete markers within the bucket.
async function main () {
  let nextKeyMarker = null;
  let nextVersionMarker = null;
  let versionListing = null;
  do {
    versionListing = await client.getBucketVersions({
      keyMarker: nextKeyMarker,
      versionIdMarker: nextVersionMarker
    })
  
    versionListing.objects.forEach(o => {
      console.log(`${o.name}, ${o.versionId}`)
    })
    versionListing.deleteMarker.forEach(o => {
      console.log(`${o.name}, ${o.versionId}`)
    })
  
    nextKeyMarker = versionListing.NextKeyMarker;
    nextVersionMarker = versionListing.NextVersionIdMarker;
  } while (versionListing.isTruncated);
}

main()
```

## List the versions of objects whose names contain a specified prefix

The following code provides an example on how to list all versions of objects whose names contain a specified prefix:

```
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
const accessKeyId = "<yourAccessKeyId>";
const accessKeySecret = "<yourAccessKeySecret>";
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
const endpoint = "httpss://oss-cn-hangzhou.aliyuncs.com";
const bucket = "<yourBucketName>";

// Create an OSSClient instance.
const ossClient = new OSS({
    accessKeyId,
  accessKeySecret,
  endpoint,
  bucket
});
// Specify to return all versions of objects whose names contain the "test-" prefix.
async function main () {
  let nextKeyMarker = null;
  let nextVersionMarker = null;
  let versionListing = null;
  const prefix = 'test-'
  do {
    versionListing = await client.getBucketVersions({
      keyMarker: nextKeyMarker,
      versionIdMarker: nextVersionMarker,
      prefix
    })
  
    versionListing.objects.forEach(o => {
      console.log(`${o.name}, ${o.versionId}`)
    })
    versionListing.deleteMarker.forEach(o => {
      console.log(`${o.name}, ${o.versionId}`)
    })
  
    nextKeyMarker = versionListing.NextKeyMarker;
    nextVersionMarker = versionListing.NextVersionIdMarker;
  } while (versionListing.isTruncated);
}

main()
```

## List a specified number of object versions

The following code provides an example on how to list a specified number of object versions:

```
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
const accessKeyId = "<yourAccessKeyId>";
const accessKeySecret = "<yourAccessKeySecret>";
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
const endpoint = "httpss://oss-cn-hangzhou.aliyuncs.com";
const bucket = "<yourBucketName>";

// Create an OSSClient instance.
const ossClient = new OSS({
    accessKeyId,
  accessKeySecret,
  endpoint,
  bucket
});
// Specify to return up to 100 object versions.
const versionListing = await client.getBucketVersions({
  'max-keys': 100
})
// Display the version ID of the returned object versions. If versioning is not enabled for the bucket, the version IDs of the returned objects are none.
versionListing.objects.forEach(o => {
  console.log(`${o.name}, ${o.versionId}`)
})
versionListing.deleteMarker.forEach(o => {
  console.log(`${o.name}, ${o.versionId}`)
})
```

## List objects by folder

OSS does not use a hierarchical structure for objects, but instead uses a flat structure. All elements are stored as objects in buckets. A folder is an object whose size is 0 and whose name ends with a forward slash \(/\). You can upload and download this object. By default, objects whose names end with a forward slash \(/\) is displayed as a folder in the OSS console.

You can specify the delimiter and prefix parameters to list objects by folder.

-   If you set prefix to a folder name in the request, objects and subfolders whose names contain the prefix are listed.
-   If you also set delimiter to a forward slash \(/\) in the request, the objects and subfolders whose names start with the specified prefix in the folder are listed. Each subfolder is listed as a single result element in commonPrefixes. The objects and folders in these subfolders are not listed.

Example: The following four objects are stored in the bucket: oss.jpg, fun/test.jpg, fun/movie/001.avi, and fun/movie/007.avi. The forward slash \(/\) is specified as the folder delimiter. The following examples describe how to list objects in simulated folders.

-   List the versions of objects within the root folder of a bucket

    The following code provides an example on how to list the versions of objects within the root folder of a bucket:

    ```
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    const accessKeyId = "<yourAccessKeyId>";
    const accessKeySecret = "<yourAccessKeySecret>";
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    const endpoint = "httpss://oss-cn-hangzhou.aliyuncs.com";
    const bucket = "<yourBucketName>";
    
    // Create an OSSClient instance.
    const ossClient = new OSS({
    	accessKeyId,
      accessKeySecret,
      endpoint,
      bucket
    });
    // Set the delimiter parameter to a forward slash (/) to list the versions of objects and the names of subfolders within the root folder.
    async function main () {
      let nextKeyMarker = null;
      let nextVersionMarker = null;
      let versionListing = null;
      do {
        versionListing = await client.getBucketVersions({
          keyMarker: nextKeyMarker,
          versionIdMarker: nextVersionMarker,
          delimiter: '/'
        })
        nextKeyMarker = versionListing.NextKeyMarker;
        nextVersionMarker = versionListing.NextVersionIdMarker;
      } while (versionListing.isTruncated);
    }
    
    main()
    ```

    Returned results

    ```
    {
      res: {},
      objects: [
        {
          name: 'oss.jpg',
          versionId: 'xxx',
        }
      ],
      prefixes: ["fun/"],
      NextKeyMarker: null,
      NextVersionIdMarker: null,
      isTruncated: false
    }
    ```

-   List objects and subfolders within a folder

    The following code provides an example on how to list the objects and subfolders within a specified folder:

    ```
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    const accessKeyId = "<yourAccessKeyId>";
    const accessKeySecret = "<yourAccessKeySecret>";
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    const endpoint = "httpss://oss-cn-hangzhou.aliyuncs.com";
    const bucket = "<yourBucketName>";
    
    // Create an OSSClient instance.
    const ossClient = new OSS({
    	accessKeyId,
      accessKeySecret,
      endpoint,
      bucket
    });
    // Configure the prefix parameter to list all objects and subfolders within the fun folder. Set the delimiter parameter to a forward slash (/).
    async function main () {
      let nextKeyMarker = null;
      let nextVersionMarker = null;
      let versionListing = null;
      let prefix = 'fun/'
      do {
        versionListing = await client.getBucketVersions({
          keyMarker: nextKeyMarker,
          versionIdMarker: nextVersionMarker,
          prefix,
          delimiter: '/'
        })
        nextKeyMarker = versionListing.NextKeyMarker;
        nextVersionMarker = versionListing.NextVersionIdMarker;
      } while (versionListing.isTruncated);
    }
    
    main()
    ```

    Returned results

    ```
    {
      res: {},
      objects: [
        {
          name: 'test.jpg',
          versionId: 'xxx',
        }
      ],
      prefixes: ["fun/movie/"],
      NextKeyMarker: null,
      NextVersionIdMarker: null,
      isTruncated: false
    }
    ```


