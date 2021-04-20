# Bucket Policy概述

Bucket Policy是阿里云OSS推出的针对Bucket的授权策略，您可以通过Bucket Policy授权其他用户访问您指定的OSS资源。

## 使用场景

Bucket Policy通常应用于以下场景的授权访问：

-   需要进行跨账号或对匿名用户授权访问或管理整个Bucket或Bucket内的部分资源。
-   需要对同账号下的不同RAM用户授予访问或管理Bucket资源的不同权限，例如只读、读写或完全控制的权限。

## 操作方式

您可以选用以下多种操作方式配置不同场景的Bucket Policy。

|操作方式|说明|
|----|--|
|控制台|-   [通过控制台图形化界面的方式配置Bucket Policy](/cn.zh-CN/控制台用户指南/文件管理/通过Bucket Policy授权用户访问指定资源.md)
-   [通过策略语法的方式配置Bucket Policy](/cn.zh-CN/控制台用户指南/文件管理/通过Bucket Policy授权用户访问指定资源.md)
-   [基于Bucket Policy实现跨部门数据共享](/cn.zh-CN/开发指南/数据安全/访问控制/Bucket Policy/基于Bucket Policy实现跨部门数据共享.md)
-   [基于Bucket Policy实现跨账号访问OSS](/cn.zh-CN/开发指南/数据安全/访问控制/Bucket Policy/教程示例：基于Bucket Policy实现跨账号访问OSS.md) |
|命令行工具ossutil|[bucket-policy（授权策略）](/cn.zh-CN/常用工具/命令行工具ossutil/常用命令/bucket-policy（授权策略）.md)|
|不同语言SDK|-   [Java SDK](/cn.zh-CN/SDK 示例/Java/存储空间/授权策略.md)
-   [PHP SDK](/cn.zh-CN/SDK 示例/PHP/存储空间/授权策略.md)
-   [Node.js SDK](/cn.zh-CN/SDK 示例/Node.js/存储空间/授权策略.md)
-   [Python SDK](/cn.zh-CN/SDK 示例/Python/存储空间/授权策略（Policy）.md)
-   [.NET SDK](/cn.zh-CN/SDK 示例/.NET/存储空间/授权策略.md)
-   [Go SDK](/cn.zh-CN/SDK 示例/Go/存储空间/授权策略.md)
-   [C++ SDK](/cn.zh-CN/SDK 示例/C++/存储空间/授权策略.md) |

