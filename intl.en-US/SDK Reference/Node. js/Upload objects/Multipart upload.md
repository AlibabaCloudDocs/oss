# Multipart upload

By using the multipart upload feature provided by OSS, you can split a large object into multiple parts and upload them separately. After all parts are uploaded, call the CompleteMultipartUpload operation to combine these parts into a single object to implement resumable upload.

## Background information

To upload a large file, you can call [MultipartUpload]() to perform multipart upload. In multipart upload, a file is divided into multiple parts for upload. If some parts fail to upload, you can continue the upload based on the recorded upload progress to upload only the parts fail to upload. We recommend that you use multipart upload to upload files larger than 100 MB to improve the upload success rate.

You must handle the `ConnectionTimeoutError` error by yourself when you call the MultipartUpload operation. You can handle timeout errors by reducing part size, increasing expiration period, sending the request again, or identifying `ConnectionTimeoutError` error messages. For more information, see [Network connection timeout handling]().

The following table describes the parameters that you can configure for multiple upload.

|Type|Parameter|Description|
|----|---------|-----------|
|Required parameters|name \{String\}|The name of the object. The object name includes the complete object path excluding the bucket name.|
|file \{String\|File\}|The path of the local file or HTML5 file to upload.|
|\[options\] \{Object\} Optional parameters|\[checkpoint\] \{Object\}|The checkpoint file that records the result of the multipart upload task. You must set this parameter if you want to perform resumable upload by using multipart upload. The checkpoint file stores the progress information about a multipart upload task. If a part fails to be uploaded, the task can be continued based on the progress recorded in the checkpoint file. After you have uploaded the file, the file is deleted.|
|\[parallel\] \{Number\}|The number of parts that can be uploaded concurrently. Default value: 5. Do not modify the value of this parameter unless necessary.|
|\[partSize\] \{Number\}|The size of each part to upload. Valid values: 100 KB to 5 GB. Default value: 1 MB. Do not modify the value of this parameter unless necessary.|
|\[progress\] \{Function\}|The callback function used to query the progress of the upload task. The callback function can be an async function. The callback function includes the following parameters: -   percentage \{Number\}: the progress of the upload task in percentage. Valid values: decimals from 0 to 1.
-   checkpoint \{Object\}: the checkpoint file that records the result of multipart upload.
-   res \{Object\}: the response returned when a part is uploaded. |
|\[meta\] \{Object\}|The user metadata, which is prefixed with `x-oss-meta`.|
|\[mime\] \{String\}|The Content-Type request header.|
|\[headers\] \{Object\}|Other headers. For more information, visit [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616.html). Some headers are described as follows: -   Cache-Control: specifies the caching behavior by using specified commands in HTTP requests or responses. Example: `Cache-Control: public, no-cache`.
-   Content-Disposition: specifies whether the returned content is displayed as a web page or downloaded as an attachment. Example: `Content-Disposition: somename`.
-   Content-Encoding: the method to compress data of the specified media type. Example: `Content-Encoding: gzip`.
-   Expires: the validity period of the request. Unit: milliseconds. |

The sample code in this topic uses the catch method. To obtain the syntax of this method, learn the Promise function defined in ECMAScript 6 and the async and await methods of this function. For more information about how to use OSS SDKs, see [Installation](/intl.en-US/SDK Reference/Node. js/Installation.md).

## Complete sample code

The following code provides a complete example that describes the process of multipart upload:

```
const OSS = require('ali-oss');

const client = new OSS({
  // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
  region: '<Your region>',
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>',
});

const progress = (p, _checkpoint) => {
  console.log(p); // The upload progress of the object.
  console.log(_checkpoint); // The checkpoint information of the multipart upload task.
};

// Start the multipart upload task.
async function multipartUpload() {
  try {
    // Set object-name to a name such as file.txt or a folder such as abc/test/file.txt to upload the object to the root folder or the specified folder of the bucket.
    const result = await client.multipartUpload('<Object Name>', 'file-object', {
      progress,
      // meta specifies the user metadata. You can query the object metadata by calling the HeadObject operation.
      meta: {
        year: 2020,
        people: 'test',
      },
    });
    console.log(result);
    const head = await client.head('object-name');
    console.log(head);
  } catch (e) {
    // Handle timeout exceptions.
    if (e.code === 'ConnectionTimeoutError') {
      console.log('TimeoutError');
      // do ConnectionTimeoutError operation
    }
    console.log(e);
  }
}

multipartUpload();
```

**Note:** OSS SDK for Node.js does not support MD5 verification in multipart upload tasks. We recommend that you call the CRC64 library to determine whether to perform CRC64 checks after the multipart upload task is complete.

## Cancel a multipart upload task

You can call `client.AbortMultipartUpload` to cancel a multipart upload task. If you cancel a multipart upload task, you cannot use the upload ID to upload any parts. The uploaded parts are deleted.

The following code provides an example on how to cancel a multipart upload task:

```
const OSS = require('ali-oss');

const client = new OSS({
  // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
  region: '<Your region>',
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>',
});

async function abortMultipartUpload() {
  const name = '<Object Name>'; // The full path of the object in a bucket.
  const uploadId = '<Upload Id>'; // The upload ID of the multipart upload task.
  const result = await client.abortMultipartUpload(name, uploadId);
  console.log(result);
}

abortMultipartUpload();
```

## List multipart upload tasks

The following sample code provides an example on how to call `client.listUploads` to list multipart upload tasks:

```
const OSS = require('ali-oss');

const client = new OSS({
  // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
  region: '<Your region>',
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>',
});

async function listUploads(query = {}) {
  // You can configure parameters such as prefix, marker, delimiter, upload-id-marker, and max-uploads for query.
  const result = await client.listUploads(query);

  result.uploads.forEach(upload => {
    console.log(upload.uploadId); // The upload ID of the multipart upload task.
    console.lo g(upload.name); // Combine all uploaded parts into a complete object and specify the full path of the object in a bucket.
  });
}

const query = {
  'max-uploads': 1000, // Specify the maximum number of multipart upload tasks to return at a time. The default value and the maximum value of max-uploads are both 1000.
};
listUploads(query);
```

## List uploaded parts

You can call the `client.listParts` method to list all parts that are uploaded by using the specified upload ID.

-   Simple list

    The following code provides an example on how to list uploaded parts:

    ```
    const OSS = require('ali-oss');
    
    const client = new OSS({
      // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
      region: '<Your region>',
      // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
      accessKeyId: '<Your AccessKeyId>',
      accessKeySecret: '<Your AccessKeySecret>',
      bucket: '<Your bucket name>',
    });
    
    async function listParts() {
      const query = {
        'max-parts': 1000, // Specify the maximum number of parts that can be returned in the OSS response. The default value and the maximum value of max-uploads are both 1000.
      };
      const result = await client.listParts('<Object Name>', '<Upload Id>', query);
    
      result.parts.forEach(part => {
        console.log(part.PartNumber);
        console.log(part.LastModified);
        console.log(part.ETag);
        console.log(part.Size);
      });
    }
    
    listParts();
    ```

-   List all uploaded parts

    By default, `client.listParts` can list up to 1,000 parts at a time. The following code provides an example on how to list more than 1,000 parts:

    ```
    const OSS = require('ali-oss')
    
    const client = new OSS({
      // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
      region: '<Your region>',
      // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
      accessKeyId: '<Your AccessKeyId>',
      accessKeySecret: '<Your AccessKeySecret>',
      bucket: '<Your bucket name>',
    });
    
    async function listParts() {
      const query = {
        'max-parts': 1000, // Specify the maximum number of parts that can be returned in the OSS response. The default value and the maximum value of max-uploads are both 1000.
      };
      let result;
      do {
        result = await client.listParts('<Object Name>', '<Upload Id>', query);
        // Set the position from which the list begins.
        query['part-number-marker'] = result.nextPartNumberMarker;
        result.parts.forEach(part => {
          console.log(part.PartNumber);
          console.log(part.LastModified);
          console.log(part.ETag);
          console.log(part.Size);
        });
      } while (result.isTruncated === "true");
    }
    
    listParts();
    ```


