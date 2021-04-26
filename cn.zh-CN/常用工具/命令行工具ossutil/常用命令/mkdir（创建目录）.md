# mkdir（创建目录）

当您希望按实际业务场景对上传至存储空间（Bucket）下的文件（Object）进行合理归类时，您需要先创建目录，然后将目标文件存放至指定目录。本文介绍如何使用mkdir命令创建目录。

**说明：** 本文各命令行示例均基于Linux 64位系统，其他系统请将命令开头的./ossutil64替换成对应的Binary名称。详情请参见[命令行工具ossutil快速入门](/cn.zh-CN/快速入门/命令行工具ossutil快速入门.md)。

## 命令格式

```
./ossutil64 mkdir oss://bucketname dirname [--encoding-type <value>]
```

参数及选项说明如下：

|配置项|说明|
|---|--|
|bucketname|目标Bucket名称。|
|dirname|创建的目录名称。目录名称须以正斜线（/）结尾。若未添加正斜线（/），ossutil会在目录末尾自动添加。|
|--encoding-type|对`oss://bucket_name`后面的key（目录名称）进行编码，取值为url。如果不指定该选项，则表示目录名称未经过编码。|

## 使用示例

按如下步骤将目标文件上传至指定目录。

1.  创建目录。
    -   创建单级目录

        ```
        ./ossutil mkdir oss://examplebucket/dir/
        ```

        以下输出结果表明已在目标存储空间examplebucket下成功创建名为dir/的目录。

        ```
        0.385877(s) elapsed
        ```

    -   创建多级目录

        当您需要对文件存放的目录进行更精细的分类时，您需要创建多级目录对文件进行管理。例如您需要在目标存储空间examplebucket中Photo/目录下存放2021年份的快照信息。

        ```
        ./ossutil mkdir oss://examplebucket/Photo/2021/ 
        ```

        如果误删除了2021/目录，且上一级目录Photo/下文件个数为0，则Photo/目录也会被自动移除。

2.  将文件上传至目标目录

    将exampleobject.txt文件上传至存储空间examplebucket下已创建的dir/目录。

    ```
    ./ossutil cp exampleobject.txt oss://examplebucket/dir/
    ```

    以上输出结果表明文件已上传至目标目录。

    ```
    Succeed: Total num: 1, size: 0. OK num: 1(upload 1 files).
    
    average speed 0(byte/s)
    ```


## 通用选项

当您需要通过命令行工具ossutil管理不同地域的Bucket时，可以通过-e选项切换至指定Bucket所属的Endpoint。当您需要通过命令行工具ossutil管理多个阿里云账号下的Bucket时，可以通过-i选项切换至指定账号的AccessKey ID，并通过-k选项切换至指定账号的AccessKey Secret。

例如您需要在另一个阿里云账号下的华东1（杭州）地域名为examplebucket的存储空间下创建目录dir/，命令如下：

```
./ossutil64 mkdir oss://examplebucket/dir/ -e oss-cn-hangzhou.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

有关此命令其他通用选项的更多信息，请参见[通用选项](/cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

