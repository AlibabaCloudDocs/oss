# Upload objects

This topic describes how to use simple upload or multipart upload to upload local files or data to Object Storage Service \(OSS\) buckets.

## Simple upload

The following section describes how to use putObject to upload file objects, Blob data, or OSS buffers to OSS. You cannot use progress functions in simple upload.

**Note:** Blob indicates large binary objects. For more information, see [Blob](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob).

The following code provides an example on how to upload data to an object named exampleobject.txt in a bucket named examplebucket.

For more information about how to set up a security token service \(STS\), see [Access OSS with a temporary access credential provided by STS](/intl.en-US/Developer Guide/Data security/Access and control/Access OSS with a temporary access credential provided by STS.md) in OSS Developer Guide. You can call the [AssumeRole](/intl.en-US/API Reference/API Reference (STS)/Operation interfaces/AssumeRole.md) operation or use [STS SDKs in various programming languages](/intl.en-US/SDK Reference/STS SDK Reference/STS SDK overview.md) to obtain a temporary access credential.

```
let OSS = require('ali-oss');

let client = new OSS({
    // Set yourRegion to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourRegion to oss-cn-hangzhou. 
    region: 'yourRegion',
    // Obtain a temporary access credential from STS. The temporary access credential contains a security token and a temporary AccessKey pair that consists of an AccessKey ID and an AccessKey secret. 
    accessKeyId: 'yourAccessKeyId',
    accessKeySecret: 'yourAccessKeySecret',
    stsToken: 'yourSecurityToken',
    // Specify the bucket name. 
    bucket: 'examplebucket'
  });

// You can upload file objects, Blob data, and OSS buffers to an OSS bucket. 
// Specify the full path of the local file to upload. By default, if you do not specify the local file, the local file is uploaded from the path of the project to which the sample program belongs. 
const data = 'D:\\localpath\\examplefile.txt';
// Specify the content to upload. 
//const data = new Blob('Hello OSS');
// Specify the content to upload. 
//const data = new OSS.Buffer('Hello OSS'));

async function putObject () {
  try {
    // Specify the full path of the object. The full path of the object cannot contain bucket names. 
    // You can customize an object name such as exampleobject.txt to upload data to the current bucket. You can also customize a directory named mytestdoc/exampleobject.txt to upload data to the specified directory in the bucket. 
    let result = await client.put('exampleobject.txt', data);
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}
putObject();
```

## Multipart upload

To upload a large object, you can call [MultipartUpload]() to perform multipart upload. In multipart upload, a local file is split into multiple parts. Then, separately upload the parts. If some parts fail to upload, you can continue the upload based on the recorded upload progress to upload only the parts that fail to upload. To upload an object larger than 100 MB, we recommend that you use multipart upload. This improves the upload success rate.

If the `ConnectionTimeoutError` occurs when you call the MultipartUpload operation, you must fix the error on your own. To fix the error, you can reduce the size of each part, extend the timeout period, or resend the request. You can also capture `ConnectionTimeoutError` to analyze the specific cause. For more information, see [Network connection timeout handling]().

**Note:**

-   The checkpoint parameter specifies a checkpoint file to record upload progress. To continue a resumable upload task, pass in the checkpoint parameter to the request.
-   We recommend that you create an OSS client instance when you perform multipart upload.
-   For more information about parameters for multipart upload, visit [GitHub](https://github.com/ali-sdk/ali-oss#multipartuploadname-file-options).

The following code provides an example on how to perform resumable upload by using multipart upload:

```
let OSS = require('ali-oss')

let client = new OSS({
    // Set yourRegion to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourRegion to oss-cn-hangzhou. 
    region: 'yourRegion',
    // Obtain a temporary access credential from STS. The temporary access credential contains a security token and a temporary AccessKey pair that consists of an AccessKey ID and an AccessKey secret. 
    accessKeyId: 'yourAccessKeyId',
    accessKeySecret: 'yourAccessKeySecret',
    stsToken: 'yourSecurityToken',
    // Specify the bucket name. 
    bucket: 'examplebucket'
  });

let client = new OSS(ossConfig);

let tempCheckpoint;

// Define the upload method. 
async function multipartUpload () {
  try {
    // Specify the full paths of the object and the local file. The full path of the object cannot contain bucket names. 
    // You can customize an object name such as exampleobject.txt to upload data to the current bucket or customize a directory name such as mytestdoc/exampleobject.txt to upload data to the specified directory in the bucket. 
    // By default, if you do not specify the path of the local file, the local file is uploaded from the path of the project to which the sample program belongs. 
    let result = await client.multipartUpload('exampleobject.txt', 'D:\\localpath\\examplefile.txt', { 
      progress: function (p, checkpoint) {
        // Set the checkpoint parameter. A resumable upload task cannot be automatically continued after the browser is restarted. You must manually continue the task. 
        tempCheckpoint = checkpoint;
      },
      meta: { year: 2020, people: 'test' },
      mime: 'text/plain'
   })
  } catch(e){
    console.log(e);
  }
}

// Start the multipart upload task. 
multipartUpload();

// Stop the multipart upload task. 
client.cancel();

// Resume the multipart upload task. 
let resumeclient = new OSS(ossConfig);
async function resumeUpload () {
  try {
    let result = await resumeclient.multipartUpload('exampleobject.txt', 'D:\\localpath\\examplefile.txt', {
    progress: function (p, checkpoint) {
          tempCheckpoint = checkpoint;
        },
        checkpoint: tempCheckpoint,
        meta: { year: 2020, people: 'test' },
        mime: 'text/plain'
  })
  } catch (e) {
    console.log(e);
  }
}

resumeUpload();          
```

-   progress specifies a progress callback function that is used to query the upload progress.

    ```
    const progress = function progress(p, checkpoint) {
      console.log(p)
    };                   
    ```

-   meta specifies the user metadata. You can call the HeadObject operation to obtain the object metadata. In addition, you must configure exposed headers in the OSS console. To configure exposed headers in the OSS console, find the required bucket. In the left-side navigation pane, choose Access Control \> **Cross-origin Resource Sharing \(CORS\)**. For more information, see [Configure CORS rules](/intl.en-US/Console User Guide/Manage buckets/Access control/Configure CORS rules.md).

    The following figure shows the response returned when the request is successful.

    ![fig_browserheader](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4596498951/p13703.png)

-   To obtain a callback for the request, you can add a `callback` field to the options parameter. The following code provides an example on the callback field:

    ```
    callback: {
      url: 'http://oss-demo.aliyuncs.com:23450',
      host: 'oss-cn-hangzhou.aliyuncs.com',
      /* eslint no-template-curly-in-string: [0] */
      body: 'bucket=${bucket}&object=${object}&var1=${x:var1}',
      contentType: 'application/x-www-form-urlencoded',
      customValue: {
        var1: 'value1',
        var2: 'value2',
      },
    },                    
    ```


