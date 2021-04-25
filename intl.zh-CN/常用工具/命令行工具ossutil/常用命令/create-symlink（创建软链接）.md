# create-symlink（创建软链接）

软链接功能用于快速访问存储空间（Bucket）内的常用文件（Object）。通过create-symlink创建软链接后，您可以通过软链接文件快速打开源文件，类似于Windows的快捷方式。

**说明：** 本文各命令行示例均基于Linux 64位系统，其他系统请将命令开头的./ossutil64替换成对应的Binary名称。详情请参见[命令行工具ossutil快速入门](/intl.zh-CN/快速入门/命令行工具ossutil快速入门.md)。

## 命令格式

```
./ossutil64 create-symlink cloud\_url target\_object [--encoding-type <value>] [--payer <value>]
```

参数及选项说明如下：

|配置项|说明|
|---|--|
|cloud\_url|软链接文件所在Bucket的完整路径。|
|target\_object|软链接文件指向的目标Object所在Bucket的完整路径。软链接文件与目标Object必须属于同一个Bucket。|
|--encoding-type|对`cloud_url`以及`target_object`中包含的文件名称进行编码，取值为url。如果不指定该选项，则表示文件名称未经过编码。|
|--payer|请求的支付方式。如果希望访问指定路径下的资源产生的流量、请求次数等费用由请求者支付，请将此选项的值设置为requester。|

## 使用示例

使用此命令创建软链接时不会检查目标文件是否存在。如果目标文件存在，通过创建的软链接文件可直接访问目标文件。如果目标文件不存在，通过创建的软链接文件无法找到目标文件。当您无法判断目标文件是否存在时，请通过[ls（列举）](/intl.zh-CN/常用工具/命令行工具ossutil/常用命令/ls（列举）.md)命令获取目标Bucket内包含的所有文件。

为已存在的目标文件创建软链接文件示例如下：

**说明：** 如果新添加的软链接文件名称与已有的软链接文件重名，则新添加的软链接文件将覆盖已有的软链接文件。

-   为目标存储空间examplebucket根目录下的exampleobject.jpg文件创建名为test.jpg的软链接文件，并将软链接文件保存至该Bucket的根目录。

    ```
    ./ossutil64 create-symlink  oss://examplebucket/test.jpg  oss://examplebucket/exampleobject.jpg
    ```

-   在开启请求者付费模式下，为目标存储空间examplebucket根目录下的test.jpg文件创建名为example.jpg的软链接文件，并将软链接文件保存至该Bucket下的destfolder目录。

    ```
    ./ossutil64 create-symlink  oss://examplebucket/destfolder/example.jpg  oss://examplebucket/test.jpg --payer requester
    ```

-   以下输出结果表明已为目标文件成功创建软链接文件。

    ```
    0.106744(s) elapsed
    ```

    软链接文件创建成功后，您可以通过[read-symlink（读取软链接）](/intl.zh-CN/常用工具/命令行工具ossutil/常用命令/read-symlink.md)或[stat](/intl.zh-CN/常用工具/命令行工具ossutil/常用命令/stat.md)命令获取软链接文件相关信息，例如ETag值、文件最后更新时间等。


## 通用选项

当您需要通过命令行工具ossutil管理不同地域的Bucket时，可以通过-e选项切换至指定Bucket所属的Endpoint。当您需要通过命令行工具ossutil管理多个阿里云账号下的Bucket时，可以通过-i选项切换至指定账号的AccessKey ID，并通过-k选项切换至指定账号的AccessKey Secret。

例如您需要为另一个阿里云账号下，华东2（上海）地域下目标存储空间testbucket下的exampleobject.png文件创建名为testobject.png的软链接，命令如下：

```
./ossutil64 create-symlink  oss://testbucket/testobject.png  oss://testbucket/exampleobject.png -e oss-cn-shanghai.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

