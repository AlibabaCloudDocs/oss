# cat（输出文件内容）

cat命令可以将文件内容输出到ossutil。

**说明：** 本文命令均以Linux系统为例，实际使用时，请将命令名称改为您实际可执行程序文件的名称。例如Windows 32位系统的帮助命令为ossutil32.exe help。

## 命令格式

```
./ossutil cat oss://bucket/object [--payer requester]
```

**说明：** 仅建议使用此命令查看文本文件内容。

## 使用示例

-   查看指定文件内容

    ```
    ./ossutil cat oss://bucket1/test.txt
    language=ch
    endpoint=oss-cn-hangzhou.aliyuncs.com
    
    0.273027(s) elapsed
    ```

-   在已开启版本控制的Bucket内查看指定版本的文件的内容

    ```
    ./ossutil cat oss://bucket1/test.txt --version-id  CAEQARiBgID8rumR2hYiIGUyOTAyZGY2MzU5MjQ5ZjlhYzQzZjNlYTAyZDE3MDRk
    language=ch
    endpoint=oss-cn-hangzhou.aliyuncs.com
    
    0.375125(s) elapsed
    ```

    在使用`--version-id`选项前，需使用[ls --all-versions](/cn.zh-CN/常用工具/命令行工具ossutil/常用命令/ls（列举）.md)命令获取文件的versionid。

    **说明：** `--version-id`选项仅支持在已开启版本控制的Bucket内使用。开启Bucket版本控制命令请参见[bucket-versioning（版本控制）](/cn.zh-CN/常用工具/命令行工具ossutil/常用命令/bucket-versioning（版本控制）.md)。


## 常用选项

您可以在使用cat命令时附加如下选项：

|选项名称|描述|
|----|--|
|--encoding-type|输入或者输出的Object名的编码方式，目前只支持url编码，即指定该选项时，取值为url。如果不指定该选项，则表示Object名未经过编码。Bucket名不支持url编码。|
|--loglevel|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。 |
|--version-id|操作指定版本的Object，仅支持在已开启版本控制的Bucket内使用。|
|--payer|请求的支付方式，如果为请求者付费模式，需将该值设置为requester。|
|--proxy-host|网络代理服务器的url地址，支持http、https、socks5。例如http://120.79.\*\*.\*\*:3128、 socks5://120.79.\*\*.\*\*:1080。|
|--proxy-user|网络代理服务器的用户名，默认为空。|
|--proxy-pwd|网络代理服务器的密码，默认为空。|

**说明：** 更多通用选项请参见[查看选项](/cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

