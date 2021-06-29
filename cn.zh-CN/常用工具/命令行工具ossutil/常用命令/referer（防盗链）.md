# referer（防盗链）

referer命令用于添加、修改、查询、删除存储空间（Bucket）的防盗链配置。

**说明：**

-   本文各命令行示例均基于Linux 64位系统，其他系统请将命令开头的./ossutil64替换成对应的Binary名称。详情请参见[命令行工具ossutil快速入门](/cn.zh-CN/快速入门/命令行工具ossutil快速入门.md)。
-   防盗链功能介绍请参见[防盗链](/cn.zh-CN/开发指南/数据安全/防盗链.md)。

## 命令格式

-   添加/修改防盗链配置

    ```
    ./ossutil referer --method put oss://bucket referer-value [--disable-empty-referer]
    ```

    若Bucket未设置防盗链，此命令将添加防盗链配置；若Bucket已设置防盗链，此命令将修改防盗链配置。

    -   referer-value：设置Referer白名单，仅允许指定的域名访问OSS资源。支持通配符星号（\*）和问号（?），多个域名可用空格隔开。
    -   --disable-empty-referer：选择是否允许Referer为空。增加此选项表示不允许Referer为空，反之表示允许Referer为空。如果不允许空Referer，则只有HTTP或HTTPS header中包含Referer字段的请求才能访问OSS资源。
-   查看防盗链配置

    ```
    ./ossutil referer --method get oss://bucket  [local_xml_file]
    ```

    local\_xml\_file为文件路径参数。若填写，则将防盗链的配置保存为本地文件；若置空，则将防盗链的配置输出到屏幕上。

-   删除防盗链配置

    ```
    ./ossutil referer --method delete oss://bucket
    ```


## 使用示例

-   设置防盗链，且不允许Referer为空：

    ```
    ./ossutil referer --method put oss://bucket1 www.test1.com www.test2.com --disable-empty-referer
    ```

-   查看防盗链配置

    ```
    ./ossutil referer --method get oss://bucket1  /file/referer.xml
    ```

-   删除防盗链配置

    ```
    ./ossutil referer --method delete oss://bucket1
    ```


## 常用选项

您可以在使用referer命令时附加如下选项：

|选项名称|描述|
|----|--|
|--method|表示http的请求类型。取值： -   put：添加或修改配置。
-   get：获取配置。
-   delete：删除配置。 |
|--disable-empty-referer|选择是否允许Referer为空。增加此选项表示不允许Referer为空，反之表示允许Referer为空。如果不允许空Referer，则只有HTTP或HTTPS header中包含Referer字段的请求才能访问OSS资源。|
|--loglevel|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。 |
|--proxy-host|网络代理服务器的url地址，支持http、https、socks5。例如http://120.79.\*\*.\*\*:3128、 socks5://120.79.\*\*.\*\*:1080。|
|--proxy-user|网络代理服务器的用户名，默认为空。|
|--proxy-pwd|网络代理服务器的密码，默认为空。|

**说明：** 更多通用选项请参见[查看选项](/cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

