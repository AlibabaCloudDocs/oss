# CORS

Cross-origin resource sharing \(CORS\) allows web applications to access resources that belong to different regions. Object Storage Service \(OSS\) provides CORS operations to facilitate cross-origin access control.

**Note:** For the complete CORS rule code, visit [GitHub](https://github.com/aliyun/aliyun-oss-php-sdk/blob/master/samples/BucketCors.php). For more information, see the [CORS](/intl.en-US/Developer Guide/Buckets/Configure CORS.md) and [PutBucketcors](/intl.en-US/API Reference/Bucket operations/CORS/PutBucketCors.md) topics in OSS Developer Guide.

## Configure CORS rules

The following code provides an example on how to configure CORS rules for a bucket named examplebucket:

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
use OSS\Model\CorsConfig;
use OSS\Model\CorsRule;

// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use a Resource Access Management (RAM) user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
$accessKeyId = "yourAccessKeyId";
$accessKeySecret = "yourAccessKeySecret";
// Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
$endpoint = "yourEndpoint";
// Specify the bucket name. 
$bucket= "examplebucket";

$corsConfig = new CorsConfig();
$rule = new CorsRule();
// Specify the response headers based on which to allow cross-origin requests. You can specify multiple allowed headers. Only one asterisk (*) can be used as the wildcard for each allowed header. 
// If you do not have special requirements, we recommend that you set AllowedHeader to an asterisk (*). 
$rule->addAllowedHeader("*");
// Specify the response headers that you are allowed to access from applications. You can specify multiple exposed headers. Exposed headers cannot contain asterisks (*). 
$rule->addExposeHeader("x-oss-header");
// Specify the allowed origins from which cross-origin requests are sent. You can specify multiple allowed origins. Only one asterisk (*) can be used as the wildcard for each allowed origin. 
$rule->addAllowedOrigin("https://www.example.com:8080");
$rule->addAllowedOrigin("https://*.aliyun.com");
// If you set AllowedOrigin to an asterisk (*), requests from all origins are allowed. 
//$rule->addAllowedOrigin("*");
// Specify the allowed methods that are used to send cross-origin requests. 
$rule->addAllowedMethod("POST");
// Specify the time that the browser can cache the response to a preflight (OPTIONS) request to a specific resource. Unit: seconds. 
$rule->setMaxAgeSeconds(10);
// You can add up to 10 rules for each bucket. 
$corsConfig->addRule($rule);

try{
    $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);

    // The existing rules are replaced. 
    $ossClient->putBucketCors($bucket, $corsConfig);
} catch(OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}
print(__FUNCTION__ . ": OK" . "\n");            
```

## Query CORS rules

The following code provides an example on how to query the CORS rules configured for a bucket named examplebucket:

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

$corsConfig = null;
try{
    $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);

    $corsConfig = $ossClient->getBucketCors($bucket);
} catch(OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}
print(__FUNCTION__ . ": OK" . "\n");
print($corsConfig->serializeToXml() . "\n");            
```

## Delete CORS rules

The following code provides an example on how to delete all CORS rules configured for a bucket named examplebucket:

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

try{
    $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);

    $ossClient->deleteBucketCors($bucket);
} catch(OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}
print(__FUNCTION__ . ": OK" . "\n");            
```

