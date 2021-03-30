# bucket-encryption \(server-side encryption\)

After you configure server-side encryption, OSS encrypts uploaded objects and permanently stores the encrypted objects. When you download objects, OSS decrypts the objects and returns and decrypted objects to you. This topic describes how to use bucket-encryption to add, modify, query, or delete encryption configurations for a bucket.

**Note:** For more information about how server-side encryption works, see [Server-side encryption](/intl.en-US/Developer Guide/Data security/Data encryption/Server-side encryption.md).

## Usage notes

Each sample command line in this topic is based on the 64-bit Linux system. For other systems, replace ./ossutil64 of the command with the corresponding binary name. For example, for the 64-bit Windows system, replace ./ossutil64 with ossutil64.exe. The following table lists the binary names corresponding to each system.

|System|Binary name|
|------|-----------|
|64-bit Linux|./ossutil64|
|32-bit Linux|./ossutil32|
|64-bit Windows|ossutil64.exe|
|32-bit Windows|ossutil32.exe|
|64-bit macOS|./ossutilmac64|
|32-bit macOS|./ossutilmac32|
|64-bit ARM|./ossutilarm64|
|32-bit ARM|./ossutilarm32|

## Add or modify bucket encryption configurations

-   Command syntax

    ```
    ./ossutil64 bucket-encryption --method put oss://bucketName  --sse-algorithm algorithmName  [--kms-masterkey-id  keyid] 
    ```

    The following table describes the parameters you can configure in the command.

    |Parameter|Description|
    |---------|-----------|
    |bucketName|The bucket for which you want to configure server-side encryption.|
    |--sse-algorithm|The encryption method for the bucket. Valid values:

    -   KMS: The keys managed by Key Management Service \(KMS\) are used for encryption and decryption \(SSE-KMS\).
    -   AES256: The keys managed by OSS are used for encryption and decryption \(SSE-OSS\). |
    |--kms-masterkey-id|When the encryption method is set to SSE-KMS, OSS uses the default KMS-managed customer master key \(CMK\) to encrypt objects. To use the specified KMS-managed CMK to encrypt objects, set this parameter to the valid CMK ID.|

-   Examples
    -   Set the default encryption method to SSE-OSS and the encryption algorithm to AES-256 for examplebucket.

        ```
        ./ossutil64 bucket-encryption --method put oss://examplebucket --sse-algorithm AES256
        ```

    -   Set the default encryption method to SSE-KMS for examplebucket. Specify a CMK ID, and set the encryption algorithm to AES-256.

        ```
        ./ossutil64 bucket-encryption --method put oss://examplebucket --sse-algorithm KMS --kms-masterkey-id 9468da86-3509-4f8d-a61e-6eab1eac****
        ```

    -   If a similar output is displayed, server-side encryption is configured for examplebucket:

        ```
        0.856895(s) elapsed
        ```


## Query the server-side encryption configurations of a bucket

-   Command syntax

    ```
    ./ossutil64 bucket-encryption --method get oss://bucket
    ```

-   Examples

    Query the encryption configurations of examplebucket. Command:

    ```
    ./ossutil64 bucket-encryption --method get oss://examplebucket
    ```

    The following output result shows that the server-side encryption method configured for examplebucket is SSE-KMS, the CMK ID is not specified, and the encryption algorithm is AES-256:

    ```
    SSEAlgorithm:KMS
    KMSMasterKeyID:
    KMSDataEncryption:
    ```


## Delete the server-side encryption configurations of a bucket

-   Command syntax

    ```
    ./ossutil64 bucket-encryption --method delete oss://bucket
    ```

-   Examples

    Delete the server-side encryption configurations of examplebucket. Command:

    ```
    ./ossutil64 bucket-encryption --method delete oss://examplebucket
    ```

    The following output result shows that server-side encryption configurations are deleted for examplebucket:

    ```
    0.856686(s) elapsed
    ```


## Common options

To use command-line tool ossutil to manage buckets that reside in different regions, you can use the -e option to switch to the endpoint of the specified bucket. To use command-line tool ossutil to manage buckets that belong to multiple Alibaba Cloud accounts, you can use the -i option to switch to the AccessKey ID of the specified account, and use the -k option to switch to the AccessKey secret of the specified account.

To set the encryption method to AES-256 for a bucket named examplebucket located in the China \(Hangzhou\) region for a different Alibaba Cloud account, run the following command:

```
./ossutil64 bucket-encryption --method put oss://examplebucket --sse-algorithm AES256 -e oss-cn-hangzhou.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

For more information about other common options that apply to the bucket-encryption command, see [Common options](/intl.en-US/Tools/ossutil/View options.md).

