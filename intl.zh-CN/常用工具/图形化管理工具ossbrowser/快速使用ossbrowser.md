# 快速使用ossbrowser

ossbrowser是阿里云官方提供的OSS图形化管理工具，提供类似Windows资源管理器的功能。使用ossbrowser，您可以快速完成存储空间（Bucket）和文件（Object）的相关操作。

## 前提条件

已安装并登录ossbrowser。具体操作，请参见[安装并登录ossbrowser](/intl.zh-CN/常用工具/图形化管理工具ossbrowser/安装并登录ossbrowser.md)。

## Bucket或Object相关操作

ossbrowser支持的Bucket或Object级别的操作与控制台支持的操作类似，请按照ossbrowser界面指引完成Bucket或Object相关操作。

|分类|配置说明|
|--|----|
|Bucket相关操作|-   创建Bucket

Bucket是您用于存储Object的容器。在上传任何文件到OSS之前，您必须先创建存储空间。有关创建Bucket时如何填写Bucket名称、选择所在地域、ACL权限和存储类型信息，请参见[创建存储空间](/intl.zh-CN/控制台用户指南/存储空间管理/创建存储空间.md)。

-   删除Bucket

如果您不再需要Bucket，请将其删除，以免产生额外费用。有关删除Bucket的注意事项，请参见[删除存储空间](/intl.zh-CN/控制台用户指南/存储空间管理/基础设置/删除存储空间.md)。 |
|Object相关操作|ossbrowser支持上传、下载、预览、移动或复制文件、生成文件URL等操作。操作过程中有如下注意事项：-   ossbrowser默认使用分片上传和断点续传上传文件，上传文件最大不能超过48.8 TB。若您因意外中断了文件上传的过程，且未继续完成该文件的上传，则已上传的部分会以碎片（Part）的形式存储在OSS的存储空间（Bucket）中。若您不再需要这些Part，建议您通过以下方式删除，以免产生额外的存储费用。
    -   手动删除Part的具体操作，请参见[删除碎片](#li_3rq_l1e_6gg)。
    -   通过生命周期规则自动删除Part的具体操作，请参见[设置生命周期规则](/intl.zh-CN/控制台用户指南/存储空间管理/基础设置/设置生命周期规则.md)。
-   移动或复制文件最大不能超过5 GB，5 GB以上文件的移动或复制操作建议使用[ossutil](/intl.zh-CN/常用工具/命令行工具ossutil/概述.md)。

Object各类操作的其他注意事项，请参见控制台用户指南对应文档。 |

