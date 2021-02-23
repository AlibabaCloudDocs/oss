# bucket-encryption（服务器端加密）

配置服务器端加密后，OSS对上传的文件进行加密，再将得到的加密文件持久化保存。下载文件时，OSS自动将加密文件解密后返回给用户。本文介绍如何通过 bucket-encryption命令添加、修改、查询和删除Bucket的加密配置。

**说明：**

-   本文涉及命令均基于Linux 64位系统。如果您使用Windows 64位系统，请将如下示例中的./ossutil64替换为ossutil64.exe。
-   有关服务器端加密的更多信息，请参见[服务器端加密](/intl.zh-CN/开发指南/数据安全/数据加密/服务器端加密.md)。

## 添加或修改Bucket加密配置

-   命令格式

    ```
    ./ossutil64 bucket-encryption --method put oss://bucketName  --sse-algorithm algorithmName  [--kms-masterkey-id  keyid] 
    ```

    参数说明如下：

    |参数|说明|
    |--|--|
    |bucketName|配置服务器端加密的目标Bucket。|
    |--sse-algorithm|Bucket的加密方式。取值：

    -   KMS：使用KMS托管密钥进行加解密，即SSE-KMS。
    -   AES256：使用OSS完全托管密钥进行加解密，即SSE-OSS。 |
    |--kms-masterkey-id|当加密方式为SSE-KMS时，OSS使用默认托管的KMS CMK进行加密。若您希望通过指定的KMS CMK进行加密，请通过此选项设置正确的CMK ID。|

-   使用示例
    -   将examplebucket的服务器端加密方式设置为SSE-OSS，并指定加密算法为AES256。

        ```
        ./ossutil64 bucket-encryption --method put oss://examplebucket --sse-algorithm AES256
        ```

    -   将examplebucket的服务器端加密方式设置为SSE-KMS，指定CMK ID，并使用默认加密算法AES256进行加密。

        ```
        ./ossutil64 bucket-encryption --method put oss://examplebucket --sse-algorithm KMS --kms-masterkey-id 9468da86-3509-4f8d-a61e-6eab1eac****
        ```

    -   以下输出结果表明examplebucket已成功配置服务器端加密。

        ```
        0.856895(s) elapsed
        ```


## 获取Bucket加密配置

-   命令格式

    ```
    ./ossutil64 bucket-encryption --method get oss://bucket
    ```

-   使用示例

    获取examplebucket的加密配置。命令如下：

    ```
    ./ossutil64 bucket-encryption --method get oss://examplebucket
    ```

    以下输出结果表明examplebucket配置的服务器端加密方式为SSE-KMS，未指定CMK ID，且加密算法为AES256。

    ```
    SSEAlgorithm:KMS
    KMSMasterKeyID:
    KMSDataEncryption:
    ```


## 删除Bucket加密配置

-   命令格式

    ```
    ./ossutil64 bucket-encryption --method delete oss://bucket
    ```

-   使用示例

    删除examplebucket的加密配置。命令如下：

    ```
    ./ossutil64 bucket-encryption --method delete oss://examplebucket
    ```

    以下输出结果表明已成功删除examplebucket的服务器端加密配置。

    ```
    0.856686(s) elapsed
    ```


## 通用选项

当您需要通过命令行工具ossutil管理不同地域的Bucket时，可以通过-e选项切换至指定Bucket所属的Endpoint。当您需要通过命令行工具ossutil管理多个阿里云账号下的Bucket时，可以通过-i选项切换至指定账号的AccessKey ID，并通过-k选项切换至指定账号的AccessKey Secret。

例如您需要为另一个阿里云账号下，华东1（杭州）名为examplebucket的存储空间配置AES256的加密方式，命令如下：

```
./ossutil64 bucket-encryption --method put oss://examplebucket --sse-algorithm AES256 -e oss-cn-hangzhou.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

有关此命令其他通用选项的更多信息，请参见[通用选项](/intl.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

