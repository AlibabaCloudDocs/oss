# logging（日志管理）

logging命令用于添加、修改、查询、删除存储空间（Bucket）的日志管理配置。

**说明：**

-   本文各命令行示例均基于Linux 64位系统，其他系统请将命令开头的./ossutil64替换成对应的Binary名称。详情请参见[命令行工具ossutil快速入门](/intl.zh-CN/快速入门/命令行工具ossutil快速入门.md)。
-   日志存储功能详情请参见[日志转存](/intl.zh-CN/开发指南/日志管理/日志转存.md)。

## 命令格式

-   添加/修改日志管理配置

    ```
    ./ossutil logging --method put oss://bucket  oss://target-bucket/[prefix]
    ```

    若Bucket未开启日志管理，此命令将Bucket的访问日志以Object的形式保存到target-bucket中；若Bucket已开启日志管理，此命令可修改日志记录的存储位置。

    prefix参数可指定日志记录存储的目录和前缀。若填写，日志将被记录到target-bucket的指定目录下；若不填写，则保存到target-bucket的根目录下。日志文件命名规则请参见[日志记录命名规则](/intl.zh-CN/控制台用户指南/存储空间管理/日志管理/设置日志转存.md)。

-   查看日志管理配置

    ```
    ./ossutil logging --method get oss://bucket  [local_xml_file]
    ```

    local\_xml\_file为文件路径参数。若填写，则将日志管理的配置保存为本地文件；若置空，则将日志管理的配置输出到屏幕上。

-   删除日志管理配置

    ```
    ./ossutil logging --method delete oss://bucket
    ```


## 使用示例

-   添加日志管理配置

    ```
    ./ossutil logging --method put oss://bucket1  oss://bucket2/logging
    ```

-   查看日志管理配置

    ```
    ./ossutil logging --method get oss://bucket1  /file/logging.xml
    ```

-   删除日志配置

    ```
    ./ossutil logging --method delete oss://bucket1
    ```


## 常用选项

您可以在使用logging命令时附加如下选项：

|选项名称|描述|
|----|--|
|--method|表示http的请求类型。取值： -   put：添加或修改配置。
-   get：获取配置。
-   delete：删除配置。 |
|--loglevel|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。 |
|--proxy-host|网络代理服务器的url地址，支持http、https、socks5。例如http://120.79.\*\*.\*\*:3128、 socks5://120.79.\*\*.\*\*:1080。|
|--proxy-user|网络代理服务器的用户名，默认为空。|
|--proxy-pwd|网络代理服务器的密码，默认为空。|

**说明：** 更多通用选项请参见[查看选项](/intl.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

