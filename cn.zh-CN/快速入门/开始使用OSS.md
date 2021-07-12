# 开始使用OSS

阿里云对象存储OSS（Object Storage Service）为您提供基于网络的数据存取服务。使用OSS，您可以通过网络随时存储和调用包括文本、图片、音视频在内的各类数据文件。

## 使用控制台

您可以通过OSS控制台创建Bucket，并将文件上传至Bucket。上传完成后，将文件（Object）下载至本地或者通过生成签名URL的方式将文件分享给第三方，供其下载或预览。更多信息，请参见[控制台使用流程](/cn.zh-CN/快速入门/控制台快速入门/控制台使用流程.md)。

观看以下视频快速了解如何通过控制台使用OSS。

## 使用命令行管理工具ossutil

ossutil是OSS的命令行工具，支持Windows、Linux、macOS系统。您可以通过ossutil提供的方便、简洁、丰富的Bucket和Object命令管理您的OSS。更多信息，请参见[命令行工具ossutil快速入门](/cn.zh-CN/快速入门/命令行工具ossutil快速入门.md)。

## 使用图形化管理工具ossbrowser

ossbrowser是OSS的图形化工具，支持Windows、Linux、macOS系统。您可以通过ossbrowser的图形化界面方便直观地管理Bucket、上传下载Object和文件夹（目录）、简化Policy授权等操作。更多信息，请参见[图形化管理工具ossbrowser快速入门](/cn.zh-CN/快速入门/图形化管理工具ossbrowser快速入门.md)。

ossbrowser是桌面式图形化工具，所以传输速度和性能不如命令行工具ossutil。

## 使用API和SDK

OSS提供Java、Python、PHP、Go等多种语言的API和SDK包，方便您快速进行二次开发。各语言SDK示例，请参见[OSS SDK示例](/cn.zh-CN/SDK 示例/简介.md) 。各API接口的详细信息，请参见[OSS API文档](/cn.zh-CN/API 参考/简介.md)。

## 基于OSS的文件系统管理

OSS的存储空间内部是扁平的，没有文件系统的目录等概念，所有的对象都直接隶属于其对应的存储空间。如果您想要像使用本地文件夹和磁盘那样来使用OSS存储服务，可以通过配置云存储网关来实现。通过云存储网关提供的NFS、SMB（CIFS）、iSCSI协议，OSS的存储资源会以Bucket为基础映射成本地文件夹或者磁盘。您可以通过文件读写操作访问OSS资源，无缝衔接基于POSIX和块访问协议的应用，降低应用改造和学习成本。更多信息，请参见[通过云存储网关挂载OSS](/cn.zh-CN/控制台用户指南/文件管理/通过云存储网关挂载OSS.md)。

