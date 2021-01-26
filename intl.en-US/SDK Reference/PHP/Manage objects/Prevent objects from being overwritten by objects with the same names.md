# Prevent objects from being overwritten by objects with the same names

By default, if you upload an object that has the same name as an existing object, the existing object is overwritten by the uploaded object. This topic describes how to set the x-oss-forbid-overwrite request header to prevent objects from being overwritten by objects with the same names in simple upload and multipart upload.

## Simple upload

The following code provides an example on how to prevent objects from being overwritten by objects with the same names in simple upload:

```
<? php
if (is_file(__DIR__ . '/../autoload.php')) {
    require_once __DIR__ . '/../autoload.php';
}
if (is_file(__DIR__ . '/../vendor/autoload.php')) {
    require_once __DIR__ . '/../vendor/autoload.php';
}

use OSS\OssClient;
use OSS\Core\OssException;

// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
$accessKeyId = "<yourAccessKeyId>";
$accessKeySecret = "<yourAccessKeySecret>";
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
$endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
$bucket= "<yourBucketName>";
$object = "<yourObjectName>";
$content = "Hello OSS";

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);

try{

   // Specify whether the PutObject operation overwrites the object with the same name.
   // By default, if x-oss-forbid-overwrite is not specified, existing objects are overwritten by the uploaded objects that have the same name.
   // If x-oss-forbid-overwrite is set to false, existing objects are overwritten by the uploaded objects that have the same name.
   // If x-oss-forbid-overwrite is set to true, existing objects cannot be overwritten by objects that have the same name. If you upload an object that has the same name as an existing object, OSS returns FileAlreadyExists.
    $options = array(
        OssClient::OSS_HEADERS => array(
            'x-oss-forbid-overwrite' => 'true'
        ),
    );
    
    $ossClient->putObject($bucket, $object, $content, $options);
} catch(OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}
print(__FUNCTION__ . ": OK" . "\n");
```

## Multipart upload

The following code provides an example on how to prevent objects from being overwritten by objects with the same names in multipart upload:

```
<? php
if (is_file(__DIR__ . '/../autoload.php')) {
    require_once __DIR__ . '/../autoload.php';
}
if (is_file(__DIR__ . '/../vendor/autoload.php')) {
    require_once __DIR__ . '/../vendor/autoload.php';
}

use OSS\OssClient;
use OSS\Core\OssException;
use OSS\Core\OssUtil;

// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
$accessKeyId = "<yourAccessKeyId>";
$accessKeySecret = "<yourAccessKeySecret>";
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
$endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
$bucket= "<yourBucketName>";
$object = "<yourObjectName>";
$uploadFile = "<yourLocalFile>";

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);

try{

    // Specify whether existing objects are overwritten by objects with the same names when the multipart upload task is initialized.    
    // By default, if x-oss-forbid-overwrite is not specified, existing objects are overwritten by the uploaded objects that have the same name.
    // If x-oss-forbid-overwrite is set to false, existing objects are overwritten by the uploaded objects that have the same name.
    // If x-oss-forbid-overwrite is set to true, existing objects cannot be overwritten by objects that have the same name. If you upload an object that has the same name as an existing object, OSS returns FileAlreadyExists.
    $options = array(
        OssClient::OSS_HEADERS => array(
            'x-oss-forbid-overwrite' => 'true'
        ),
    );
    $uploadId = $ossClient->initiateMultipartUpload($bucket, $object, $options);

    // Upload the parts.
    $partSize = 1 * 1024 * 1024;
    $uploadFileSize = filesize($uploadFile);
    $pieces = $ossClient->generateMultiuploadParts($uploadFileSize, $partSize);
    $responseUploadPart = array();
    $uploadPosition = 0;
    $isCheckMd5 = true;
    foreach ($pieces as $i => $piece) {
        $fromPos = $uploadPosition + (integer)$piece[$ossClient::OSS_SEEK_TO];
        $toPos = (integer)$piece[$ossClient::OSS_LENGTH] + $fromPos - 1;
        $upOptions = array(
            $ossClient::OSS_FILE_UPLOAD => $uploadFile,
            $ossClient::OSS_PART_NUM => ($i + 1),
            $ossClient::OSS_SEEK_TO => $fromPos,
            $ossClient::OSS_LENGTH => $toPos - $fromPos + 1,
        );

        $responseUploadPart[] = $ossClient->uploadPart($bucket, $object, $uploadId, $upOptions);
    }
    $uploadParts = array();
    foreach ($responseUploadPart as $i => $eTag) {
        $uploadParts[] = array(
            'PartNumber' => ($i + 1),
            'ETag' => $eTag,
        );
    }    
    
    // Specify whether existing objects are overwritten by objects with the same names when the multipart upload task is complete.
    // By default, if x-oss-forbid-overwrite is not specified, existing objects are overwritten by the uploaded objects that have the same name.
    // If x-oss-forbid-overwrite is set to false, existing objects are overwritten by the uploaded objects that have the same name.
    // If x-oss-forbid-overwrite is set to true, existing objects cannot be overwritten by objects that have the same name. If you upload an object that has the same name as an existing object, OSS returns FileAlreadyExists.
    $options = array(
        OssClient::OSS_HEADERS => array(
            'x-oss-forbid-overwrite' => 'true'
        ),
    );
    $ossClient->completeMultipartUpload($bucket, $object, $uploadId, $uploadParts, $options);

} catch(OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n");
```

