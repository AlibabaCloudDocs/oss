# bucket-versioning（版本控制）

版本控制是针对存储空间（Bucket）级别的数据保护功能。开启版本控制后，针对数据的覆盖和删除操作将会以历史版本的形式保存下来。在对象（Object）被错误覆盖或者误删除后，您可以将Bucket中存储的Object恢复至任意时刻的历史版本。本文介绍通过bucket-versioning命令设置或获取版本控制状态。

**说明：** 本文涉及命令均基于Linux 64位系统。如果您使用Windows 64位系统，请将如下示例中的./ossutil64替换为ossutil64.exe。

## 设置版本控制状态

-   命令格式

    ```
    ./ossutil64 bucket-versioning --method put oss://bucket\_name versioning\_parameter
    ```

    参数说明如下：

    |参数|说明|
    |--|--|
    |bucket\_name|待设置版本控制状态的目标Bucket名称。|
    |versioning\_parameter|为目标Bucket设置版本控制状态。取值如下：    -   enabled：开启版本控制。当Bucket处于开启版本控制状态时，OSS将为新上传的Object生成全局唯一的随机字符串版本ID。有关启用版本控制状态下Object相关操作的更多信息，请参见[开启版本控制](/intl.zh-CN/开发指南/数据安全/版本控制/开启版本控制.md)。
    -   suspended：暂停版本控制。当Bucket处于暂停版本控制状态时，OSS将为新上传的Object生成特殊字符串为“null”的版本ID。有关暂停版本控制状态下Object的相关操作的更多信息，请参见[暂停版本控制](/intl.zh-CN/开发指南/数据安全/版本控制/暂停版本控制.md)。
**说明：** 默认情况下，Bucket版本控制状态为“未开启”。一旦Bucket开启了版本控制，将无法返回至“未开启”状态。但是，您可以暂停Bucket版本控制。 |

-   使用示例

    对目标存储空间examplebucket开启版本控制。

    ```
    ./ossutil64 bucket-versioning --method put oss://examplebucket enabled
    ```

    对目标存储空间examplebucket暂停版本控制。

    ```
    ./ossutil64 bucket-versioning --method put oss://examplebucket suspended
    ```

    以下输出结果表明examplebucket已成功设置版本控制状态。

    ```
    0.261209(s) elapsed
    ```


## 获取版本控制状态

-   命令格式

    ```
    ./ossutil64 bucket-versioning --method get oss://bucket\_name
    ```

-   使用示例

    查询examplebucket的版本控制状态。

    ```
    ./ossutil64 bucket-versioning --method get oss://examplebucket
    ```

    以下输出结果表明examplebucket已开启版本控制。

    ```
    bucket versioning status:Enabled
    
    0.218001(s) elapsed
    ```

    以下输出结果表明examplebucket已暂停版本控制。

    ```
    bucket versioning status:Suspended
    
    0.168791(s) elapsed
    ```

    以下输出结果表明examplebucket未开启版本控制。

    ```
    bucket versioning status:Null
    
    0.158691(s) elapsed
    ```


## 相关操作

-   在开启版本控制的Bucket中上传文件的行为与未开启版本控制状态下的上传行为一致。但Bucket开启版本控制后，OSS会为新上传至Bucket的Object生成全局唯一的versionID。详情请参见[cp（上传文件）](/intl.zh-CN/常用工具/命令行工具ossutil/常用命令/cp/上传文件.md)。
-   开启Bucket的版本控制后，针对数据的覆盖和删除操作将会以历史版本的形式保存下来。您可以通过指定versionID的方式下载指定版本Object，详情请参见[cp（下载文件）](/intl.zh-CN/常用工具/命令行工具ossutil/常用命令/cp/下载文件.md)。您还可以通过指定versionID的方式将Object恢复至任意时刻的历史版本，详情请参见[cp（拷贝文件）](/intl.zh-CN/常用工具/命令行工具ossutil/常用命令/cp/拷贝文件.md)。

## 通用选项

当您需要通过命令行工具ossutil管理不同地域的Bucket时，可以通过-e选项切换至指定Bucket所属的Endpoint。当您需要通过命令行工具ossutil管理多个阿里云账号下的Bucket时，可以通过-i选项切换至指定账号的AccessKey ID，并通过-k选项切换至指定账号的AccessKey Secret。

例如您需要为另一个阿里云账号下，华东1（杭州）名为examplebucket的存储空间开启版本控制，命令如下：

```
./ossutil64 bucket-versioning--method put oss://examplebucket enabled -e oss-cn-hangzhou.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

有关此命令其他通用选项的更多信息，请参见[通用选项](/intl.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

