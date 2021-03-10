# mb（创建存储空间）

存储空间（Bucket）是用于存储对象（Object）的容器。在上传任意类型的Object前，您需要先创建Bucket。本文介绍如何通过mb命令创建Bucket。

## 注意事项

本文各命令行示例均基于Linux 64位系统，其他系统请将命令开头的./ossutil64替换成对应的Binary名称。例如对于Windows 64位系统，请将./ossutil64替换成ossutil64.exe。各系统对应的Binary名称如下。

|系统|Binary名称|
|--|--------|
|Linux 64位|./ossutil64|
|Linux 32位|./ossutil32|
|Windows 64位|ossutil64.exe|
|Windows 32位|ossutil32.exe|
|macOS 64位|./ossutilmac64|
|macOS 32位|./ossutilmac32|
|ARM 64位|./ossutilarm64|
|ARM 32位|./ossutilarm32|

## 命令格式

```
./ossutil64 mb oss://bucket\_name [--acl <value>][--storage-class <value>][--redundancy-type <value>]
```

参数说明如下：

|参数|说明|
|--|--|
|bucket\_name|创建的Bucket名称。Bucket名称在OSS范围内必须全局唯一，一旦创建完成则无法修改。|
|--acl|Bucket的读写权限ACL。取值如下：-   private（默认值）：只有该Bucket的拥有者可以对该Bucket内的文件进行读写操作，其他人无法访问该Bucket内的文件。
-   public-read：只有Bucket拥有者可以对该Bucket内的文件进行写操作，其他用户（包括匿名访问者）都可以对该Bucket中的文件进行读操作。这有可能造成您数据的外泄以及费用激增，若被人恶意写入违法信息还可能会侵害您的合法权益。除特殊场景外，不建议您配置公共读写权限。
-   public-read-write：任何人（包括匿名访问者）都可以对该Bucket内文件进行读写操作。这有可能造成您数据的外泄以及费用激增，请谨慎操作。 |
|--storage-class|Bucket的存储类型。取值如下：-   Standard（默认值）：支持频繁的数据访问。
-   IA：适用于较低访问频率（平均每月访问频率1到2次）的业务场景，有最低存储时间（30天）和最小计量单位（64 KB）要求。支持数据实时访问，访问数据时会产生数据取回费用。
-   Archive：适用于数据长期保存的业务场景，有最低存储时间（60天）和最小计量单位（64 KB）要求。数据需解冻（约1分钟）后访问，解冻会产生数据取回费用。
-   ColdArchive：适用于需要超长时间存放的极冷数据，有最低存储时间（180天）和最小计量单位（64 KB）要求。数据需解冻后访问，解冻时间根据数据大小和选择的解冻模式决定，解冻会产生数据取回费用。

有关存储类型的更多信息，请参见[存储类型介绍](/intl.zh-CN/开发指南/存储类型/存储类型介绍.md)。 |
|--redundancy-type|Bucket的数据容灾类型。取值如下：-   LRS（默认值）：本地冗余LRS将您的数据冗余存储在同一个可用区的不同存储设备上，可支持两个存储设备并发损坏时，仍维持数据不丢失，可正常访问。
-   ZRS：同城冗余ZRS采用多可用区（AZ）机制，将您的数据冗余存储在同一地域（Region）的3个可用区。可支持单个可用区（机房）整体故障时（如断电、火灾等），仍然能够保障数据的正常访问。

**说明：** 仅华南1（深圳）、华北2（北京）、华东1（杭州）、华东2（上海）、中国（香港）、新加坡地域支持开启同城冗余存储。 |

## 使用示例

-   仅创建名为examplebucket的存储空间。

    ```
    ./ossutil64 mb oss://examplebucket
    ```

    如果创建Bucket时未指定Bucket所在地域，则默认在ossutil配置文件中Endpoint指向的地域创建Bucket。例如配置文件中的Endpoint为`https://oss-cn-hangzhou.aliyuncs.com`，则表示在华东1（杭州）地域创建了Bucket。

-   创建名为examplebucket的存储空间，并指定读写权限ACL为私有、存储类型为低频访问以及数据容灾类型为同城冗余ZRS。

    ```
    ./ossutil64 mb oss://examplebucket --acl private --storage-class IA --redundancy-type ZRS
    ```

-   以下输出结果表明已成功创建符合指定条件的存储空间。

    ```
    0.335189(s) elapsed
    ```


## 通用选项

当您需要通过命令行工具ossutil管理不同地域的Bucket时，可以通过-e选项切换至指定Bucket所属的Endpoint。当您需要通过命令行工具ossutil管理多个阿里云账号下的Bucket时，可以通过-i选项切换至指定账号的AccessKey ID，并通过-k选项切换至指定账号的AccessKey Secret。

例如您需要为另一个阿里云账号下，华东2（上海）地域创建名为examplebucket的存储空间，命令如下：

```
./ossutil64 mb oss://examplebucket -e oss-cn-shanghai.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

有关此命令的其他通用选项的更多信息，请参见[通用选项](/intl.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

