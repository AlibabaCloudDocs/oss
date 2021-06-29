# listpart（列举分片）

listpart命令用于列举没有完成分片上传的Object的分片信息。

**说明：** 本文命令均以Linux系统为例，实际使用时，请将命令名称改为您实际可执行程序文件的名称。例如Windows 32位系统的帮助命令为ossutil32.exe help。

## 命令格式

```
./ossutil listpart oss://bucket/object uploadid [options]
```

您需要先通过[ls -m](/intl.zh-CN/常用工具/命令行工具ossutil/常用命令/ls（列举）.md)命令查看Bucket的Object和UploadID信息，再用本命令查看分片的详细信息。

## 使用示例

列举单个未完成分片上传的Object的分片信息：

```
./ossutil listpart oss://bucket/object 15754AF7980C4DFB8193F190837520BB
```

## 常用选项

您可以在使用listpart命令时附加如下选项：

|选项名称|描述|
|----|--|
|--encoding-type|输入或者输出的Object名的编码方式，目前只支持url编码，即指定该选项时，取值为url。如果不指定该选项，则表示Object名未经过编码。Bucket名不支持url编码。|
|--proxy-host|网络代理服务器的url地址，支持http、https、socks5。例如http://120.79.\*\*.\*\*:3128、 socks5://120.79.\*\*.\*\*:1080。|
|--proxy-user|网络代理服务器的用户名，默认为空。|
|--proxy-pwd|网络代理服务器的密码，默认为空。|

**说明：** 更多通用选项请参见[查看选项](/intl.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

