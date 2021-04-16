# set-acl（设置或修改ACL）

ACL是授予存储空间（Bucket）和文件（Object）访问权限的访问策略。您可以在创建Bucket或上传Object时设置ACL，也可以在创建Bucket或上传Object后的任意时间内修改ACL。set-acl命令用于设置或修改Bucket或Object的访问权限ACL。

**说明：** 本文各命令行示例均基于Linux 64位系统，其他系统请将命令开头的./ossutil64替换成对应的Binary名称。详情请参见[命令行工具ossutil快速入门](/intl.zh-CN/快速入门/命令行工具ossutil快速入门.md)。

## 设置或修改Bucket ACL

-   命令格式

    ```
    ./ossutil64 set-acl oss://bucket\_name acl -b [--retry-times <value>]
    ```

    参数及选项说明如下：

    |配置项|说明|
    |---|--|
    |bucket\_name|待设置或修改ACL的Bucket名称。|
    |acl|Bucket的读写权限ACL。取值如下：    -   private（默认值）：只有该Bucket的拥有者可以对该Bucket内的文件进行读写操作，其他人无法访问该Bucket内的文件。
    -   public-read：只有Bucket拥有者可以对该Bucket内的文件进行写操作，其他用户（包括匿名访问者）都可以对该Bucket中的文件进行读操作。这有可能造成您数据的外泄以及费用激增，若被人恶意写入违法信息还可能会侵害您的合法权益。除特殊场景外，不建议您配置公共读写权限。
    -   public-read-write：任何人（包括匿名访问者）都可以对该Bucket内文件进行读写操作。这有可能造成您数据的外泄以及费用激增，请谨慎操作。 |
    |-b|不添加此选项，默认对Object设置ACL。如果需要对Bucket设置ACL时，须添加此选项。|
    |--retry-times|发生错误后的重试次数。默认值为10，取值范围为1~500。|

-   使用示例

    设置目标存储空间examplebucket的ACL为private。

    ```
    ./ossutil64 set-acl oss://examplebucket private -b   
    ```


## 设置或修改Object ACL

-   命令格式

    ```
    ./ossutil64 set-acl oss://bucket\_name[/prefix]acl 
    [-r]
    [--include <value>] 
    [--exclude <value>]
    [--version-id <value>]
    [--job <value>] 
    [--retry-times <value>]
    [--encoding-type <value>]
    ```

    参数及选项说明如下：

    |配置项|说明|
    |---|--|
    |bucket\_name|Bucket名称。|
    |prefix|Bucket下的资源、例如目录、文件等。|
    |acl|Object的读写权限ACL。取值如下：    -   default：继承Bucket的读写权限。
    -   private（默认值）：只有该Bucket的拥有者可以对该Bucket内的文件进行读写操作，其他人无法访问该Bucket内的文件。
    -   public-read：只有Bucket拥有者可以对该Bucket内的文件进行写操作，其他用户（包括匿名访问者）都可以对该Bucket中的文件进行读操作。这有可能造成您数据的外泄以及费用激增，若被人恶意写入违法信息还可能会侵害您的合法权益。除特殊场景外，不建议您配置公共读写权限。
    -   public-read-write：任何人（包括匿名访问者）都可以对该Bucket内文件进行读写操作。这有可能造成您数据的外泄以及费用激增，请谨慎操作。 |
    |-r|如果指定该选项时，ossutil会对Bucket下所有符合prefix条件的Object设置ACL。如果不指定该选项，则ossutil只对cloud\_url中指定的单个Object设置ACL。|
    |--include|包含符合指定条件的所有Object。|
    |--exclude|不包含任何符合指定条件的Object。|
    |--version-id|Object的指定版本ID。仅适用于已开启或暂停版本控制状态Bucket下的Object。|
    |--job|多文件操作时的并发任务数，默认值为3，取值范围为1~10000。|
    |--retry-times|发生错误后的重试次数。默认值为10，取值范围为1~500。|
    |--encoding-type|对`oss://bucket_name`之后的prefix进行编码，取值为url。如果不指定该选项，则表示prefix未经过编码。|

-   使用示例
    -   将目标存储空间examplebucket下的exampleobject.txt文件的读写权限ACL设置为private。

        ```
        ./ossutil64 set-acl oss://examplebucket/exampleobject.txt private
        ```

    -   将目标存储空间examplebucket下指定版本ID为`CAEQARiBgID8rumR2hYiIGUyOTAyZGY2MzU5MjQ5ZjlhYzQzZjNlYTAyZDE3****`的exampleobject.txt文件的读写权限ACL设置为private。

        ```
        ./ossutil64 set-acl oss://examplebucket/exampleobject.txt private --version-id CAEQARiBgID8rumR2hYiIGUyOTAyZGY2MzU5MjQ5ZjlhYzQzZjNlYTAyZDE3****
        ```

    -   将目标存储空间examplebucket下所有符合指定前缀为test的文件读写权限ACL设置为default。

        ```
        ./ossutil64 set-acl oss://examplebucket/test default -r
        ```

    -   将目标存储空间examplebucket下所有后缀为.jpg的文件读写权限设置为private。

        ```
        ./ossutil64 set-acl oss://examplebucket private --include "*.jpg" -r
        ```

    -   将目标存储空间examplebucket下文件名称包含abc，且后缀不是.png和.txt的文件读写权限设置为default。

        ```
        ./ossutil64 set-acl oss://examplebucket default --include "*abc*" --exclude "*.png" --exclude "*.txt" -r
        ```


## 通用选项

当您需要通过命令行工具ossutil管理不同地域的Bucket时，可以通过-e选项切换至指定Bucket所属的Endpoint。当您需要通过命令行工具ossutil管理多个阿里云账号下的Bucket时，可以通过-i选项切换至指定账号的AccessKey ID，并通过-k选项切换至指定账号的AccessKey Secret。

例如您需要获取另一个阿里云账号下，华东2（上海）地域下名为testbucket的存储空间设置读写权限ACL为private，命令如下：

```
./ossutil64 set-acl oss://testbucket private -b -e oss-cn-shanghai.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

有关此命令的其他通用选项的更多信息，请参见[通用选项](/intl.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

