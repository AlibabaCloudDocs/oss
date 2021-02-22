# Client-side encryption

When you perform client-side encryption, objects are encrypted on the local client before they are uploaded to OSS.

## Disclaimer

-   When you use client-side encryption, you must ensure the integrity and validity of the Customer Master Key \(CMK\). If the CMK is used incorrectly or lost due to improper maintenance, you will be liable for any losses or consequences caused by decryption failures.
-   When you copy or migrate encrypted data, you must ensure the integrity and validity of the object metadata related to client-side encryption. If the object metadata related to client-side encryption is used incorrectly or lost due to improper maintenance, you will be liable for any losses or consequences caused by decryption failures.

## Background information

In client-side encryption, a random data key is generated for each object to perform symmetric encryption on the object. The client uses a CMK to encrypt the random data key. The encrypted data key is uploaded as a part of the object metadata and stored in the OSS server. When an encrypted object is downloaded, the client uses the CMK to decrypt the random data key and then uses the data key to decrypt the object. The CMK is used only on the client and is not transmitted over the network or stored in the server, which ensures data security.

**Note:**

-   Client-side encryption supports multipart upload for objects larger than 5 GB. When you use multipart upload to upload an object, you must specify the total size of the object and the size of each part. The size of each part except for the last part must be the same and be a multiple of 16 bytes.
-   After you upload objects encrypted on the client, object metadata related to client-side encryption is protected and you cannot call [CopyObject](/intl.en-US/API Reference/Object operations/Basic operations/CopyObject.md) to modify object metadata related to client-side encryption.

## Encryption methods

You can use CMKs managed in one of the following ways:

-   Use CMKs managed by Key Management Service \(KMS\)

    When you use a CMK stored in KMS for client-side encryption, you must send the CMK ID to OSS SDK.

-   Use CMKs managed by yourself \(RSA\)

    When you use a CMK managed by yourself for client-side encryption, you must send the public key and the private key of your CMK to OSS SDK as parameters.


You can use the preceding methods to prevent data leaks and protect your data on the client. Even if your data is leaked, it cannot be decrypted by others.

## Object metadata related to client-side encryption

|Parameter|Description|Required|
|:--------|:----------|:-------|
|x-oss-meta-client-side-encryption-key|The encrypted data key, which is a string encrypted by using the CMK and encoded in Base64.|Yes|
|x-oss-meta-client-side-encryption-start|The initial value which is randomly generated for data encryption. The initial value is a string encrypted by using the CMK and encoded in Base64.|Yes|
|x-oss-meta-client-side-encryption-cek-alg|The encryption algorithm used to encrypt data.|Yes|
|x-oss-meta-client-side-encryption-wrap-alg|The encryption algorithm used to encrypt data keys.|Yes|
|x-oss-meta-client-side-encryption-matdesc|The description of the CMK in JSON format. **Warning:** We recommend that you configure a description for each CMK and store the mapping relationship between the CMK and its description. CMKs of which the description is not specified cannot be replaced.

|No|
|x-oss-meta-client-side-encryption-unencrypted-content-length|The length of data before encryption. This parameter is not generated if Content-Length is not specified.|No|
|x-oss-meta-client-side-encryption-unencrypted-content-md5|The MD5 hash of the data before encryption. This parameter is not generated if Content-MD5 is not specified.|No|
|x-oss-meta-client-side-encryption-data-size|The total size of the data that needs to encrypt for multipart upload when init\_multipart is called.|Yes \(for multipart upload\)|
|x-oss-meta-client-side-encryption-part-size|The size of each part that needs to encrypt for multipart upload when init\_multipart is called. **Note:** The size of each part must be an integral multiple of 16 bytes.

|Yes \(for multipart upload\)|

The following section provides complete sample code on how to use RSA-based CMKs managed by yourself to perform client-side encryption in the following scenarios: upload objects from memory or local files, resumable upload, multipart upload, and download objects to local files.

## Encrypt objects to upload from memory

The following code provides an example on how to use an RSA-based CMK managed by yourself to encrypt an object to upload from memory:

```
#include <alibabacloud/oss/OssEncryptionClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initialize the OSS account information.*/
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";
    std::string ObjectName = "yourObjectName";

    /* Specify the CMK and the description. */
    std::string RSAPublicKey = "your rsa public key";
    std::string RSAPrivateKey = "your rsa private key";
    std::map<std::string, std::string> desc;
    desc["comment"] = "your comment";

    /* Initialize network resources.*/
    InitializeSdk();
    ClientConfiguration conf;
    CryptoConfiguration cryptoConf;
    auto materials = std::make_shared<SimpleRSAEncryptionMaterials>(RSAPublicKey, RSAPrivateKey, desc);
    OssEncryptionClient client(Endpoint, AccessKeyId, AccessKeySecret, conf, materials, cryptoConf);
    std::shared_ptr<std::iostream> content = std::make_shared<std::stringstream>();
    *content << "Thank you for using Aliyun Object Storage Service!" ;
    PutObjectRequest request(BucketName, ObjectName, content);
    /* Upload the object */
    auto outcome = client.PutObject(request);
    if (! outcome.isSuccess()) {
        /* Handle exceptions.*/
        std::cout << "PutObject fail" <<
        ",code:" << outcome.error().Code() <<
        ",message:" << outcome.error().Message() <<
        ",requestId:" << outcome.error().RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }
    /* Release network resources.*/
    ShutdownSdk();
    return 0;
}
```

## Encrypt objects to upload from a local file

The following code provides an example on how to use an RSA-based CMK managed by yourself to encrypt an object to upload from a local file:

```
#include <alibabacloud/oss/OssEncryptionClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initialize the OSS account information.*/
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";
    std::string ObjectName = "yourObjectName";

    /* Specify the CMK and the description. */
    std::string RSAPublicKey = "your rsa public key";
    std::string RSAPrivateKey = "your rsa private key";
    std::map<std::string, std::string> desc;
    desc["comment"] = "your comment";

    /* Initialize network resources.*/
    InitializeSdk();
    ClientConfiguration conf;
    CryptoConfiguration cryptoConf;
    auto materials = std::make_shared<SimpleRSAEncryptionMaterials>(RSAPublicKey, RSAPrivateKey, desc);
    OssEncryptionClient client(Endpoint, AccessKeyId, AccessKeySecret, conf, materials, cryptoConf);
    /* Upload the object. */
    auto outcome = client.PutObject(BucketName, ObjectName, "yourLocalFilename");
    if (! outcome.isSuccess()) {
        /* Handle exceptions.*/
        std::cout << "PutObject fail" <<
        ",code:" << outcome.error().Code() <<
        ",message:" << outcome.error().Message() <<
        ",requestId:" << outcome.error().RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }
    /* Release network resources.*/
    ShutdownSdk();
    return 0;
}
```

## Encrypt objects to upload by using resumable upload

The following code provides an example on how to use an RSA-based CMK managed by yourself to encrypt an object to upload by using resumable upload:

```
#include <alibabacloud/oss/OssEncryptionClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initialize the OSS account information.*/
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";
    std::string ObjectName = "yourObjectName";
    std::string UploadFilePath = "yourUploadfilePath";
    std::string CheckpointFilePath = "yourCheckpointFilepath";

    /* Specify the CMK and the description. */
    std::string RSAPublicKey = "your rsa public key";
    std::string RSAPrivateKey = "your rsa private key";
    std::map<std::string, std::string> desc;
    desc["comment"] = "your comment";

    /* Initialize network resources.*/
    InitializeSdk();
    ClientConfiguration conf;
    CryptoConfiguration cryptoConf;
    auto materials = std::make_shared<SimpleRSAEncryptionMaterials>(RSAPublicKey, RSAPrivateKey, desc);
    OssEncryptionClient client(Endpoint, AccessKeyId, AccessKeySecret, conf, materials, cryptoConf);

    /* Start resumable upload. */
    UploadObjectRequest request(BucketName, ObjectName, UploadFilePath, CheckpointFilePath);
    auto outcome = client.ResumableUploadObject(request);

    if (! outcome.isSuccess()) {
        /* Handle exceptions.*/
        std::cout << "ResumableUploadObject fail" <<
        ",code:" << outcome.error().Code() <<
        ",message:" << outcome.error().Message() <<
        ",requestId:" << outcome.error().RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }
    /*Release network resources.*/
    ShutdownSdk();
    return 0;
}
```

## Encrypt objects to upload by using multipart upload

The following code provides an example on how to use an RSA-based CMK managed by yourself to encrypt an object to upload by using multipart upload:

```
#include <alibabacloud/oss/OssEncryptionClient.h>
#include <fstream>
using namespace AlibabaCloud::OSS;

static int64_t getFileSize(const std::string& file)
{
    std::fstream f(file, std::ios::in | std::ios::binary);
    f.seekg(0, f.end);
    int64_t size = f.tellg();
    f.close();
    return size;
}

int main(void)
{
    /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";
    std::string ObjectName = "yourObjectName";
    std::string fileToUpload = "yourLocalFilename";

    /* Specify the CMK and the description. */
    std::string RSAPublicKey = "your rsa public key";
    std::string RSAPrivateKey = "your rsa private key";
    std::map<std::string, std::string> desc;
    desc["comment"] = "your comment";

    /*Initialize network resources.*/
    InitializeSdk();
    ClientConfiguration conf;
    CryptoConfiguration cryptoConf;
    auto materials = std::make_shared<SimpleRSAEncryptionMaterials>(RSAPublicKey, RSAPrivateKey, desc);
    OssEncryptionClient client(Endpoint, AccessKeyId, AccessKeySecret, conf, materials, cryptoConf);

    /* Initialize the context for encryption in multipart upload. */
    /* The part size must be an integral multiple of 16 bytes. */
    int64_t partSize = 100 * 1024;
    auto fileSize = getFileSize(fileToUpload);
    MultipartUploadCryptoContext cryptoCtx;
    cryptoCtx.setPartSize(partSize);
    cryptoCtx.setDataSize(fileSize);

    /* Initiate the multipart upload task. */
    InitiateMultipartUploadRequest initUploadRequest(BucketName, ObjectName);
    auto uploadIdResult = client.InitiateMultipartUpload(initUploadRequest, cryptoCtx);
    auto uploadId = uploadIdResult.result().UploadId();
    PartList partETagList;
    int partCount = static_cast<int> (fileSize / partSize);
    /* Calculate the number of parts to upload.*/
    if (fileSize % partSize ! = 0)  {
        partCount++;
    }
    /* Upload each part sequentially. */
    for (int i = 1; i <= partCount; i++) {
        auto skipBytes = partSize * (i - 1);
        auto size = (partSize < fileSize - skipBytes) ? partSize : (fileSize - skipBytes);
        std::shared_ptr<std::iostream> content = std::make_shared<std::fstream>(fileToUpload, std::ios::in|std::ios::binary);
        content->seekg(skipBytes, std::ios::beg);
        UploadPartRequest uploadPartRequest(BucketName, ObjectName, content);
        uploadPartRequest.setContentLength(size);
        uploadPartRequest.setUploadId(uploadId);
        uploadPartRequest.setPartNumber(i);
        auto uploadPartOutcome = client.UploadPart(uploadPartRequest, cryptoCtx);
        if (uploadPartOutcome.isSuccess()) {
            Part part(i, uploadPartOutcome.result().ETag());
            partETagList.push_back(part);
        }
        else {
            std::cout << "uploadPart fail" <<
            ",code:" << uploadPartOutcome.error().Code() <<
            ",message:" << uploadPartOutcome.error().Message() <<
            ",requestId:" << uploadPartOutcome.error().RequestId() << std::endl;
        }
    }
    /* Complete the multipart upload. */
    CompleteMultipartUploadRequest request(BucketName, ObjectName);
    request.setUploadId(uploadId);
    request.setPartList(partETagList);
    auto outcome = client.CompleteMultipartUpload(request, cryptoCtx);
    if (! outcome.isSuccess()) {
        /*Handle exceptions.*/
        std::cout << "CompleteMultipartUpload fail" <<
        ",code:" << outcome.error().Code() <<
        ",message:" << outcome.error().Message() <<
        ",requestId:" << outcome.error().RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }
    /* Release network resources.*/
    ShutdownSdk();
    return 0;
}
```

## Decrypt objects to download to local files

The following code provides an example on how to use an RSA-based CMK managed by yourself to decrypt an object to download to a local file:

```
#include <alibabacloud/oss/OssEncryptionClient.h>
#include <fstream>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";
    std::string ObjectName = "yourObjectName";
    std::string FileNametoSave = "yourFileName";

    /* Specify the CMK and the description. */
    std::string RSAPublicKey = "your rsa public key";
    std::string RSAPrivateKey = "your rsa private key";
    std::map<std::string, std::string> desc;
    desc["comment"] = "your comment";

    /*Initialize network resources.*/
    InitializeSdk();
    ClientConfiguration conf;
    CryptoConfiguration cryptoConf;
    auto materials = std::make_shared<SimpleRSAEncryptionMaterials>(RSAPublicKey, RSAPrivateKey, desc);

    /* To decrypt content that is encrypted by using other CMKs, you must provide the information about the CMKs. */
    //std::string RSAPublicKey2 =  "your rsa public key";
    //std::string RSAPrivateKey2 = "your rsa private key";
    //std::map<std::string, std::string> desc2;
    //desc2["comment"] = "your comment";
    //materials.addEncryptionMaterial(RSAPublicKey2, RSAPrivateKey2, desc2);

    OssEncryptionClient client(Endpoint, AccessKeyId, AccessKeySecret, conf, materials, cryptoConf);

    /* Download the object to a local file. */
    GetObjectRequest request(BucketName, ObjectName);
    request.setResponseStreamFactory([=]() {return std::make_shared<std::fstream>(FileNametoSave, std::ios_base::out | std::ios_base::in | std::ios_base::trunc| std::ios_base::binary); });
    auto outcome = client.GetObject(request);
    if (outcome.isSuccess()) {    
        std::cout << "GetObjectToFile success" << outcome.result().Metadata().ContentLength() << std::endl;
    }
    else {
        /* Handle exceptions.*/
        std::cout << "GetObjectToFile fail" <<
        ",code:" << outcome.error().Code() <<
        ",message:" << outcome.error().Message() <<
        ",requestId:" << outcome.error().RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }
    /* Release network resources.*/
    ShutdownSdk();
    return 0;
}
```

## Decrypt objects to download to local memory

The following code provides an example on how to use an RSA-based CMK managed by yourself to decrypt an object to download to local memory:

```
#include <alibabacloud/oss/OssEncryptionClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";
    std::string ObjectName = "yourObjectName";
    std::string RSAPublicKey = "your rsa public key";
    std::string RSAPrivateKey = "your rsa private key";

    /* Specify the CMK and the description. */
    std::string RSAPublicKey = "your rsa public key";
    std::string RSAPrivateKey = "your rsa private key";
    std::map<std::string, std::string> desc;
    desc["comment"] = "your comment";

    /* Initialize network resources.*/
    InitializeSdk();
    ClientConfiguration conf;
    CryptoConfiguration cryptoConf;
    auto materials = std::make_shared<SimpleRSAEncryptionMaterials>(RSAPublicKey, RSAPrivateKey, desc);

    /* To decrypt content that is encrypted by using different CMKs, you must provide the information about the CMKs. */
    //std::string RSAPublicKey2 =  "your rsa public key";
    //std::string RSAPrivateKey2 = "your rsa private key";
    //std::map<std::string, std::string> desc2;
    //desc2["comment"] = "your comment";
    //materials.addEncryptionMaterial(RSAPublicKey2, RSAPrivateKey2, desc2);

    OssEncryptionClient client(Endpoint, AccessKeyId, AccessKeySecret, conf, materials, cryptoConf);

    /* Download the object to local memory. */
    GetObjectRequest request(BucketName, ObjectName);
    auto outcome = client.GetObject(request);
    if (outcome.isSuccess()) {    
      std::cout << "getObjectToBuffer" << " success, Content-Length:" << outcome.result().Metadata().ContentLength() << std::endl;
        /* Display the downloaded content. */
        std::string content;
        *(outcome.result().Content()) >> content;
        std::cout << "getObjectToBuffer" << "content:" << content << std::endl; 
    }
    else {
        /*Handle exceptions.*/
        std::cout << "getObjectToBuffer fail" <<
        ",code:" << outcome.error().Code() <<
        ",message:" << outcome.error().Message() <<
        ",requestId:" << outcome.error().RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

## Decrypt objects to download by range

The following code provides an example on how to use an RSA-based CMK managed by yourself to decrypt an object to download by range:

```
#include <alibabacloud/oss/OssEncryptionClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /*Initialize the OSS account information.*/
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";
    std::string ObjectName = "yourObectName ";

    /* Specify the CMK and the description. */
    std::string RSAPublicKey = "your rsa public key";
    std::string RSAPrivateKey = "your rsa private key";
    std::map<std::string, std::string> desc;
    desc["comment"] = "your comment";

    /*Initialize network resources.*/
    InitializeSdk();
    ClientConfiguration conf;
    CryptoConfiguration cryptoConf;
    auto materials = std::make_shared<SimpleRSAEncryptionMaterials>(RSAPublicKey, RSAPrivateKey, desc);

    /* To decrypt content that is encrypted by using different CMKs, you must provide the information about the CMKs. */
    //std::string RSAPublicKey2 =  "your rsa public key";
    //std::string RSAPrivateKey2 = "your rsa private key";
    //std::map<std::string, std::string> desc2;
    //desc2["comment"] = "your comment";
    //materials.addEncryptionMaterial(RSAPublicKey2, RSAPrivateKey2, desc2);

    OssEncryptionClient client(Endpoint, AccessKeyId, AccessKeySecret, conf, materials, cryptoConf);

    /* Set the download range. */
    GetObjectRequest request(BucketName,  ObjectName);
    request.setRange(0, 1);
    auto outcome = client.GetObject(request);
    if (! outcome.isSuccess ()) {    
        /* Handle exceptions.*/
        std::cout << "getObject fail" <<
        ",code:" << outcome.error().Code() <<
        ",message:" << outcome.error().Message() <<
        ",requestId:" << outcome.error().RequestId() << std::endl;
        ShutdownSdk();
         return -1;  
    }

    /* Release network resources.*/
    ShutdownSdk();
    return 0;
}
```

## Decrypt objects to download by using resumable download

The following code provides an example on how to use an RSA-based CMK managed by yourself to decrypt an object to download by using resumable download:

```
#include <alibabacloud/oss/OssEncryptionClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /*Initialize the OSS account information.*/
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";
    std::string ObjectName = "yourObjectName";
    std::string DownloadFilePath = "yourDownloadFilePath";
    std::string CheckpointFilePath = "yourCheckpointFilepath";

    /* Specify the CMK and the description. */
    std::string RSAPublicKey = "your rsa public key";
    std::string RSAPrivateKey = "your rsa private key";
    std::map<std::string, std::string> desc;
    desc["comment"] = "your comment";

    /*Initialize network resources.*/
    InitializeSdk();
    ClientConfiguration conf;
    CryptoConfiguration cryptoConf;
    auto materials = std::make_shared<SimpleRSAEncryptionMaterials>(RSAPublicKey, RSAPrivateKey, desc);

    /* To decrypt content that is encrypted by using different CMKs, you must provide the information about the CMKs. */
    //std::string RSAPublicKey2 =  "your rsa public key";
    //std::string RSAPrivateKey2 = "your rsa private key";
    //std::map<std::string, std::string> desc2;
    //desc2["comment"] = "your comment";
    //materials.addEncryptionMaterial(RSAPublicKey2, RSAPrivateKey2, desc2);

    OssEncryptionClient client(Endpoint, AccessKeyId, AccessKeySecret, conf, materials, cryptoConf);

    /* Start resumable download. */
    DownloadObjectRequest request(BucketName, ObjectName, DownloadFilePath, CheckpointFilePath);
    auto outcome = client.ResumableDownloadObject(request);

    if (! outcome.isSuccess()) {
        /* Handle exceptions.*/
        std::cout << "ResumableDownloadObject fail" <<
        ",code:" << outcome.error().Code() <<
        ",message:" << outcome.error().Message() <<
        ",requestId:" << outcome.error().RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }
    /* Release network resources.*/
    ShutdownSdk();
    return 0;
}
```

