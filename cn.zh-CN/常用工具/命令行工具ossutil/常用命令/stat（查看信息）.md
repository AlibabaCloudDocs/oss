# stat（查看信息）

stat命令用于获取指定存储空间（Bucket）或者对象（Object）的描述信息。例如，通过set-meta命令设置的Object元信息 ，可以通过该命令查看。

**说明：**

-   本文命令均以Linux系统为例，实际使用时，请将命令名称改为您实际可执行程序文件的名称。例如Windows 32位系统的帮助命令为ossutil32.exe help。
-   RAM用户使用此命令查看Object元信息时，则RAM用户需要拥有HeadObject、GetObjectAcl、GetBucketInfo的权限。

## 命令格式

```
./ossutil stat oss://bucket[/object] [--encoding-type url] [--payer requester] [-c file]
```

## 使用示例

-   查看Bucket信息

    ```
     ./ossutil stat oss://bucket1
    ```

-   查看指定Object信息

    ```
    ./ossutil stat oss://bucket1/object
    ```

-   查看名称中含特殊字符的Object信息

    ossutil目前只支持以URL编码的方式输出或输入Object的文件名，对于无法输入或识别的特殊字符，可以先进行URL编码之后再输出。例如，想查看名为示例.txt的Object信息：

    ```
    ./ossutil stat oss://bucket1/%E7%A4%BA%E4%BE%8B.txt --encoding-type url
    ```

    **说明：** Bucket名称不支持URL编码。

-   在已开启版本控制的Bucket查看指定版本的Object

    ```
    ./ossutil stat oss://bucket1/test.jpg --version-id  CAEQARiBgID8rumR2hYiIGUyOTAyZGY2MzU5MjQ5ZjlhYzQzZjNlYTAyZDE3MDRk
    ```

    在使用`--version-id`选项前，需使用[ls --all-versions](/cn.zh-CN/常用工具/命令行工具ossutil/常用命令/ls（列举）.md)命令获取文件的versionid。

    **说明：** `--version-id`选项仅支持在已开启版本控制的Bucket内使用。开启Bucket版本控制命令请参见[bucket-versioning（版本控制）](/cn.zh-CN/常用工具/命令行工具ossutil/常用命令/bucket-versioning（版本控制）.md)。


## 常用选项

您可以在使用stat命令时，附加如下选项：

|选项名称|描述|
|----|--|
|--encoding-type|输入或者输出的Object名的编码方式，目前只支持URL编码，即指定该选项时，取值为url。如果不指定该选项，则表示Object名未经过编码。Bucket名不支持URL编码。|
|--loglevel|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。 |
|--retry-times|当错误发生时的重试次数，默认值：10，取值范围：1~500。|
|--version-id|查看指定版本的Object，仅支持在已开启版本控制的Bucket内使用。|
|--payer|请求的支付方式，如果为请求者付费模式，需将该值设置为requester。|
|--proxy-host|网络代理服务器的URL地址，支持http、https、socks5。例如http://120.79.\*\*.\*\*:3128、 socks5://120.79.\*\*.\*\*:1080。|
|--proxy-user|网络代理服务器的用户名，默认为空。|
|--proxy-pwd|网络代理服务器的密码，默认为空。|

**说明：** 更多通用选项请参见[查看选项](/cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

