# sync（同步文件）

sync命令用于将文件从源端同步到目的端。

## 注意事项

sync命令与cp命令用法相似，但有如下区别：

-   sync命令增加--delete选项，当同步数据的目的端为OSS时，ossutil会将目的端存在而源端不存在的文件都删除，仅保留本次同步的文件；当目的端为本地时，ossutil会强制要求增加--backup-dir选项，将目的端存在而源端不存在的文件迁移到指定路径。

    **警告：** 建议您使用--delete选项前开启[版本控制](/intl.zh-CN/开发指南/数据安全/版本控制/版本控制介绍.md)，防止数据被误删。

-   sync命令强制以recursive方式遍历指定文件夹内所有文件或子文件夹。
-   若同步的源或目的端为OSS，且文件夹名称未以正斜线（/）结尾，ossutil会自动在文件夹名称后增加正斜线（/）。
-   sync不支持--version-id选项，无法在已开启版本控制的Bucket内同步历史版本文件。

除以上区别外，sync命令支持cp命令的所有用法。cp命令使用方法请参见[cp](/intl.zh-CN/常用工具/命令行工具ossutil/常用命令/cp/简介.md)。

## 同步本地文件到OSS

-   命令格式

    ```
    ./ossutil sync file_url cloud_url  [-f] [-u] [--delete] [--backup-dir <value>] [--enable-symlink-dir] [--disable-all-symlink] [--disable-ignore-error] [--only-current-dir] [--output-dir=odir] [--bigfile-threshold=size] [--checkpoint-dir=cdir] [--snapshot-path=sdir] [--payer requester]
    ```

    参数说明：

    -   file\_url：本地文件夹路径。例如Linux系统文件路径`/localfolder/`；Windows系统文件路径`D:\localfolder\`。
    -   cloud\_url：OSS文件夹（文件目录）路径，格式为`oss://bucketname/path/`。例如`oss://examplebucket/exampledir/`。如果输入的`cloud_url`没有以正斜线（/）结尾，ossutil会自动在结尾处添加一个正斜线（/）。
-   使用示例

    OSS端名为examplebucket的Bucket下example文件夹内有文件a.txt、b.txt以及子文件夹C，本地端根目录下example文件夹内有d.txt文件。文件结构如下：

    ```
    examplebucket           本地根目录
    └── example/             └── example/
           ├── a.txt                └── d.txt
           ├── b.txt
           └── C/
    ```

    示例场景及命令如下：

    -   将本地example文件夹同步到OSS

        ```
        ./ossutil sync example/  oss://examplebucket/example/
        ```

        命令执行完毕后，examplebucket的example文件夹内新增d.txt文件。

        ```
        examplebucket           本地根目录
        └── example/             └── example/
               ├── a.txt                └── d.txt
               ├── b.txt
               ├── d.txt
               └── C/
        ```

    -   同步本地文件夹到OSS，并省略问询操作

        同步本地文件夹到OSS时，若目的端存在同名文件，ossutil会进行覆写操作的问询。若您确认目的端文件均可被覆写，可增加-f，--force选项强制执行覆写操作。命令如下：

        ```
        ./ossutil sync localfolder/ oss://examplebucket/destfolder/ -f
        ```

        命令执行完毕后，examplebucket的example文件夹内新增d.txt文件。

        ```
        examplebucket           本地根目录
        └── example/             └── example/
               ├── a.txt                └── d.txt
               ├── b.txt
               ├── d.txt
               └── C/
        ```

    -   同步本地文件夹到OSS，并删除OSS指定路径下已有而本地端没有的文件

        当同步数据的目的端为OSS时，可增加--delete，将目的端存在而源端不存在的文件都删除，仅保留本次同步的文件。

        ```
        ./ossutil sync example/  oss://examplebucket/example/ --delete
        ```

        命令执行完成后，本地端的example文件夹被同步到OSS端，同时会删除OSS端的文件a.txt、b.txt以及子文件夹C，即OSS端的example文件夹内仅保留d.txt文件。

        ```
        examplebucket           本地根目录
        └── example/             └── example/
               └── d.txt                └── d.txt
        ```


## 同步OSS文件到本地

-   命令格式

    ```
    ./ossutil sync cloud_url file_url  [-f] [-u] [--delete] [--backup-dir] [--only-current-dir] [--disable-ignore-error] [--output-dir=odir] [--bigfile-threshold=size] [--checkpoint-dir=cdir] [--range=x-y] [--payer requester]
    ```

    使用sync命令将OSS文件同步到本地，其中：

    -   file\_url：本地文件夹路径。例如Linux系统文件路径`/localfolder/`；Windows系统文件路径`D:\localfolder\`。
    -   cloud\_url：OSS文件夹路径，格式为`oss://bucketname/path/`。例如`oss://examplebucket/exampledir/`。
-   使用示例

    OSS端名为examplebucket的Bucket下example文件夹内有文件a.txt、b.txt以及子文件夹C，本地端根目录下example文件夹内有d.txt文件。文件结构如下：

    ```
    examplebucket           本地根目录
    └── example/             └── example/
           ├── a.txt                └── d.txt
           ├── b.txt
           └── C/
    ```

    示例场景及命令如下：

    -   将OSS的example文件夹同步到本地

        ```
        ./ossutil sync  oss://examplebucket/example/  example/ 
        ```

        命令执行完毕后，本地example文件夹内新增a.txt、b.txt以及子文件夹C。

        ```
        examplebucket           本地根目录
        └── example/             └── example/
               ├── a.txt                ├── a.txt 
               ├── b.txt                ├── b.txt
               └── C/                   ├── d.txt
                                           └── C/ 
        ```

    -   将OSS端example文件夹同步到本地，并迁移本地端指定路径下已有而OSS端没有的文件到backup文件夹

        ```
        ./ossutil sync oss://examplebucket/example/  example/  --delete  --backup-dir backup/
        ```

        命令执行完毕后，OSS端的example文件夹被同步到本地，同时本地example文件夹内原有的文件被迁移到backup文件夹内。即本地example文件夹仅保存a.txt、b.txt及子文件夹C，backup文件夹内新增d.txt文件。

        ```
        examplebucket           本地根目录
        └── example/             ├── example/
               ├── a.txt         │     ├── a.txt 
               ├── b.txt         │     ├── b.txt
               └── C/            │     └── C/                             
                                    └── backup/
                                           └──d.txt
        ```


## 在OSS之间同步文件

-   命令格式

    ```
    ./ossutil sync cloud_url cloud_url [-f] [-u] [--delete] [--backup-dir] [--only-current-dir] [--disable-ignore-error] [--output-dir=odir] [--bigfile-threshold=size] [--checkpoint-dir=cdir] [--payer requester]
    ```

    使用sync命令在OSS之间同步数据，同步的源和目的需在同一个地域。cloud\_url为OSS文件夹路径，格式为`oss://bucketname/path/`。例如`oss://examplebucket/exampledir/`。

-   使用示例

    某阿里云账号下有examplebucket1和examplebucket2两个Bucket，examplebucket1下有example和test两个文件夹；examplebucket2下有example文件夹。文件结构如下：

    ```
    examplebucket1          examplebucket2
    ├── example/            └── example/
    │      ├── a.txt               ├── a.txt
    │      └── b.txt               └── c.txt
    └── test/     
            └── d.txt       
    ```

    示例场景及命令如下：

    -   将examplebucket1的example文件夹同步到test文件夹内

        ```
        ./ossutil sync oss://examplebucket1/example/  oss://examplebucket1/test/
        ```

        命令执行完毕后，example文件夹内的所有文件被同步到test文件夹下。

        ```
        examplebucket1          examplebucket2
        ├── example/            └── example/
        │      ├── a.txt               ├── a.txt
        │      └── b.txt               └── c.txt
        └── test/        
                ├── a.txt
                ├── b.txt
                └── d.txt      
        ```

    -   将examplebucket1的example文件夹同步到examplebucket2的example文件夹

        ```
        ./ossutil sync oss://examplebucket1/example/ oss://examplebucket2/example/
        ```

        命令执行完毕后examplebucket1的example文件夹中所有文件被同步到examplebucket2的example文件夹。

        ```
        examplebucket1          examplebucket2
        ├── example/            └── example/
        │      ├── a.txt               ├── a.txt  
        │      └── b.txt               ├── b.txt
        └── test/                       └── c.txt 
                └── d.txt
        ```

    -   将examplebucket1的所有文件同步到examplebucket2内，并删除examplebucket2有而examplebucket1没有的文件

        ```
        ./ossutil sync oss://examplebucket1 oss://examplebucket2 --delete
        ```

        命令执行完毕后，examplebucket1的example和test文件夹被同步到examplebucket2根目录，examplebucket2内其他文件被删除。

        ```
        examplebucket1          examplebucket2
        ├── example/            ├── example/
        │      ├── a.txt       │      ├── a.txt  
        │      └── b.txt       │      └── b.txt
        └── test/               └── test/  
                └── d.txt               └── d.txt                  
        ```


## 常用选项

您可以在使用sync命令时附加如下选项：

|选项名称|描述|
|----|--|
|-f，--force|强制操作，不进行询问提示。|
|-u，--update|只有当目标文件不存在，或源文件的最后修改时间晚于目标文件时，ossutil才会执行同步操作。|
|--output-dir|指定输出文件所在的目录。输出文件为使用cp和sync命令进行批量拷贝出错时产生的report文件。

默认值：当前目录下的ossutil\_output目录。 |
|--bigfile-threshold|开启大文件断点续传的文件大小阈值。单位为Byte。默认值：100 MB

取值范围：0~9223372036854775807 |
|--part-size|分片大小。单位为Byte。默认情况下ossutil根据文件大小自行计算合适的分片大小值。如果有特殊需求或者需要性能调优，可以设置该值。

取值范围：1~9223372036854775807 |
|--checkpoint-dir|checkpoint目录的路径（默认值为：`.ossutil_checkpoint`），断点续传操作失败时，ossutil会自动创建该目录，并在该目录下记录checkpoint信息，操作成功会删除该目录。如果指定了该选项，请确保所指定的目录可以被删除。|
|--range|下载目标文件的指定字段形成一个新的文件，字段从0开始编号。 -   指定区间

例如`3-9`表示从第3个字节到第9个字节（包含第3和第9字节）。

-   指定开始位置

例如`3-`表示从第3个字节开始到文件结尾（包含第3个字节）。

-   指定结束位置

例如`-9`表示从0字节到第9个字节（包含第9个字节）。 |
|--encoding-type|输入或者输出的文件名的编码方式。取值：url。如果不指定该选项，则表示文件名未经过编码。

**说明：** Bucket名不支持URL编码。 |
|--include|匹配包含指定条件的文件。例如`*.jpg`表示匹配所有文件名以`.jpg`结尾的文件。|
|--exclude|匹配不包含指定条件的文件。例如`*.txt`表示匹配所有文件名不是以`.txt`结尾的文件。|
|--meta|设置Object的meta为`[header:value#header:value...]`，例如`Cache-Control:no-cache#Content-Encoding:gzip`。详情请参见[set-meta](/intl.zh-CN/常用工具/命令行工具ossutil/常用命令/set-meta.md)。|
|--acl|设置Object的访问权限。取值为： -   default（默认）：继承Bucket
-   private：私有
-   public-read：公共读
-   public-read-write：公共读写 |
|--snapshot-path|同步文件时，若指定--snapshot-path选项，ossutil在指定的目录下生成文件同步的快照信息，在下一次指定该选项同步文件时，ossutil会读取指定路径下的快照信息进行增量同步。详情请参见[上传并生成快照](/intl.zh-CN/常用工具/命令行工具ossutil/常用命令/cp/上传文件.md)。|
|--disable-crc64|关闭CRC64校验。ossutil传输数据时默认开启CRC64校验。 |
|--maxupspeed|最大上传速度。单位：KB/s。默认值：0（不受限制） |
|--payer|请求的支付方式，如果为请求者付费模式，可以将该值设置成`requester`。|
|-j，--jobs|多文件操作时的并发任务数。默认值：3

取值范围：1~10000 |
|--parallel|单文件内部操作的并发任务数，默认将由ossutil根据操作类型和文件大小自行决定。取值范围：1~10000 |
|--loglevel|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括HTTP请求和响应信息）。 |
|--retry-times|当错误发生时的重试次数。默认值：10

取值范围：1~500 |
|--proxy-host|网络代理服务器的URL地址，支持HTTP、HTTPS、SOCKS5。例如`http://120.79.**.**:3128`、 `socks5://120.79.**.**:1080`。|
|--proxy-user|网络代理服务器的用户名，默认为空。|
|--proxy-pwd|网络代理服务器的密码，默认为空。|
|--local-host|工具的本地IP地址。当您的电脑有多个IP地址的时候，您可以指定此项，ossutil将通过指定IP访问OSS。|
|--enable-symlink-dir|表示同步链接子目录，默认不同步。[probe命令](/intl.zh-CN/常用工具/命令行工具ossutil/常用命令/probe.md)可以探测是否存在死循环链接文件或者目录。|
|--only-current-dir|表示只同步当前目录下的文件，忽略掉当前目录下的子目录。|
|--disable-dir-object|表示同步文件时不为目录生成OSS对象。|
|--disable-all-symlink|同步时忽略所有的符号链接子文件以及符号链接子目录。|
|--tagging|同步文件时设置文件的对象标签，格式为`"abc=1&bcd=2&……"`。|
|--disable-ignore-error|批量操作时候不忽略错误。|
|--delete|当同步数据的目的端为OSS时，ossutil会将目的端指定路径下其他文件都删除，仅保留本次同步的文件；当目的端为本地时，ossutil会强制要求增加--backup-dir选项，将目的端存在而源端不存在的文件备份到指定路径。|
|--backup-dir|当同步的目的端为本地时，配合--delete选项将目的端存在而源端不存在的文件备份到指定路径。|

**说明：** 更多通用选项请参见[查看选项](/intl.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

