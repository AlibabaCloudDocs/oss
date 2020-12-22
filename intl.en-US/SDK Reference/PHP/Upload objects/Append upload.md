# Append upload

## Upload a local file by using append upload

The following code provides an example on how to upload a local file to a specified bucket by using append upload:

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
$endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Configure the bucket name.
$bucket= "<yourBucketName>";
// Configure the object name. The object name indicates the complete path of the object excluding the bucket name. Example: example/folder/abc.jpg.
$object = "<yourObjectName>";
// Obtain the first local file. Specify the complete path of the local file. Example: /users/local/abc.jpg.
$filePath = "<yourLocalFile1>";
// Obtain the second local file. Specify the complete path of the local file. Example: /users/local/example.txt.
$filePath1 = "<yourLocalFile2>";
try{
    $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);

    $position = $ossClient->appendFile($bucket, $object, $filePath, 0);
    $position = $ossClient->appendFile($bucket, $object, $filePath1, $position);
} catch(OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}
print(__FUNCTION__ . ": OK" . "\n");
            
```

For more information about the naming conventions for buckets, see [bucket](/intl.en-US/Developer Guide/Terms.md). For more information about the naming conventions for objects, see [object](/intl.en-US/Developer Guide/Terms.md).

## Upload a string by using append upload

The following code provides an example on how to upload a string to a specified bucket by using append upload:

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
// Configure the bucket name.
$bucket= "<yourBucketName>";
// Configure the object name. The object name indicates the complete path of the object excluding the bucket name. Example: example/folder/abc.jpg.
$object = "<yourObjectName>";
// Specifies that the object content obtained after the first, second, and third append upload is Hello OSS, Hi OSS, and OSS OK.
$content_array = array('Hello OSS', 'Hi OSS', 'OSS OK');
try{
    $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);

    // Perform the first append operation. If the object is appended for the first time, the append position is 0. The position for the next append operation is included in the response. The position from which the next append operation starts is the current length of the object.
    $position = $ossClient->appendObject($bucket, $object, $content_array[0], 0);    
    $position = $ossClient->appendObject($bucket, $object, $content_array[1], $position);   
    $position = $ossClient->appendObject($bucket, $object, $content_array[2], $position);
} catch(OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}
print(__FUNCTION__ . ": OK" . "\n");
            
```

