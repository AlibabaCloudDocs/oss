# cors-options（检测跨域请求）

cors-options命令用于测试存储空间（Bucket）是否允许指定的跨域访问请求。

**说明：**

-   本文各命令行示例均基于Linux 64位系统，其他系统请将命令开头的./ossutil64替换成对应的Binary名称。详情请参见[命令行工具ossutil快速入门](/cn.zh-CN/快速入门/命令行工具ossutil快速入门.md)。
-   跨域访问功能详情请参见[设置跨域资源共享](/cn.zh-CN/开发指南/存储空间（Bucket）/设置跨域资源共享.md)。
-   设置跨域访问的命令请参见[cors（跨域资源共享）](/cn.zh-CN/常用工具/命令行工具ossutil/常用命令/cors（跨域资源共享）.md)。

## 命令格式

```
./ossutil cors-options --acr-method <value> --origin <value> --acr-headers <value> oss://bucket/[object]
```

cors-options命令向OSS发送http options请求，测试目标OSS是否允许自己的跨域请求：

-   --acr-method：http请求头Access-Control-Request-Method的值，可选值为GET、PUT、POST、DELETE、HEAD。
-   --origin：http请求头Origin字段的值，表示请求来源域，用来标示跨域请求。
-   --acr-headers：http请求头Access-Control-Request-Headers的值，表示在实际请求中会用到的除简单头部之外的头。如果有多个取值，各个header用英文的逗号（,）分隔，再加上双引号。例如`--acr-headers "header1,header2,header3"`。

## 使用示例

```
./ossutil cors-options --acr-method  put --origin "www.aliyun.com" oss://bucket1
Access-Control-Allow-Methods: GET, POST, PUT
Access-Control-Allow-Origin: *
Access-Control-Max-Age: 0
```

## 常用选项

您可以在使用cors-options命令时附加如下选项：

|选项名称|描述|
|----|--|
|--origin|http请求头Origin字段的值，表示请求来源域，用来标示跨域请求。|
|--acr-method|http请求头Access-Control-Request-Method的值，可选值为：GET、PUT、POST、DELETE、HEAD。|
|--acr-headers|http请求头Access-Control-Request-Headers的值，表示在实际请求中会用到的除简单头部之外的头。如果有多个取值，各个header用英文的逗号（,）分隔，再加上双引号。例如：--acr-headers "header1,header2,header3"。|
|--encoding-type|输入或者输出的Object名的编码方式，目前只支持url编码，即指定该选项时，取值为url。如果不指定该选项，则表示Object名未经过编码。Bucket名不支持url编码。|
|--loglevel|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。 |
|--proxy-host|网络代理服务器的url地址，支持http、https、socks5。例如http://120.79.\*\*.\*\*:3128、 socks5://120.79.\*\*.\*\*:1080。|
|--proxy-user|网络代理服务器的用户名，默认为空。|
|--proxy-pwd|网络代理服务器的密码，默认为空。|

**说明：** 更多通用选项请参见[查看选项](/cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

