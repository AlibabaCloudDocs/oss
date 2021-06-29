# set-meta（文件元信息）

set-meta命令用于设置已上传对象（Object）的元信息。

**说明：** 本文命令均以Linux系统为例，实际使用时，请将命令名称改为您实际可执行程序文件的名称。例如Windows 32位系统的帮助命令为ossutil32.exe help。

## 命令格式

```
./ossutil set-meta oss://bucket[/prefix] [header:value#header:value...] [--update] [--delete] [-r] [-f] [-c file]
```

## 使用示例

-   设置指定Object的meta信息

    ```
    ./ossutil set-meta oss://bucket1/path/object x-oss-object-acl:private 
    ```

    未指定--update选项和--delete选项时，此命令将用指定的meta代替原来的meta。若未指定`[header:value#header:value...]`，对于不可删除的Header（不以X-Oss-meta开头的Header），其值不变；其他meta信息将会被删除。

    **说明：** Header不区分大小写，但Value区分大小写。当前可选的Header列表如下：

    ```
    Headers:
          Expires(time.RFC3339:2006-01-02T15:04:05Z07:00)
          X-Oss-Object-Acl
          Origin
          X-Oss-Storage-Class
          Content-Encoding
          Cache-Control
          Content-Disposition
          Accept-Encoding
          X-Oss-Server-Side-Encryption
          Content-Type
          以及以X-Oss-Meta-开头的header
    ```

    更多详情请参见[管理文件元信息](/cn.zh-CN/开发指南/对象/文件（Object）/管理文件元信息.md)。

-   设置指定前缀的所有Object的meta信息

    ```
    ./ossutil set-meta oss://bucket1/path/ Cache-Control:no-cache#x-oss-object-acl:private -r                             
    ```

    指定了-r选项，将会批量设置指定前缀的Object的meta信息。当一个Object操作出现错误时，会将出错Object的错误信息记录到report文件，并继续操作其他Object，成功操作的Object信息将不会被记录到report文件中。多个meta信息以井号（\#）隔开。

-   更新指定Object的meta信息

    ```
    ./ossutil set-meta oss://bucket1/path/object x-oss-object-acl:private --update
    ```

    仅更新指定Object的指定Header为输入的value值，其中value可以为空。指定Object的其他meta信息不会改变。--update（可简写为-u）和--delete选项不能同时指定。

-   删除指定Object的meta信息

    ```
    ./ossutil set-meta oss://bucket1/obj1 X-Oss-Meta-delete --delete
    ```

    指定--delete选项时，ossutil会删除指定Object的指定Header，此时value必须为空，指定Object的其他meta信息不会改变。对于不可删除的Headers（不以X-Oss-Meta-开头的header），该选项不起作用。--update和--delete选项不能同时指定

-   批量设置指定条件的Object的meta信息

    您可以使用--include/--exclude，在设置meta信息时选定符合条件的Object。详情请参见[过滤选项](/cn.zh-CN/常用工具/命令行工具ossutil/常用命令/cp/简介.md)。

    -   将所有文件类型为jpg的Object设置为低频存储

        ```
        ./ossutil64 set-meta oss://my-bucket/path X-Oss-Storage-Class:IA --include "*.jpg" -u -r                                        
        ```

    -   将所有Object名包含abc且文件类型不为jpg和txt的Object设置为标准存储

        ```
        ./ossutil set-meta oss://my-bucket/path X-Oss-Storage-Class:Standard --include "*abc*" --exclude "*.jpg" --exclude "*.txt" -u -r
        ```

-   在已开启版本控制的Bucket内设置指定版本的Object的meta信息

    ```
    ./ossutil set-meta oss://bucket1/test.jpg X-Oss-Storage-Class:Standard --version-id  CAEQARiBgID8rumR2hYiIGUyOTAyZGY2MzU5MjQ5ZjlhYzQzZjNlYTAyZDE3MDRk
    ```

    在使用--version-id选项前，需使用[ls --all-versions](/cn.zh-CN/常用工具/命令行工具ossutil/常用命令/ls（列举）.md)命令获取Object的versionid。

    **说明：** `--version-id`选项仅支持在已开启版本控制的Bucket内使用。开启Bucket版本控制命令请参见[bucket-versioning（版本控制）](/cn.zh-CN/常用工具/命令行工具ossutil/常用命令/bucket-versioning（版本控制）.md)。


## 常用选项

您可以在使用set-meta命令的时候指定如下选项：

|选项名称|描述|
|----|--|
|-u，--update|用于更新meta信息。|
|--delete|用于删除指定的meta信息。|
|-r，--recursive|递归进行操作。当指定该选项时，命令会对Bucket下所有符合条件的Object进行操作，否则只对指定的单个Object进行操作。|
|-f，--force|强制操作，不进行询问提示。|
|--include|包含对象匹配模式，如：\*.jpg。|
|--exclude|不包含对象匹配模式，如：\*.txt。|
|--encoding-type|输入或者输出的Object名的编码方式，目前只支持url编码，即指定该选项时，取值为url。如果不指定该选项，则表示Object名未经过编码。Bucket名不支持url编码。|
|-j，--jobs|多文件操作时的并发任务数，默认值：3，取值范围：1-10000。|
|-L，--language|设置ossutil工具的语言，默认值：CH，取值范围：CH/EN，若设置成“CH”，请确保您的系统编码为UTF-8。|
|--output-dir|指定输出文件所在的目录，输出文件目前包含：cp命令批量拷贝文件出错时所产生的report文件（关于report文件更多信息，请参考cp命令帮助）。默认值为：当前目录下的ossutil\_output目录。|
|--loglevel|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。 |
|--version-id|操作指定版本的Object，仅支持在已开启版本控制的Bucket内使用。|
|--retry-times|当错误发生时的重试次数，默认值：10，取值范围：1-500。|
|--proxy-host|网络代理服务器的url地址，支持http、https、socks5。例如http://120.79.\*\*.\*\*:3128、 socks5://120.79.\*\*.\*\*:1080。|
|--proxy-user|网络代理服务器的用户名，默认为空。|
|--proxy-pwd|网络代理服务器的密码，默认为空。|

**说明：** 更多通用选项请参见[查看选项](/cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

