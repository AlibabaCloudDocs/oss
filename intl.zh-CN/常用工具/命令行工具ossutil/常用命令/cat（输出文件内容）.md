# cat（输出文件内容）

cat命令仅支持将存储空间（Bucket）内文件（Object）的内容输出到屏幕。

**说明：** 本文各命令行示例均基于Linux 64位系统，其他系统请将命令开头的./ossutil64替换成对应的Binary名称。详情请参见[命令行工具ossutil快速入门](/intl.zh-CN/快速入门/命令行工具ossutil快速入门.md)。

## 命令格式

```
./ossutil64 cat oss://bucketname/objectname [--payer <value>] [--version-id <value>]
```

**说明：** 仅建议使用此命令查看文本文件内容。

参数及选项说明如下：

|配置项|说明|
|---|--|
|bucketname|Bucket名称。|
|objectname|Object名称。|
|--payer|请求的支付方式。如果希望访问指定路径下的资源产生的流量、请求次数等费用由请求者支付，请将此选项的值设置为requester。|
|--version-id|Object的指定版本。仅适用于已开启或暂停版本控制状态Bucket下的Object。|

## 使用示例

-   将未开启版本控制的目标存储空间examplebucket内名为test.txt的文件内容输出到屏幕

    ```
    ./ossutil64 cat oss://examplebucket/test.txt
    ```

    以下输出结果表明test.txt文件包含的信息，以及完成输出操作所用时长。

    ```
    My Website Home Page.
    
    0.088092(s) elapsed
    ```

-   将已开启版本控制的目标存储空间examplebucket内名为exampleobject.txt文件的指定版本内容输出到屏幕

    ```
    ./ossutil64 cat oss://examplebucket/exampleobject.txt --version-id  CAEQARiBgID8rumR2hYiIGUyOTAyZGY2MzU5MjQ5ZjlhYzQzZjNlYTAyZDE3****
    ```

    有关获取Object所有版本的具体操作，请参见[ls命令](/intl.zh-CN/常用工具/命令行工具ossutil/常用命令/ls（列举账号级别下的资源）.md)。

    以下输出结果表明指定版本的exampleobject.txt文件包含的信息，以及完成输出操作所用时长。

    ```
    Hello World.
    
    0.044820(s) elapsed
    ```


## 通用选项

当您需要通过命令行工具ossutil管理不同地域的Bucket时，可以通过-e选项切换至指定Bucket所属的Endpoint。当您需要通过命令行工具ossutil管理多个阿里云账号下的Bucket时，可以通过-i选项切换至指定账号的AccessKey ID，并通过-k选项切换至指定账号的AccessKey Secret。

例如您需要查看另一个阿里云账号下，华东2（上海）地域下目标存储空间examplebucket1下名为exampleobject1.txt文件内容，命令如下：

```
./ossutil64 cat oss://examplebucket1/exampleobject1.txt -e oss-cn-shanghai.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

有关此命令其他通用选项的更多信息，请参见[通用选项](/intl.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

