# bucket-encryption

bucket-encryption命令用于添加、修改、查询、删除存储空间（Bucket）的加密配置。

**说明：**

-   本文命令均以Linux系统为例，实际使用时，请将命令名称改为您实际可执行程序文件的名称。例如Windows 32位系统的帮助命令为ossutil32.exe help。
-   关于Bucket加密功能详情请参见[服务器端加密](/intl.zh-CN/开发指南/数据安全/数据加密/服务器端加密.md)。

## 添加或修改Bucket加密配置

您可以使用bucket-encryption命令携带--method put选项添加或修改Bucket加密配置。

-   命令格式

    ```
    ./ossutil bucket-encryption --method put oss://bucket --sse-algorithm algorithmName [--kms-masterkey-id  keyid] 
    ```

    --sse-algorithm：选择Bucket的加密方式，取值如下。

    -   KMS：设置加密方式为SSE-KMS。

        您可以选择增加--kms-masterkey-id，并设置正确的CMK ID，OSS将使用指定的KMS CMK进行加密；否则，使用默认托管的KMS CMK进行加密。

    -   AES256：设置加密方式为SSE-OSS，且指定加密算法为AES256。
    加密方式详细介绍，请参见[服务器端加密](/intl.zh-CN/开发指南/数据安全/数据加密/服务器端加密.md)。

-   使用示例
    -   将examplebucket的默认加密方式设置为SSE-OSS，并指定加密算法为AES256。命令如下：

        ```
        ./ossutil bucket-encryption --method put oss://examplebucket --sse-algorithm AES256
        ```

    -   将examplebucket的默认加密方式设置为SSE-KMS，指定CMK ID，指定加密算法为AES256。命令如下：

        ```
        ./ossutil bucket-encryption --method put oss://examplebucket --sse-algorithm KMS --kms-masterkey-id 9468da86-3509-4f8d-a61e-6eab1eac****
        ```


## 获取Bucket加密配置

您可以使用bucket-encryption命令携带--method get选项获取Bucket加密配置。

-   命令格式

    ```
    ./ossutil bucket-encryption --method get oss://bucket
    ```

-   使用示例

    获取examplebucket的加密配置。命令如下：

    ```
    ./ossutil bucket-encryption --method get oss://examplebucket
    ```

    输出如下：

    ```
    SSEAlgorithm:KMS
    KMSMasterKeyID:
    KMSDataEncryption:
    ```

    表明examplebucket的默认加密方式为SSE-KMS，加密算法为AES256，未指定CMK ID。


## 删除Bucket加密配置

您可以使用bucket-encryption命令携带--method delete选项删除Bucket加密配置。

-   命令格式

    ```
    ./ossutil bucket-encryption --method delete oss://bucket
    ```

-   使用示例

    删除examplebucket的加密配置。命令如下：

    ```
    ./ossutil bucket-encryption --method delete oss://examplebucket
    ```


## 常用选项

您可以在使用bucket-encryption命令时附加如下选项：

|选项名称|描述|
|----|--|
|--sse-algorithm|设置Bucket的服务端加密方式，取值如下：-   KMS：设置加密方式为SSE-KMS。
-   AES256：设置加密方式为SSE-OSS，且指定加密算法为AES256。 |
|--kms-masterkey-id|仅当--sse-algorithm值为`KMS`时，可选择是否增加此选项。增加此选项，并设置正确的CMK ID，OSS将使用指定的KMS CMK进行加密；否则，将使用默认托管的KMS CMK进行加密。|
|--method|表示HTTP的请求类型。取值如下： -   put：添加或修改配置。
-   get：获取配置。
-   delete：删除配置。 |
|--loglevel|设置日志级别，默认为空，表示不输出日志文件。取值如下： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括HTTP请求和响应信息）。 |
|--proxy-host|网络代理服务器的URL地址，支持HTTP、HTTPS、SOCKS5。例如`http://120.79.**.**:3128`、 `socks5://120.79.**.**:1080`。|
|--proxy-user|网络代理服务器的用户名，默认为空。|
|--proxy-pwd|网络代理服务器的密码，默认为空。|

**说明：** 更多通用选项请参见[查看选项](/intl.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

