# read-symlink（读取软链接）

read-symlink命令用于读取软链接文件的描述信息，包括软链接文件的ETag值、最后更新时间等。此操作要求用户对软链接文件有读权限。

**说明：** 本文各命令行示例均基于Linux 64位系统，其他系统请将命令开头的./ossutil64替换成对应的Binary名称。详情请参见[命令行工具ossutil快速入门](/cn.zh-CN/快速入门/命令行工具ossutil快速入门.md)。

## 命令格式

```
./ossutil64 read-symlink oss://bucketname/objectname [--encoding-type <value>] [--payer <value>]
```

参数及选项说明如下：

|配置项|说明|
|---|--|
|bucketname|Bucket名称。|
|objectname|软链接文件名称。|
|--encoding-type|对软链接文件名称进行编码，取值为url。如果不指定该选项，则表示文件名称未经过编码。|
|--payer|请求的支付方式。如果希望访问指定路径下的资源产生的流量、请求次数等费用由请求者支付，请将此选项的值设置为requester。|

## 使用示例

以下示例用于读取目标存储空间examplebucket下软链接文件test.jpg的描述信息。

```
./ossutil64 read-symlink oss://examplebucket/test.jpg
```

以下输出结果表明已成功获取软链接文件test.jpg的ETag值、最后更新时间（Last-Modified）以及指向的目标文件（X-Oss-Symlink-Target）为example.jpg。

```
Etag                    : 938F26218CE422CBEEE0B6543A2B2D
Last-Modified           : 2021-04-21 18:00:13 +0800 CST
X-Oss-Symlink-Target    : example.jpg
0.217317(s) elapsed
```

如果此命令操作的文件类型不是软链接文件，将返回错误`NotSymlink`。

## 通用选项

当您需要通过命令行工具ossutil管理不同地域的Bucket时，可以通过-e选项切换至指定Bucket所属的Endpoint。当您需要通过命令行工具ossutil管理多个阿里云账号下的Bucket时，可以通过-i选项切换至指定账号的AccessKey ID，并通过-k选项切换至指定账号的AccessKey Secret。

例如您需要获取另一个阿里云账号下，华东2（上海）地域下目标存储空间testbucket中名为testobject.png软链接文件的信息，命令如下：

```
./ossutil64 read-symlink oss://testbucket/testobject.png -e oss-cn-shanghai.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

有关此命令的其他通用选项的更多信息，请参见[通用选项](/cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

