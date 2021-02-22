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
|x-oss-meta-client-side-encryption-start|The initial value which is generated randomly for data encryption. The initial value is a string encrypted by using the CMK and encoded in Base64.|Yes|
|x-oss-meta-client-side-encryption-cek-alg|The encryption algorithm used to encrypt data.|Yes|
|x-oss-meta-client-side-encryption-wrap-alg|The encryption algorithm used to encrypt data keys.|Yes|
|x-oss-meta-client-side-encryption-matdesc|The description of the CMK in JSON format. **Warning:** We recommend that you configure a description for each CMK and store the mapping relationship between the CMK and its description. CMKs of which the description is not specified cannot be replaced.

|No|
|x-oss-meta-client-side-encryption-unencrypted-content-length|The length of data before encryption. This parameter is not generated if Content-Length is not specified.|No|
|x-oss-meta-client-side-encryption-unencrypted-content-md5|The MD5 hash of the data before encryption. This parameter is not generated if Content-MD5 is not specified.|No|
|x-oss-meta-client-side-encryption-data-size|The total size of the data that needs to be encrypted for multipart upload when init\_multipart is called.|Yes \(for multipart upload\)|
|x-oss-meta-client-side-encryption-part-size|The size of each part that needs to be encrypted for multipart upload when init\_multipart is called. **Note:** The size of each part must be an integral multiple of 16 bytes.

|Yes \(for multipart upload\)|

## Use RSA-based CMKs managed by yourself to encrypt objects to upload by using simple upload or decrypt objects to download by using simple download

The following code provides an example on how to use an RSA-based CMK managed by yourself to encrypt an object to upload by using simple upload or decrypt an object to download by using simple download:

```
package main

import (
  "bytes"
  "fmt"
  "io/ioutil"
  "os"

  "github.com/aliyun/aliyun-oss-go-sdk/oss"
  "github.com/aliyun/aliyun-oss-go-sdk/oss/crypto"
)

func main() {
  // Create an OSSClient instance.
  client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
  if err ! = nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // Specify a description for the CMK. The description cannot be modified after being specified. You can specify only one description for a CMK.
  // If all objects share a CMK, you can leave the description of the CMK empty. In this case, the CMK cannot be replaced.
  // If the description of the CMK is empty, the client cannot determine which CMK to use for decryption.
  // We recommend that you configure a description (a JSON string) for each CMK and store the mapping relationship between the CMK and description on the client. The server does not store the relationship.

    // Convert the CMK description (a JSON string) into a mapping relationship. 
    materialDesc := make(map[string]string)
  materialDesc["desc"] = "<your master encrypt key material describe information>"

  // Create a CMK object based on the CMK description.
  masterRsaCipher, err := osscrypto.CreateMasterRsa(materialDesc, "<your rsa public key>", "<your rsa private key>")
  if err ! = nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // Create an encryption operation based on the CMK object and perform encryption in AES CTR mode.
  contentProvider := osscrypto.CreateAesCtrCipher(masterRsaCipher)

  // Obtain an existing bucket to be used in client-side encryption.
  // The usage of a bucket used in client-side encryption is similar to that of a common bucket.
  cryptoBucket, err := osscrypto.GetCryptoBucket(client, "<yourBucketName>", contentProvider)
  if err ! = nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // The system performs encryption when PutObject is called.
  err = cryptoBucket.PutObject("<yourObjectName>", bytes.NewReader([]byte("yourObjectValueByteArrary")))
  if err ! = nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // The system performs decryption when GetObject is called.
  body, err := cryptoBucket.GetObject("<yourObjectName>")
  if err ! = nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }
  defer body.Close()

  data, err := ioutil.ReadAll(body)
  if err ! = nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }
  fmt.Println("data:", string(data))
}
```

## Use RSA-based CMKs managed by yourself to encrypt objects to upload by using multipart upload

The following code provides an example on how to use an RSA-based CMK managed by yourself to encrypt an object to upload by using multipart upload:

```
package main

import (
  "fmt"
  "os"

  "github.com/aliyun/aliyun-oss-go-sdk/oss"
  "github.com/aliyun/aliyun-oss-go-sdk/oss/crypto"
)

func main() {
  // Create an OssClient instance.
  client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
  if err ! = nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // Specify a description for the CMK. The description cannot be modified after being specified. You can specify only one description for a CMK.
  // If all objects share a CMK, you can leave the description of the CMK empty. In this case, the CMK cannot be replaced.
  // If the description of the CMK is empty, the client cannot determine which CMK to use for decryption.
  // We recommend that you configure a description (a JSON string) for each CMK and store the mapping relationship between the CMK and description on the client. The server does not store the relationship.

    // Convert the CMK description (a JSON string) into a mapping relationship.
  materialDesc := make(map[string]string)
  materialDesc["desc"] = "<your master encrypt key material describe information>"

  // Create a CMK object based on the CMK description.
  masterRsaCipher, err := osscrypto.CreateMasterRsa(materialDesc, "<your rsa public key>", "<your rsa private key>")
  if err ! = nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // Create an encryption operation based on the CMK object and perform encryption in AES CTR mode.
  contentProvider := osscrypto.CreateAesCtrCipher(masterRsaCipher)

  // Obtain an existing bucket to be used in client-side encryption.
  // The usage of a bucket used in client-side encryption is similar to that of a common bucket.
  cryptoBucket, err := osscrypto.GetCryptoBucket(client, "<yourBucketName>", contentProvider)
  if err ! = nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  fileName := "<yourLocalFilePath>"
  fileInfo, err := os.Stat(fileName)
  if err ! = nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }
  fileSize := fileInfo.Size()

  // Encrypt the context.
  var cryptoContext osscrypto.PartCryptoContext
  cryptoContext.DataSize = fileSize

  // Specify the expected number of parts. The actual number is based on the calculation result.
  expectPartCount := int64(10)

  // Specify the size of a part encrypted in AES CTR mode, which is an integral multiple of 16 bytes.
  cryptoContext.PartSize = (fileSize / expectPartCount / 16) * 16

  imur, err := cryptoBucket.InitiateMultipartUpload("<yourObjectName>", &cryptoContext)
  if err ! = nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  chunks, err := oss.SplitFileByPartSize(fileName, cryptoContext.PartSize)
  if err ! = nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  var partsUpload []oss.UploadPart
  for _, chunk := range chunks {
    part, err := cryptoBucket.UploadPartFromFile(imur, fileName, chunk.Offset, chunk.Size, (int)(chunk.Number), cryptoContext)
    if err ! = nil {
      fmt.Println("Error:", err)
      os.Exit(-1)
    }
    partsUpload = append(partsUpload, part)
  }

  // Complete multipart upload.
  _, err = cryptoBucket.CompleteMultipartUpload(imur, partsUpload)
  if err ! = nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }
}
```

## Decrypt objects that are encrypted by using different RSA-based CMKs managed by yourself

The following code provides an example on how to decrypt an object that is encrypted by using a different RSA-based CMK managed by yourself:

```
package main

import (
  "bytes"
  "fmt"
  "io/ioutil"
  "os"

  "github.com/aliyun/aliyun-oss-go-sdk/oss"
  "github.com/aliyun/aliyun-oss-go-sdk/oss/crypto"
)

// Query the CMK based on the CMK description. This API is required if you need to decrypt an object that is encrypted by using a different master key.
type MockRsaManager struct {
}

func (mg *MockRsaManager) GetMasterKey(matDesc map[string]string) ([]string, error) {
  keyList := []string{"<yourRsaPublicKey", "yourRsaPrivatKey"}
  return keyList, nil
}

// Decrypt the object that is encrypted by using a different CMK.
func main() {
  // Create an OSSClient instance.
  client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
  if err ! = nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // Specify a description for the CMK. The description cannot be modified after being specified. You can specify only one description for a CMK.
  // If all objects share a CMK, you can leave the description of the CMK empty. In this case, the CMK cannot be replaced.
  // If the description of the CMK is empty, the client cannot determine which CMK to use for decryption.
  // We recommend that you configure a description (a JSON string) for each CMK and store the mapping relationship between the CMK and description on the client. The server does not store the relationship.

    // Convert the CMK description (a JSON string) into a mapping relationship.
  materialDesc := make(map[string]string)
  materialDesc["desc"] = "<your master encrypt key material describe information>"

  // Create a CMK object based on the CMK description.
  masterRsaCipher, err := osscrypto.CreateMasterRsa(materialDesc, "<your rsa public key>", "<your rsa private key>")
  if err ! = nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // Create an encryption operation based on the CMK object and perform encryption in AES CTR mode.
  contentProvider := osscrypto.CreateAesCtrCipher(masterRsaCipher)

  // To decrypt objects encrypted by using different CMKs, you must provide this operation.
  var mockRsaManager MockRsaManager
  var options []osscrypto.CryptoBucketOption
  options = append(options, osscrypto.SetMasterCipherManager(&mockRsaManager))

  // Obtain an existing bucket to be used in client-side encryption.
  // The usage of a bucket used in client-side encryption is similar to that of a common bucket.
  cryptoBucket, err := osscrypto.GetCryptoBucket(client, "<yourBucketName>", contentProvider, options...)
  if err ! = nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // The system performs encryption when PutObject is called.
  err = cryptoBucket.PutObject("<yourObjectName>", bytes.NewReader([]byte("yourObjectValueByteArrary")))
  if err ! = nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // The system performs decryption when GetObject is called.
  body, err := cryptoBucket.GetObject("<otherObjectNameEncryptedWithOtherRsa>")
  if err ! = nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }
  defer body.Close()

  data, err := ioutil.ReadAll(body)
  if err ! = nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }
  fmt.Println("data:", string(data))
}
```

## Use CMKs stored in KMS to encrypt objects to be uploaded by using simple upload or decrypt objects to be downloaded by using simple download

The following code provides an example on how to use a CMK stored in KMS to encrypt an object to be uploaded by using simple upload or decrypt an object to be downloaded by using simple download:

```
package main

import (
  "bytes"
  "fmt"
  "io/ioutil"
  "os"

  kms "github.com/aliyun/alibaba-cloud-sdk-go/services/kms"
  "github.com/aliyun/aliyun-oss-go-sdk/oss"
  "github.com/aliyun/aliyun-oss-go-sdk/oss/crypto"
)

func main() {
  // Create an OSSClient instance.
  client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
  if err ! = nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // Create a KMSClient instance.
  kmsClient, err := kms.NewClientWithAccessKey("<yourKmsRegion>", "<yourKmsAccessKeyId>", "<yourKmsAccessKeySecret>")
  if err ! = nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // Specify a description for the CMK. The description cannot be modified after being specified. You can specify only one description for a CMK.
  // If all objects share a CMK, you can leave the description of the CMK empty. In this case, the CMK cannot be replaced.
  // If the description of the CMK is empty, the client cannot determine which CMK to use for decryption.
  // We recommend that you configure a description (a JSON string) for each CMK and store the mapping relationship between the CMK and description on the client. The server does not store the relationship.

    // Convert the CMK description (a JSON string) into a mapping relationship.
  materialDesc := make(map[string]string)
  materialDesc["desc"] = "<your kms encrypt key material describe information>"

  // Create a CMK object based on the CMK description.
  masterkmsCipher, err := osscrypto.CreateMasterAliKms(materialDesc, "<YourKmsId>", kmsClient)
  if err ! = nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // Create an encryption operation based on the CMK object and perform encryption in AES CTR mode.
  contentProvider := osscrypto.CreateAesCtrCipher(masterkmsCipher)

  // Obtain an existing bucket to be used in client-side encryption.
  // The usage of a bucket used in client-side encryption is similar to that of a common bucket.
  cryptoBucket, err := osscrypto.GetCryptoBucket(client, "<yourBucketName>", contentProvider)
  if err ! = nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // The system performs encryption when PutObject is called.
  err = cryptoBucket.PutObject("<yourObjectName>", bytes.NewReader([]byte("yourObjectValueByteArrary")))
  if err ! = nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // The system performs decryption when GetObject is called.
  body, err := cryptoBucket.GetObject("<yourObjectName>")
  if err ! = nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }
  defer body.Close()

  data, err := ioutil.ReadAll(body)
  if err ! = nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }
  fmt.Println("data:", string(data))
}
```

