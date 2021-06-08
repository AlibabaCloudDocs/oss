# 同步OSS文件到本地

sync命令用于将OSS中的文件同步到本地。

## 注意事项

-   Binary名称

    本文各命令行示例均基于Linux 64位系统，其他系统请将命令开头的./ossutil64替换成对应的Binary名称。详情请参见[命令行工具ossutil快速入门](/cn.zh-CN/快速入门/命令行工具ossutil快速入门.md)。

-   文件数量

    通过sync命令执行一次同步任务时，最多可同步100万个文件。当同步的文件个数超出100万时，将报错over max sync numbers 1000000.。

-   sync命令与cp命令的区别

    -   sync命令强制以递归的方式遍历指定文件夹内所有文件或子文件夹。cp命令需增加-r选项才会进行递归操作。
    -   通过sync命令将数据同步到OSS时，ossutil支持通过--backup-dir选项指定目标文件夹，用于保存目的端存在而源端不存在的文件。cp命令不支持--backup-dir选项。
    -   sync不支持--version-id选项，无法在已开启版本控制的Bucket内同步历史版本文件。cp命令支持--version-id选项。
    除以上区别外，sync命令与cp命令用法类似。有关cp命令的用法及示例，请参见[cp（下载文件）](/cn.zh-CN/常用工具/命令行工具ossutil/常用命令/cp/下载文件.md)。


## 命令格式

```
./ossutil64 sync cloud\_url  file\_url
[-f --force]
[-u --update]
[--delete]
[--backup-dir <value>]
[--enable-symlink-dir]
[--disable-all-symlink]
[--disable-ignore-error]
[--only-current-dir]
[--output-dir <value>]
[--bigfile-threshold <value>]
[--part-size <value>]
[--checkpoint-dir <value>]
[--range <value>]
[--encoding-type <value>]
[--snapshot-path <value>]
[--include <value>]
[--exclude <value>]
[--disable-crc64]
[--payer <value>]
[-j, --job <value>]
[--parallel <value>]
[--retry-times <value>]
```

参数及选项说明如下：

|配置项|说明|
|---|--|
|cloud\_url|OSS文件夹（目录）路径。格式为`oss://bucketname/path/`。例如`oss://examplebucket/exampledir/`。如果输入的`cloud_url`没有以正斜线（/）结尾，ossutil会自动在结尾处添加一个正斜线（/）。|
|file\_url|待同步的本地文件夹路径。例如Linux系统文件夹路径`/localfolder/`，Windows系统文件夹路径`D:\localfolder\`。|
|-f --force|强制操作，不进行询问提示。|
|-u，--update|只有当目标文件不存在，或源文件的最后修改时间晚于目标文件时，ossutil才会执行同步操作。|
|--delete|将目的端指定路径下的其他文件都删除，仅保留本次同步的文件。**警告：** 建议您使用--delete选项前开启[版本控制](/cn.zh-CN/开发指南/数据安全/版本控制/版本控制介绍.md)，防止数据被误删。 |
|--backup-dir|指定目标文件夹，用于保存目的端存在而源端不存在的文件。|
|--enable-symlink-dir|同步链接子目录。|
|--disable-all-symlink|同步目录时，忽略所有的链接子文件以及链接子目录。|
|--disable-ignore-error|批量操作时不忽略错误。|
|--only-current-dir|仅同步当前目录下的文件，忽略子目录及子目录下的文件。|
|--output-dir|指定输出文件所在的目录。输出文件是指批量同步文件出错时产生的report文件，默认保存在当前目录下的ossutil\_output目录。|
|-bigfile-threshold|设置断点续传文件的大小阈值，单位为字节。默认值：100 MB

取值范围：0~9223372036854775807 |
|--part-size|设置分片大小，单位为字节。默认情况下ossutil会根据文件大小自行计算合适的分片大小值。取值范围：1~9223372036854775807 |
|--checkpoint-dir|指定断点续传记录信息所在的目录。断点续传操作失败时，ossutil会自动创建名为`.ossutil_checkpoint`的目录，并在该目录下记录checkpoint信息，断点续传成功后会删除该目录。如果指定了该选项，请确保指定的目录可以被删除。|
|--range|下载目标文件的指定字段，并保存为一个新的文件，字段从0开始编号。 -   指定区间

例如指定为`3-9`，表示下载文件的第3个字节到第9个字节（包含第3和第9字节）。

-   指定开始位置

例如指定为`3-`，表示从第3个字节开始到文件结尾（包含第3个字节）。

-   指定结束位置

例如指定为`-9`，表示从0字节到第9个字节（包含第9个字节）。 |
|--encoding-type|文件名称的编码方式。取值为url。如果不指定该选项，则表示文件名称未经过编码。|
|--snapshot-path|指定保存同步文件时的快照信息所在的目录。在下一次同步文件时，ossutil会读取指定目录下的快照信息进行增量同步。|
|--include|包含符合指定条件的所有文件。|
|--exclude|不包含任何符合指定条件的文件。|
|--disable-crc64|关闭CRC64数据校验。|
|--payer|请求的支付方式。如果希望访问指定路径下的资源产生的流量、请求次数等费用由请求者支付，请将此选项的值设置为requester。|
|-j，--job|多文件操作时的并发任务数，默认值为3，取值范围为1~10000。|
|--parallel|单文件操作时的并发任务数，取值范围为1-10000。 如果不设置此选项，默认由ossutil根据操作类型和文件大小自行决定。|
|--retry-times|发生错误后的重试次数。默认值为10，取值范围为1~500。|

## 使用示例

OSS存储空间examplebucket下localdir文件夹内有文件a.txt、b.txt以及子文件夹C，本地根目录下destdir文件夹内有d.txt文件。文件结构如下：

```
examplebucket           本地根目录
└── localdir/             └── destdir/
       ├── a.txt                └── d.txt
       ├── b.txt
       └── C/
```

-   将OSS的localdir文件夹同步到本地

    ```
    ./ossutil sync  oss://examplebucket/localdir/  destdir/ 
    ```

    命令执行完毕后，本地destdir文件夹内新增a.txt、b.txt以及子文件夹C。

    ```
    examplebucket           本地根目录
    └── localdir/             └── destdir/
           ├── a.txt                ├── a.txt 
           ├── b.txt                ├── b.txt
           └── C/                   ├── d.txt
                                       └── C/ 
    ```

-   将OSS存储空间examplebucket下的文件夹localdir同步到本地文件夹destdir，并通过--backup-dir选项指定backup文件夹，用于保存本地指定目录下已存在而OSS不存在的文件，同时在原有文件夹下的这些文件会被删除。

    ```
    ./ossutil sync oss://examplebucket/localdir/  destdir/  --delete  --backup-dir backup/
    ```

    命令执行完毕后，OSS的localdir文件夹将同步到本地，同时本地destdir文件夹内存在的而OSS不存在的文件被转移到backup文件夹内。即本地destdir文件夹仅保存a.txt、b.txt及子文件夹C，原有的d.txt文件将保存至backup文件夹，且d.txt文件在原有的destdir文件夹下被删除。

    ```
    examplebucket              本地根目录
    └── localdir/               ├── destdir/
           ├── a.txt            │     ├── a.txt 
           ├── b.txt            │     ├── b.txt
           └── C/               │     └── C/                             
                                   └── backup/
                                          └──d.txt
    ```

-   以上示例同步成功后，返回结果中将包含同步的文件数量、文件大小以及完成同步操作所用时长，示例如下：

    ```
    Succeed: Total num: 2, size: 750,081. OK num: 2(upload 2 files).
    
    average speed 1641000(byte/s)
    ```


## 通用选项

当您需要通过命令行工具ossutil管理不同地域的Bucket时，可以通过-e选项切换至指定Bucket所属的Endpoint。当您需要通过命令行工具ossutil管理多个阿里云账号下的Bucket时，可以通过-i选项切换至指定账号的AccessKey ID，并通过-k选项切换至指定账号的AccessKey Secret。

例如，您需要将另一个阿里云账号下，华东2（上海）地域下存储空间examplebucket的文件夹srcfolder同步至本地文件夹examplefolder，命令如下：

```
./ossutil64 oss://examplebucket/srcfolder/  examplefolder/ -e oss-cn-shanghai.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

有关此命令的其他通用选项的更多信息，请参见[通用选项](/cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

