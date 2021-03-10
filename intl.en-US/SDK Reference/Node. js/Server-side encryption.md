# Server-side encryption

OSS supports server-side encryption for uploaded data. When you upload data, OSS encrypts the data and stores the encrypted data. When you download data, OSS decrypts the data and returns the original data. The returned HTTP request header declares that the data is encrypted on the server.

## Background information

OSS provides the following server-side encryption methods:

-   Server-side encryption by using CMKs managed by KMS \(SSE-KMS\)

    When you upload an object, you can use a specified CMK ID or the default CMK managed by KMS to encrypt and decrypt a large amount of data. This method is cost-effective because you do not need to send user data to the KMS server by using networks for encryption and decryption.

    **Note:** You are charged for calling API operations when you use CMKs to encrypt or decrypt data.

-   Server-side encryption by using OSS-managed keys \(SSE-OSS\)

    When you upload an object, OSS encrypts the object on the server side by using AES-256 managed by OSS. OSS server-side encryption uses AES-256 to encrypt objects by using different data keys. AES-256 uses master keys that are regularly rotated to encrypt data keys.


**Note:**

-   Only one server-side encryption method can be used for an object at a time.
-   If you configure server-side encryption for a bucket, you can still configure the encryption method for a single object when you upload or copy it. In this case, the encryption method configured for the object takes precedence. For more information, see [PutObject](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.md).
-   For more information about server-side encryption, see [Server-side encryption](/intl.en-US/Developer Guide/Data security/Data encryption/Server-side encryption.md).

## Configure server-side encryption for a bucket

The following code provides an example on how to configure the default encryption method for a bucket. After the method is configured, all objects that are uploaded to the bucket without encryption methods configured are encrypted by using the default encryption method.

```
const OSS = require("ali-oss");

const store = new OSS({
  region: "<yourRegion>",
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
  accessKeyId: "<yourAccessKeyId>",
  accessKeySecret: "<yourAccessKeySecret>",
  bucket: "<yourBucketName>"
});

async function putBucketEncryption() {
  try {
    // Configure an encryption method for the bucket.    

    let result = await store.putBucketEncryption("bucket-name", {
      SSEAlgorithm: "AES256", // The AES-256 encryption method is configured here as an example. To use KMS for encryption, you must configure KMSMasterKeyID.
      // KMSMasterKeyID: "<yourKMSMasterKeyId>". Configure the CMK ID when SSEAlgorithm is set to KMS and a specified CMK is used for encryption. In other cases, this element must be set to null.
    });
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

putBucketEncryption();
```

For more information about how to configure server-side encryption for a bucket, see [PutBucketEncryption](/intl.en-US/API Reference/Bucket operations/Encryption/PutBucketEncryption.md).

## Query the server-side encryption configurations of a bucket

The following code provides an example on how to query the server-side encryption configurations of a bucket:

```
const OSS = require("ali-oss");

const store = new OSS({
  region: "<yourRegion>",
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
  accessKeyId: "<yourAccessKeyId>",
  accessKeySecret: "<yourAccessKeySecret>",
  bucket: "<yourBucketName>"
});

async function getBucketEncryption() {
  try {
    let result = await store.getBucketEncryption("bucket-name");
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

getBucketEncryption();
```

For more information about how to query the server-side encryption configurations of a bucket, see [GetBucketEncryption](/intl.en-US/API Reference/Bucket operations/Encryption/GetBucketEncryption.md).

## Delete the server-side encryption configurations of a bucket

The following code provides an example on how to delete the server-side encryption configurations of a bucket:

```
const OSS = require("ali-oss");

const store = new OSS({
  region: "<yourRegion>",
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
  accessKeyId: "<yourAccessKeyId>",
  accessKeySecret: "<yourAccessKeySecret>",
  bucket: "<yourBucketName>"
});

async function deleteBucketEncryption() {
  try {
    // Delete the server-side encryption configurations of the bucket.
    let result = await store.deleteBucketEncryption("bucket-name");
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

deleteBucketEncryption();
```

For more information about how to delete the server-side encryption configurations of a bucket, see [DeleteBucketEncryption](/intl.en-US/API Reference/Bucket operations/Encryption/DeleteBucketEncryption.md).

