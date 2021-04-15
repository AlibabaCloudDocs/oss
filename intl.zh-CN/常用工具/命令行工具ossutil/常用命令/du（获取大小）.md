# du（获取大小）

du命令用于获取指定存储空间（Bucket）、文件目录下包含的所有Object的大小。

**说明：** 本文各命令行示例均基于Linux 64位系统，其他系统请将命令开头的./ossutil64替换成对应的Binary名称。详情请参见[命令行工具ossutil快速入门](/intl.zh-CN/快速入门/命令行工具ossutil快速入门.md)。

## 命令格式

```
./ossutil64 du oss://bucketname[/prefix] [--payer requester] [--all-versions][--block-size <value>]
```

参数及选项说明如下：

|配置项|说明|
|---|--|
|bucketname|目标Bucket名称。|
|prefix|Bucket下的某个文件目录或指定前缀。|
|--payer|请求的支付方式。如果希望访问指定路径下的资源产生的流量、请求次数等费用由请求者支付，请将此选项的值设置为requester。|
|--all-versions|获取所有版本Object的大小。不添加此选项时，默认查询当前版本Object的大小。|
|--block-size|定义输出结果中指定Bucket或目录下包含的Object大小，取值为KB、MB、GB或TB。不添加此选项时，默认以Byte为单位统计Object的大小。|

## 查询指定Bucket下所有版本Object的大小

以下命令用于查询examplebucket内所有版本Object的大小：

```
./ossutil64 du oss://examplebucket --all-versions
```

以下输出结果表明examplebucket内共有13个Object，其中12个Object的存储类型为Standard（标准存储），1个Object为Archive（归档存储），Object总大小为132116024 字节。

```
storage class   object count            sum size(byte)
----------------------------------------------------------
Standard        12                       132115210
Archive         1                        814
----------------------------------------------------------
total object count: 13                          total object sum size: 132116024
total part count:   0                           total part sum size:   0

total du size(byte):132116024

0.382978(s) elapsed
```

## 查询指定目录下所有当前版本Object的大小

以下命令用于查询examplebucket内指定目录dir下的当前版本Object大小，Object大小以GB为单位进行统计：

```
./ossutil64 du oss://examplebucket/dir/  --block-size GB
```

以下输出结果表明存储空间examplebucket内指定目录dir下共有5个Object，其存储类型均为Standard，文件总大小为0.0002 GB。

```
storage class   object count            sum size(byte)
----------------------------------------------------------
Standard        5                       232277
----------------------------------------------------------
total object count: 5                           total object sum size: 232277
total part count:   0                           total part sum size:   0

total du size(GB):0.0002

0.078757(s) elapsed
```

## 查询与前缀匹配的所有版本Object的大小

以下命令用于查询目标存储空间examplebucket下与前缀test匹配的所有版本Object的大小，Object大小以KB为单位进行统计：

```
./ossutil64 du oss://examplebucket/test --all-versions --block-size KB
```

以下输出结果表明examplebucket下与前缀test匹配的Object共有4个，其存储类型均为Standard，大小为448.1455 KB。

```
storage class   object count            sum size(byte)
----------------------------------------------------------
Standard        4                       439425
----------------------------------------------------------
total object count: 4                           total object sum size: 439425
total part count:   0                           total part sum size:   0

total du size(KB):448.1455

0.126340(s) elapsed
```

## 通用选项

当您需要通过命令行工具ossutil管理不同地域的Bucket时，可以通过-e选项切换至指定Bucket所属的Endpoint。当您需要通过命令行工具ossutil管理多个阿里云账号下的Bucket时，可以通过-i选项切换至指定账号的AccessKey ID，并通过-k选项切换至指定账号的AccessKey Secret。

例如您需要获取另一个阿里云账号下，华东2（上海）地域下名为testbucket的存储空间下所有版本Object的大小，命令如下：

```
./ossutil64 du oss://testbucket --all-versions -e oss-cn-shanghai.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

有关此命令的其他通用选项的更多信息，请参见[通用选项](/intl.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

