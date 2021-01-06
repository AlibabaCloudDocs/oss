# Manage symbolic links

A symbolic link works in a similar manner as the shortcut in Windows. You can configure a symbolic link that points to a frequently accessed object for quicker object access. In a versioned bucket, different versions of symbolic links can point to different objects.

## Create a symbolic link

By default, when you create a symbolic link in a versioned bucket and point it to an object, the symbolic link points to the current version of the object Symbolic links cannot be created for delete markers in versioned buckets.

The following code provides an example on how to create a symbolic link in a versioned bucket:

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
// Specify the bucket name.
$bucket= "<yourBucketName>";
// Specify the complete name of the object excluding the bucket name. Example: example/test.txt.
$object = "<yourObjectName>";
// Specify the name of the symbolic link that you want to create.
$symLink = "<yourSymLink>";

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);

try {
    // Create the symbolic link.
    $sym = $ossClient->putSymlink($bucket, $symlink, $object);
    
} catch (OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n");
```

For more information about how to create a symbolic link, see [PutSymlink](/intl.en-US/API Reference/Object operations/Symbolic link/PutSymlink.md).

## Query a symbolic link

By default, the `getSymlink` method is used to query the current version of a symbolic link. You can specify a version ID in the request to query the specified version of an symbolic link. If the current version of the queried symbolic link is a delete marker, OSS returns `404 Not Found` and includes the `x-oss-delete-marker = true` header and the version ID specified by the `x-oss-version-id` request header in the response.

The following code provides an example on how to query a symbolic link:

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
// Specify the bucket name.
$bucket= "<yourBucketName>";
// Specify the complete name of the object excluding the bucket name. Example: example/test.txt.
$object = "<yourObjectName>";
// Specify the version ID of the version that you want to query.
$versionId = "<yourObjectVersionId>";

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);

try {
    // Query the specified version of the symbolic link.
    $sym = $ossClient->getSymlink($bucket, $symlink, array(OssClient::OSS_VERSION_ID => $versionId));
    printf($sym['x-oss-version-id'] . "\n");
    printf($sym[OssClient::OSS_SYMLINK_TARGET] . "\n");
    
} catch (OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n");
```

For more information about how to query a symbolic link, see [GetSymlink](/intl.en-US/API Reference/Object operations/Symbolic link/GetSymlink.md).

