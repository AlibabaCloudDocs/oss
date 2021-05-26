# Append upload

You can use AppendObject to append content to appendable objects that are uploaded.

## Usage notes

When you call AppendObject to upload an object, take note of the following items:

-   If the object to append does not exist, an append object is created.
-   If the object to append is an existing append object and the specified position from which the append operation starts is not equal to the current object size, the PositionNotEqualToLength error is returned. If the object to append is a non-append object, the ObjectNotAppendable error is returned.
-   The CopyObject operation cannot be performed on append objects.

For more information about how to perform append upload, see [AppendObject](/intl.en-US/API Reference/Object operations/Basic operations/AppendObject.md).

## Upload a local file by using append upload

The following code provides an example on how to use append upload to upload the content of examplefilea.txt, examplefileb.txt, and examplefilec.txt in sequence to the exampleobject.txt object in the examplebucket bucket:

```
 <?php
if (is_file(__DIR__ . '/../autoload.php')) {
    require_once __DIR__ . '/../autoload.php';
}
if (is_file(__DIR__ . '/../vendor/autoload.php')) {
    require_once __DIR__ . '/../vendor/autoload.php';
}

use OSS\OssClient;
use OSS\Core\OssException;

// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use a Resource Access Management (RAM) user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
$accessKeyId = "yourAccessKeyId";
$accessKeySecret = "yourAccessKeySecret";
// Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
$endpoint = "yourEndpoint";
// Specify the bucket name. 
$bucket= "examplebucket";
// Specify the full path of the object. The full path of the object cannot contain bucket names. 
$object = "exampleobject.txt";
// Specify the full paths of the local files to upload. If the paths of the local files are not specified, the local files are uploaded from the path of the project to which the sample program belongs. 
$filePath = "D:\\localpath\\examplefilea.txt";
$filePath1 = "D:\\localpath\\examplefileb.txt";
$filePath1 = "D:\\localpath\\examplefilec.txt";

try{
    $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);
    // Perform the first append upload. If the object is appended for the first time, the append position is 0. The position for the next append operation is included in the response. The position from which the next append operation starts is the current length of the object. 
    $position = $ossClient->appendFile($bucket, $object, $filePath, 0);
    $position = $ossClient->appendFile($bucket, $object, $filePath1, $position);
    $position = $ossClient->appendFile($bucket, $object, $filePath2, $position);
} catch(OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}
print(__FUNCTION__ . ": OK" . "\n");            
```

For more information about the naming conventions for buckets, see [bucket](/intl.en-US/Developer Guide/Terms.md). For more information about the naming conventions for objects, see [object](/intl.en-US/Developer Guide/Terms.md).

## Upload strings by using append upload

The following code provides an example on how to use append upload to upload strings in sequence to the strexampleobject.txt object in the examplebucket bucket:

```
<?php
if (is_file(__DIR__ . '/../autoload.php')) {
    require_once __DIR__ . '/../autoload.php';
}
if (is_file(__DIR__ . '/../vendor/autoload.php')) {
    require_once __DIR__ . '/../vendor/autoload.php';
}

use OSS\OssClient;
use OSS\Core\OssException;

// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
$accessKeyId = "yourAccessKeyId";
$accessKeySecret = "yourAccessKeySecret";
// Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
$endpoint = "yourEndpoint";
// Specify the bucket name. 
$bucket= "examplebucket";
// Specify the full path of the object. The full path of the object cannot contain bucket names. 
$object = "strexampleobject.txt";
// Specify the strings you want to upload in sequence. Specify that the object content obtained after the first append upload is Hello OSS, after the second append upload is Hi OSS, and after the third append upload is OSS OK. 
$content_array = array('Hello OSS', 'Hi OSS', 'OSS OK');

try{
    $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);

    // Perform the first append upload. If the object is appended for the first time, the append position is 0. The position for the next append operation is included in the response. The position from which the next append operation starts is the current length of the object. 
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

