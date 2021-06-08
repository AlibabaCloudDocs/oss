# 在OSS之间同步文件

您可以使用sync命令将同一个地域下源存储空间（Bucket）中的文件（Object）同步到目标存储空间，或者将同一存储空间下的其中一个文件夹的文件同步到另一个文件夹。

## 注意事项

-   Binary名称

    本文各命令行示例均基于Linux 64位系统，其他系统请将命令开头的./ossutil64替换成对应的Binary名称。详情请参见[命令行工具ossutil快速入门](/cn.zh-CN/快速入门/命令行工具ossutil快速入门.md)。

-   文件数量

    通过sync命令执行一次同步任务时，最多可同步100万个文件。当同步的文件个数超出100万时，将报错over max sync numbers 1000000.。

-   sync命令与cp命令的区别

    -   sync命令强制以递归的方式遍历指定文件夹内所有文件或子文件夹。cp命令需增加-r选项才会进行递归操作。
    -   通过sync命令将数据同步到OSS时，ossutil支持通过--delete选项将目的端存在而源端不存在的文件都删除，仅保留本次同步的文件。cp命令不支持--delete选项。
    -   sync不支持--version-id选项，无法在已开启版本控制的Bucket内同步历史版本文件。cp命令支持--version-id选项。
    除以上区别外，sync命令与cp命令用法类似。有关cp命令的用法及示例，请参见[cp（拷贝文件）](/cn.zh-CN/常用工具/命令行工具ossutil/常用命令/cp/拷贝文件.md)。


## 命令格式

```
./ossutil64 sync cloud\_url cloud\_url
[-f --force]
[-u --update]
[--delete]
[--enable-symlink-dir]
[--disable-all-symlink]
[--disable-ignore-error]
[--only-current-dir]
[--output-dir <value>]
[--bigfile-threshold <value>]
[--part-size <value>]
[--checkpoint-dir <value>]
[--encoding-type <value>]
[--snapshot-path <value>]
[--include <value>]
[--exclude <value>]
[--meta <value>]
[--acl <value>]
[--maxupspeed <value>]
[--disable-crc64]
[--payer <value>]
[-j, --job <value>]
[--parallel <value>]
[--retry-times <value>]
[--tagging <value>]
```

参数及选项说明如下：

|配置项|说明|
|---|--|
|cloud\_url|分别填写OSS源文件夹路径以及目标文件夹路径。格式为`oss://bucketname/path/`。例如将目标存储空间examplebucket的srcdir文件夹同步到destdir文件夹，则源文件夹路径填写为`oss://examplebucket/srcdir/`，目标文件夹路径填写为`oss://examplebucket/destcdir/`。如果输入的`cloud_url`没有以正斜线（/）结尾，ossutil会自动在结尾处添加一个正斜线（/）。|
|-f --force|强制操作，不进行询问提示。|
|-u，--update|只有当目标文件不存在，或源文件的最后修改时间晚于目标文件时，ossutil才会执行同步操作。|
|--delete|将目的端指定路径下的其他文件都删除，仅保留本次同步的文件。**警告：** 建议您使用--delete选项前开启[版本控制](/cn.zh-CN/开发指南/数据安全/版本控制/版本控制介绍.md)，防止数据被误删。 |
|--enable-symlink-dir|同步链接子目录。|
|--disable-all-symlink|同步目录时，忽略所有的链接子文件以及链接子目录。|
|--disable-ignore-error|批量操作时不忽略错误。|
|--only-current-dir|仅同步当前目录下的文件，忽略子目录及子目录下的文件。|
|--output-dir|指定输出文件所在的目录。输出文件是指批量同步文件出错时产生的report文件，默认保存在当前目录下的ossutil\_output目录。|
|-bigfile-threshold|设置断点续传文件的大小阈值，单位为字节。默认值：100 MB

取值范围：0~9223372036854775807 |
|--part-size|设置分片大小，单位为字节。默认情况下ossutil会根据文件大小自行计算合适的分片大小值。取值范围：1~9223372036854775807 |
|--checkpoint-dir|指定断点续传记录信息所在的目录。断点续传操作失败时，ossutil会自动创建名为`.ossutil_checkpoint`的目录，并在该目录下记录checkpoint信息，断点续传成功后会删除该目录。如果指定了该选项，请确保指定的目录可以被删除。|
|--encoding-type|文件名称的编码方式。取值为url。如果不指定该选项，则表示文件名称未经过编码。|
|--snapshot-path|指定保存同步文件时的快照信息所在的目录。在下一次同步文件时，ossutil会读取指定目录下的快照信息进行增量同步。|
|--include|包含符合指定条件的所有文件。|
|--exclude|不包含任何符合指定条件的文件。|
|--meta|设置文件的元信息，格式为`header:value#header:value`，示例为`Cache-Control:no-cache#Content-Encoding:gzip`。有关元信息的介绍，请参见[set-meta](/cn.zh-CN/常用工具/命令行工具ossutil/常用命令/set-meta.md)。|
|--acl|文件的读写权限ACL。取值如下：-   default：继承Bucket的读写权限。
-   private（默认值）：只有该Bucket的拥有者可以对该Bucket内的文件进行读写操作，其他人无法访问该Bucket内的文件。
-   public-read：只有Bucket拥有者可以对该Bucket内的文件进行写操作，其他用户（包括匿名访问者）都可以对该Bucket中的文件进行读操作。这有可能造成您数据的外泄以及费用激增，若被人恶意写入违法信息还可能会侵害您的合法权益。除特殊场景外，不建议您配置此权限。
-   public-read-write：任何人（包括匿名访问者）都可以对该Bucket内文件进行读写操作。这有可能造成您数据的外泄以及费用激增，请谨慎操作。 |
|--maxupspeed|最大上传速度，单位为KB/s。默认值为0，表示不限制上传速度。|
|--disable-crc64|关闭CRC64数据校验。|
|--payer|请求的支付方式。如果希望访问指定路径下的资源产生的流量、请求次数等费用由请求者支付，请将此选项的值设置为requester。|
|-j，--job|多文件操作时的并发任务数，默认值为3，取值范围为1~10000。|
|--parallel|单文件操作时的并发任务数，取值范围为1-10000。 如果不设置此选项，默认由ossutil根据操作类型和文件大小自行决定。|
|--retry-times|发生错误后的重试次数。默认值为10，取值范围为1~500。|
|--tagging|文件的标签信息，格式为`TagkeyA=TagvalueA&TagkeyB=TagvalueB....`。|

## 使用示例

某阿里云账号下有examplebucket1和examplebucket2两个Bucket，examplebucket1下有exampledir和srcdir两个文件夹。examplebucket2下有destdir文件夹。文件结构如下：

```
examplebucket1          examplebucket2
├── exampledir/            └── destdir/
│      ├── a.txt               ├── c.txt
│      └── b.txt               └── e.txt
└── srcdir/     
        └── d.txt       
```

示例场景及命令如下：

-   将examplebucket1的exampledir文件夹同步到srcdir文件夹内。

    ```
    ./ossutil sync oss://examplebucket1/exampledir/  oss://examplebucket1/srcdir/
    ```

    命令执行完毕后，exampledir文件夹内的所有文件被同步到srcdir文件夹下。

    ```
    examplebucket1                    examplebucket1         
    ├── exampledir/                  ├── exampledir/
    │      ├── a.txt                │      ├── a.txt 
    │      └── b.txt                │      └── b.txt 
    └── srcdir/                      └── srcdir/    
            └── d.txt                         ├── a.txt
                                                 ├── b.txt
                                                 └── d.txt     
    ```

-   将examplebucket1的exampledir文件夹同步到examplebucket2的destdir文件夹。

    ```
    ./ossutil sync oss://examplebucket1/exampledir/ oss://examplebucket2/destdir/
    ```

    命令执行后，examplebucket1的exampledir文件夹中所有文件被同步到examplebucket2的destdir文件夹。

    ```
    examplebucket1          examplebucket2
    ├── exampledir/            └── destdir/
    │      ├── a.txt               ├── a.txt  
    │      └── b.txt               ├── b.txt
    └── srcdir/                     ├── c.txt 
            └── d.txt               └── e.txt
    ```

-   结合--delete选项将examplebucket1的所有文件同步到examplebucket2，并删除examplebucket2存在而examplebucket1不存在的文件。

    ```
    ./ossutil sync oss://examplebucket1 oss://examplebucket2 --delete
    ```

    命令执行后，examplebucket1的exampledir和test文件夹将同步到examplebucket2，examplebucket2内已有的文件c.txt和e.txt将被删除。

    ```
    examplebucket1          examplebucket2
    ├── exampledir/            ├── exampledir/
    │      ├── a.txt          │      ├── a.txt  
    │      └── b.txt          │      └── b.txt
    └── srcdir/                └── srcdir/  
            └── d.txt                  └── d.txt                  
    ```

-   以上示例同步成功后，返回结果中将包含同步的文件数量、文件大小以及完成同步操作所用时长，示例如下：

    ```
    Succeed: Total num: 2, size: 750,081. OK num: 2(upload 2 files).
    
    average speed 1641000(byte/s)
    ```


## 通用选项

当您需要通过命令行工具ossutil管理不同地域的Bucket时，可以通过-e选项切换至指定Bucket所属的Endpoint。当您需要通过命令行工具ossutil管理多个阿里云账号下的Bucket时，可以通过-i选项切换至指定账号的AccessKey ID，并通过-k选项切换至指定账号的AccessKey Secret。

例如，您需要将另一个阿里云账号下，华东2（上海）地域下源存储空间examplebucket的文件夹srcfolder同步至目标存储空间testbucket的文件夹examplefolder，命令如下：

```
./ossutil64 oss://examplebucket/srcfolder/  oss://testbucket/examplefolder/ -e oss-cn-shanghai.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

有关此命令的其他通用选项的更多信息，请参见[通用选项](/cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

