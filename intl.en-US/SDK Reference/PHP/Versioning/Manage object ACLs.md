# Manage object ACLs

OSS provides the following four access control lists \(ACLs\) for objects to control access to objects: public-read-write, public-read, private, and default. This topic describes how to manage the ACLs of objects in a versioned bucket.

## Object ACLs

The following table describes the four ACLs of objects.

|ACL|Description|
|---|-----------|
|public-read-write|Any users, including anonymous users can read and write the object.|
|public-read|The object is a public read resource. Only the owner or authorized users of the object can write the object. Other users, including anonymous users can only read the object.|
|private|The object is a private resource. Only the owner or authorized users of the object can read and write the object.|
|default|The ACL of an object is the same as that of the bucket in which the object is stored.|

The ACL of an object takes precedence over the ACL of the bucket in which the object is stored. For example, if the ACL of a bucket is public read and the ACL of an object in the bucket is private, only the owner or authorized users of the object can read and write the object.

## Configure object ACLs

By default, the `putObjectAcl` method is used to configure the ACL of the current version of an object. If the current version of the object is a delete marker, OSS returns 404 Not Found. You can specify a version ID in the request to configure the ACL of the specified version of an object.

The following code provides an example on how to configure the ACL of an object:

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
// Specify the ID of the version for which you want to configure ACL.
$versionId = "<yourObjectVersionId>";

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);

try {
    // Set the ACL of the specified version of the object to public-read.
    $ossClient->putObjectAcl($bucket, $object, 'public-read', array(OssClient::OSS_VERSION_ID => $versionId));
  
} catch (OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n");
```

For more information about how to configure object ACLs, see [PutObjectACL](/intl.en-US/API Reference/Object operations/ACL/PutObjectACL.md).

## Query object ACLs

By default, the `getObjectAcl` method is used to query the ACL of the current version of an object. If the current version of the object is a delete marker, OSS returns 404 Not Found. You can specify a version ID in the request to query the ACL of the specified version of an object.

The following code provides an example on how to query the ACL of an object:

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
// Specify the ID of the version that you want to query.
$versionId = "<yourObjectVersionId>";

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);

try {
    // Query the ACL of the object.
    $acl = $ossClient->getObjectAcl($bucket, $object, array(OssClient::OSS_VERSION_ID => $versionId));
    printf($acl . "\n");

} catch (OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n");
```

For more information about how to query object ACLs, see [GetObjectACL](/intl.en-US/API Reference/Object operations/ACL/GetObjectACL.md)

