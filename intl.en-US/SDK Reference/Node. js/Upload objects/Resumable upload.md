# Resumable upload

You can use resumable upload to split an object into several parts and upload them simultaneously. After all parts are uploaded, you can combine the uploaded parts into a complete object and complete the resumable upload task.

For more information, see [Multipart upload and resumable upload](/intl.en-US/Developer Guide/Objects/Upload files/Multipart upload and resumable upload.md). You can configure lifecycle rules to regularly delete parts that are no longer used. For more information, see [Manage parts](/intl.en-US/Console User Guide/Upload, download, and manage objects/Manage parts.md).

The progress parameter is provided to obtain the upload progress in callback. The SDK uses the upload progress and the checkpoint information as parameters in callback requests. In resumable upload, checkpoint information is recorded. When the upload task fails because of errors, you can pass the checkpoint information to multipartUpload as a parameter to continue the upload.

```
let OSS = require('ali-oss');

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>',
});

let checkpoint;
async function resumeUpload() {
  // retry 5 times
  for (let i = 0; i < 5; i++) {
    try {
      const result = await client.multipartUpload('object-name', filePath, {
        checkpoint,
        async progress(percentage, cpt) {
          checkpoint = cpt;
        },
      });
      console.log(result);
      break; // break if success
    } catch (e) {
      console.log(e);
    }
  }
}

resumeUpload();
        
```

In the preceding code, the checkpoint information is recorded as a variable. If the program crashes, the checkpoint information is lost. We recommend that you save the checkpoint information in a file. This way, the program can obtain the checkpoint information from the file after the program restarts.

