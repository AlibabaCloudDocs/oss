# Client-side encryption

If client-side encryption is performed, objects are encrypted on the local client before they are uploaded to OSS. This topic describes how to perform client-side encryption.

## Disclaimer

-   When you use client-side encryption, you must make sure the integrity and validity of the customer master key \(CMK\). If the CMK is used incorrectly or lost due to improper maintenance, you are liable for any losses or consequences caused by decryption failures.
-   When you copy or migrate encrypted data, you must make sure the integrity and validity of the object metadata related to client-side encryption. If the object metadata related to client-side encryption is used incorrectly or lost due to improper maintenance, you are liable for any losses or consequences caused by decryption failures.

## Implementation modes

You can use the following SDKs to implement client-side encryption:

-   [Java SDK](/intl.en-US/SDK Reference/Java/Data encryption/Client-side encryption.md)
-   [Python SDK](/intl.en-US/SDK Reference/Python/Data encryption/Client-side encryption.md)
-   [Go SDK](/intl.en-US/SDK Reference/Go/Data encryption/Client-side encryption.md)
-   [C++ SDK](/intl.en-US/SDK Reference/C++/Data encryption/Client-side encryption.md)

## Background information

In client-side encryption, a random data key is generated for each object to perform symmetric encryption on the object. The client uses a CMK to encrypt the random data key. The encrypted data key is uploaded as a part of the object metadata and stored in the OSS server. When an encrypted object is downloaded, the client uses the CMK to decrypt the random data key and then uses the data key to decrypt the object. The CMK is used only on the client and is not transmitted over the network or stored in the server, which ensures data security.

**Note:**

-   Client-side encryption supports multipart upload for objects larger than 5 GB. When you use multipart upload to upload an object, you must specify the total size of the object and the size of each part. The size of each part except for the last part must be the same and be a multiple of 16 bytes.
-   After you upload objects encrypted on the client, object metadata related to client-side encryption is protected and you cannot call [CopyObject](/intl.en-US/API Reference/Object operations/Basic operations/CopyObject.md) to modify object metadata related to client-side encryption.

You can use CMKs managed in one of the following ways:

-   [Use KMS-managed CMKs](#section_127_x30_wy8)
-   [Use customer-managed CMK](#section_eil_auq_m38)

For the complete sample code, visit [GitHub](https://github.com/aliyun/aliyun-oss-python-sdk/blob/master/examples/object_crypto.py).

## Use KMS-managed CMKs

If you use KMS-managed CMKs for client-side encryption, you need only to specify the CMK ID when you upload objects instead of providing the client with a data key. The following figure shows the encryption process in detail.

![key2](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3595688951/p128520.png)

-   Encrypt and upload an object
    1.  Obtain a data key.

        The client uses a specified CMK ID to request a data key used to encrypt the object from KMS. KMS returns a random data key and an encrypted data key.

    2.  Encrypt the object and upload it to OSS.

        The client uses the returned data key to encrypt the object and uploads the encrypted object and encrypted data key to OSS.

-   Download and decrypt an object
    1.  Download an object.

        The client downloads an encrypted object. The encrypted data key is included in the metadata of the object.

    2.  Decrypt the object.

        The client sends the encrypted data key and the corresponding CMK ID to KMS. KMS uses the CMK sent by the client to decrypt the encrypted data key and returns the decrypted data key to the client.


**Note:**

-   The client obtains a unique data key for each object to upload.
-   To ensure data security, we recommend that you rotate or update the CMK regularly.
-   You must maintain the mapping relationship between the CMKs and the encrypted objects.

## Use customer-managed CMK

To use this method for client-side encryption, you must generate and manage CMKs by yourself. When you implement client-side encryption on an object to upload, you must upload a symmetric or asymmetric CMK to the client. The following figure shows the encryption process in detail.

![key3](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4595688951/p128521.png)

-   Encrypt and upload an object
    1.  You must provide the client with a symmetric or asymmetric CMK.
    2.  The client uses the CMK to generate a one-time-use symmetric data key that is used only to encrypt the current object to upload. The client generates a random and unique data key for each object to upload.
    3.  The client uses the data key to encrypt the object to upload and uses the CMK to encrypt the data key.
    4.  The encrypted data key is included in the metadata of the uploaded object.
-   Download and decrypt an object
    1.  The client downloads an encrypted object. The encrypted data key is included in the metadata of the object.
    2.  By using the materials in the object metadata, the client determines the CMK used to generate the data key and uses this CMK to decrypt the encrypted data key. Then, the client uses the decrypted data key to decrypt the object.

**Note:**

-   CMKs and unencrypted data are not sent to OSS. Therefore, keep your CMKs secure. If a CMK is lost, objects encrypted by using the data keys generated by using this CMK cannot be decrypted.
-   Data keys are randomly generated by the client.

