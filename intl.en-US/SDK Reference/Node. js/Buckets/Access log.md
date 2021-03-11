# Access log

You can enable access log to record bucket access to log files, which are stored in a specified bucket

Log format:

```
<TargetPrefix><SourceBucket>-YYYY-mm-DD-HH-MM-SS-UniqueString
		
```

For more information about access log, see the "[Log storage](/intl.en-US/Developer Guide/Manage logs/Log storage.md).

## Enable access log

Use `putBucketLogging` to enable access log of the bucket.

```
let OSS = require('ali-oss')
let client = new OSS({
  region: '<Your region>'
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>'
});
async function putBucketLogging () {
  try {
     let result = await client.putBucketLogging('bucket-name', 'logs/');
     console.log(result)
  } catch (e) {
    console.log(e)
  }
}
putBucketLogging();
```

## View configurations of access log

Use `getBucketLogging` to view configurations of access log.

```language-js
let OSS = require('ali-oss')

let client = new OSS({
  region: '<Your region>'
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>'
});

async function getBucketLogging() {
  try {
     let result = await client.getBucketLogging('bucket-name');
    console.log(result);
  } catch (e) {
    console.log(e);
  }
})

getBucketLogging();
			
```

## Disable access log

Use `deleteBucketLogging` to disable access log of the bucket.

```language-js
let OSS = require('ali-oss')

let client = new OSS({
  region: '<Your region>'
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>'
});

async function deleteBucketLogging () {
  try {
    let result = await client.deleteBucketLogging('bucket-name');
    console.log(result);
  } catch (e) {
    console.log(e);
  }

deleteBucketLogging();
			
```

