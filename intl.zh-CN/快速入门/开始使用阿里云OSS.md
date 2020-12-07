# 开始使用阿里云OSS

阿里云对象存储OSS（Object Storage Service）为您提供基于网络的数据存取服务。使用OSS，您可以通过网络随时存储和调用包括文本、图片、音视频在内的各类数据文件。

**说明：** 初次使用OSS时，建议您先通过产品简介的文档了解OSS基本概念、使用场景、使用限制等。

OSS将各类文件以对象（Object）的形式上传到存储空间（Bucket）中。您可以进行以下操作：

-   创建Bucket，并向Bucket上传Object。
-   获取已上传Object的地址进行分享和下载。
-   通过读写权限ACL、Bucket Policy、RAM Policy等功能对访问用户进行授权和鉴权。
-   通过阿里云管理控制台、各种便捷工具以及丰富的SDK包执行OSS的基本和高级操作。

## 快速开始

OSS的基本操作流程如下：

![quickstart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0955536061/p140852.png)

1.  [开通OSS服务](/intl.zh-CN/快速入门/开通OSS服务.md)
2.  [创建存储空间](/intl.zh-CN/快速入门/创建存储空间.md)
3.  [上传对象](/intl.zh-CN/快速入门/上传文件.md)
4.  [下载或分享对象](/intl.zh-CN/快速入门/下载文件.md)

## 使用控制台

您可以通过OSS控制台执行OSS的基本和高级操作，详情请参见[OSS控制台用户指南](/intl.zh-CN/控制台用户指南/登录OSS管理控制台/使用阿里云账号登录OSS管理控制台.md)。

观看以下视频快速了解如何通过控制台使用OSS。

## 使用图形化管理工具ossbrowser

ossbrowser是OSS的图形化工具，支持Windows、Linux、macOS系统。您可以通过ossbrowser的图形化界面方便直观地管理Bucket、上传下载Object和文件夹（目录）、简化Policy授权等操作。更多信息，请参见[ossbrowser快速开始](/intl.zh-CN/常用工具/图形化管理工具ossbrowser/快速开始.md)。

ossbrowser是桌面式图形化工具，所以传输速度和性能不如ossutil。

## 使用命令行管理工具ossutil

ossutil是OSS的命令行工具，支持Windows、Linux、macOS系统。您可以通过ossutil提供的方便、简洁、丰富的Bucket和Object命令管理您的OSS。支持Bucket的创建与管理、Object和文件夹（目录）的上传下载、断点续传、并发上传等。更多信息，请参见[ossutil快速开始](/intl.zh-CN/常用工具/命令行工具ossutil/概述.md)。

## 使用API和SDK

OSS提供Java、Python、PHP、Go等多种语言的API和SDK包，方便您快速进行二次开发。各语言SDK示例，请参见[OSS SDK示例](/intl.zh-CN/SDK 示例/简介.md) 。各API接口的详细信息，请参见[OSS API文档](/intl.zh-CN/API 参考/简介.md)。

## 基于OSS的文件系统管理

OSS的存储空间内部是扁平的，没有文件系统的目录等概念，所有的对象都直接隶属于其对应的存储空间。如果您想要像使用本地文件夹和磁盘那样来使用OSS存储服务，可以通过配置云存储网关来实现。通过云存储网关提供的NFS、SMB（CIFS）、iSCSI协议，OSS的存储资源会以Bucket为基础映射成本地文件夹或者磁盘。您可以通过文件读写操作访问OSS资源，无缝衔接基于POSIX和块访问协议的应用，降低应用改造和学习成本。更多信息，请参见[配置云存储网关](/intl.zh-CN/控制台用户指南/上传、下载和管理文件/配置云存储网关.md)。

## 更多参考

OSS的更多高级操作，请参见[OSS开发指南](/intl.zh-CN/开发指南/基本概念.md)。

