# revert-versioning（恢复版本）

revert-versioning命令用于在开启版本控制的存储空间（Bucket）中，将文件（Object）删除标记状态IS-LATEST属性为true的删除标记删除，以恢复文件到最近的版本。

**说明：**

-   本文各命令行示例均基于Linux 64位系统，其他系统请将命令开头的./ossutil64替换成对应的Binary名称。详情请参见[命令行工具ossutil快速入门](/intl.zh-CN/快速入门/命令行工具ossutil快速入门.md)。
-   版本控制功能介绍请参见[版本控制介绍](/intl.zh-CN/开发指南/数据安全/版本控制/版本控制介绍.md)。
-   删除标记的介绍请参见[删除标记](/intl.zh-CN/开发指南/数据安全/版本控制/删除标记.md)。

## 命令格式

```
./ossutil revert-versioning oss://bucket[/prefix] [--encoding-type encodeType] [-r] [--start-time startTime] [--end-time endTime]  [--include include-pattern] [--exclude exclude-pattern] [--payer requester]
```

Bucket已开启版本控制时，此命令通过删除已删除Object的最新删除标记，将Object恢复为最近的一个版本。

## 使用示例

-   恢复单个处于删除状态的Object为最近的版本

    ```
    ./ossutil revert-versioning oss://bucket1/test.txt
    ```

-   恢复指定前缀处于删除状态的Object为最近的版本

    ```
    ./ossutil revert-versioning oss://bucket1/prefix -r
    ```

-   恢复Bucket内所有处于删除状态的Object为最近的版本

    ```
    ./ossutil revert-versioning oss://bucket1  -r
    ```

-   恢复处于删除状态，且删除时间在指定时间范围内的Object为最近的版本

    起始时间为北京时间2020年6月16日16:22:58，结束时间为北京时间2020年6月16日16:39:38。

    ```
    ./ossutil revert-versioning oss://bucket1/test -r --start-time 1592295778 --end-time 1592296778
    ```

    -   --start-time：值为Linux或Unix系统下面的时间戳，如果输入这个选项，Object的删除时间早于该时间的将被忽略。
    -   --end-time：值为Linux或Unix系统下面的时间戳，如果输入这个选项，Object的删除时间晚于该时间的将被忽略。
-   恢复处于删除状态，且匹配指定条件的Object为最近的版本

    您可以使用--include或--exclude参数，在恢复时选定符合条件的Object。详情请参见[过滤选项](/intl.zh-CN/常用工具/命令行工具ossutil/常用命令/cp/简介.md)。

    -   恢复所有TXT文件

        ```
        
        ./ossutil revert-versioning oss://bucket1 --include "*.txt" -r
        ```

    -   恢复所有非JPG格式的文件

        ```
        
        ./ossutil revert-versioning oss://bucket1 --exclude "*.jpg" -r
        ```


## 常用选项

使用revert-versioning命令加不同的选项可以恢复不同的Object，常用选项如下：

|参数名称|描述|
|----|--|
|-r，--recursive|递归进行操作。当指定该选项时，命令会对Bucket下所有符合条件的Object进行操作，否则只对指定的单个Object进行操作。|
|--start-time|值为Linux或Unix系统下面的时间戳，如果输入这个选项，最后更新时间早于该时间的Object会被忽略。|
|--end-time|值为Linux或Unix系统下面的时间戳，如果输入这个选项，最后更新时间晚于该时间的Object会被忽略。|
|--include|包含对象匹配模式，如：\*.jpg。|
|--exclude|不包含对象匹配模式，如：\*.txt。|
|--encoding-type|输入或者输出的Object名的编码方式，目前只支持url编码，即指定该选项时，取值为url。如果不指定该选项，则表示Object名未经过编码。Bucket名不支持url编码。|
|--payer|请求的支付方式，如果为请求者付费模式，可以将该值设置成requester。|
|--loglevel|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。 |
|--proxy-host|网络代理服务器的url地址，支持http、https、socks5。例如http://120.79.\*\*.\*\*:3128、 socks5://120.79.\*\*.\*\*:1080。|
|--proxy-user|网络代理服务器的用户名，默认为空。|
|--proxy-pwd|网络代理服务器的密码，默认为空。|

**说明：** 更多通用选项请参见[查看选项](/intl.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

