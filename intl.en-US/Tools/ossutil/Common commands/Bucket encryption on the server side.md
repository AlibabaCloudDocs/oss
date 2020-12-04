# Bucket encryption on the server side

The bucket-encryption command is used to add, modify, query, or delete encryption configurations for a bucket.

**Note:**

-   The commands described in this topic apply to Linux. To use the commands in other systems, replace ./ossutil in the command with the actual executable program name. For example, you can use the help command in 32-bit Windows systems by running ossutil32.exe help.
-   For more information about bucket encryption, see [Server-side encryption](/intl.en-US/Developer Guide/Data security/Data encryption/Server-side encryption.md).

## Add or modify bucket encryption configurations

You can run the bucket-encryption command that contains the --method put option to add or modify bucket encryption configurations.

-   Command syntax

    ```
    ./ossutil bucket-encryption --method put oss://bucket --sse-algorithm algorithmName [--kms-masterkey-id  keyid] 
    ```

    --sse-algorithm: specifies the encryption method for the bucket. Valid values:

    -   KMS: sets the encryption method to SSE-KMS.

        You can add --kms-masterkey-id and set the correct CMK ID. OSS uses the specified KMS CMK to encrypt data. Otherwise, OSS uses the default KMS CMK to encrypt data.

    -   AES256: sets the encryption method to SSE-OSS and the encryption algorithm to AES256.
    For more information about the encryption method, see [Server-side encryption](/intl.en-US/Developer Guide/Data security/Data encryption/Server-side encryption.md).

-   Examples
    -   Set the default encryption method of examplebucket to SSE-OSS and the encryption algorithm to AES256. Command:

        ```
        ./ossutil bucket-encryption --method put oss://examplebucket --sse-algorithm AES256
        ```

    -   Set the default encryption method of examplebucket to SSE-KMS, CMK ID, and encryption algorithm to AES256. Command:

        ```
        ./ossutil bucket-encryption --method put oss://examplebucket --sse-algorithm KMS --kms-masterkey-id 9468da86-3509-4f8d-a61e-6eab1eac****
        ```


## Query the server-side encryption configurations of a bucket

You can run the bucket-encryption command that contains the --methodget option to query bucket encryption configurations.

-   Command syntax

    ```
    ./ossutil bucket-encryption --method get oss://bucket
    ```

-   Examples

    Obtain the encryption configuration of examplebucket. Command:

    ```
    ./ossutil bucket-encryption --method get oss://examplebucket
    ```

    Command output:

    ```
    SSEAlgorithm:KMS
    KMSMasterKeyID:
    KMSDataEncryption:
    ```

    The default encryption method of examplebucket is SSE-KMS. The encryption algorithm is AES256. No CMK ID is specified.


## Delete the server-side encryption configurations of a bucket

You can run the bucket-encryption command that contains the --methoddelete option to delete bucket encryption configurations.

-   Command syntax

    ```
    ./ossutil bucket-encryption --method delete oss://bucket
    ```

-   Examples

    Delete the encryption configuration of examplebucket. Command:

    ```
    ./ossutil bucket-encryption --method delete oss://examplebucket
    ```


## Common options

The following table describes the options you can add to the bucket-encryption command.

|Option|Description|
|------|-----------|
|--sse-algorithm|The server-side encryption method for a bucket. Valid values:-   KMS: sets the encryption method to SSE-KMS.
-   AES256: sets the encryption method to SSE-OSS and the encryption algorithm to AES256. |
|--kms-masterkey-id|You can choose whether to add this option only when the value of the --sse-algorithm parameter is `KMS`. If you add this option and set the CMK ID, OSS uses the specified KMS CMK to encrypt data. Otherwise, OSS uses the default KMS CMK to encrypt data.|
|--method|Specifies the HTTP request method. Valid values: -   put: adds or modifies bucket encryption configurations.
-   get: queries bucket encryption configurations.
-   delete: deletes bucket encryption configurations. |
|--loglevel|Specifies the log level. This parameter is empty by default. This indicates that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain corresponding HTTP request and response information. |
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 proxies are supported. Examples: `http://120.79. **.**:3128` and `socks5://120.79. **. **:1080`.|
|--proxy-user|Specifies the username of the proxy server. This parameter is empty by default.|
|--proxy-pwd|Specifies the password for the proxy server. This parameter is empty by default.|

**Note:** For more information about common options, see [View options](/intl.en-US/Tools/ossutil/View options.md).

