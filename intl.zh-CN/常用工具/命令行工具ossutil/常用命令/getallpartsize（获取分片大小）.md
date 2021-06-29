# getallpartsize（获取分片大小）

getallpartsize命令用于获取存储空间（Bucket）内所有未完成上传的Multipart任务的每个分片大小以及分片总大小。

**说明：** 本文命令均以Linux系统为例，实际使用时，请将命令名称改为您实际可执行程序文件的名称。例如Windows 32位系统的帮助命令为ossutil32.exe help。

## 命令格式

```
./ossutil getallpartsize oss://bucket 
```

## 使用示例

列举所有未完成上传的Multipart Object的分片信息以及总大小：

```
./ossutil getallpartsize oss://bucket1
PartNumber      UploadId                                Size(Byte)      Path
1               F18A92392DFD4B3FA897C267829FE417        52428800        oss://bucket1/test/test1.txt
2               F18A92392DFD4B3FA897C267829FE417        52428800        oss://bucket1/test/test1.txt
3               F18A92392DFD4B3FA897C267829FE417        52428800        oss://bucket1/test/test1.txt
4               F18A92392DFD4B3FA897C267829FE417        52428800        oss://bucket1/test/test1.txt
5               F18A92392DFD4B3FA897C267829FE417        52428800        oss://bucket1/test/test1.txt
6               F18A92392DFD4B3FA897C267829FE417        52428800        oss://bucket1/test/test1.txt

total part count:6     total part size(MB):300.00
```

## 常用选项

您可以在使用getallpartsize命令时附加如下选项：

|选项名称|描述|
|----|--|
|--encoding-type|输入或者输出的Object名的编码方式，目前只支持url编码，即指定该选项时，取值为url。如果不指定该选项，则表示Object名未经过编码。Bucket名不支持url编码。|
|--proxy-host|网络代理服务器的url地址，支持http、https、socks5。例如http://120.79.\*\*.\*\*:3128、 socks5://120.79.\*\*.\*\*:1080。|
|--proxy-user|网络代理服务器的用户名，默认为空。|
|--proxy-pwd|网络代理服务器的密码，默认为空。|

**说明：** 更多通用选项请参见[查看选项](/intl.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

