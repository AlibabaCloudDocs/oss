# OSS怎样上传下载文件夹（目录）？

阿里云OSS的数据模型为扁平型结构，所有文件（Object）都直接隶属于其对应的存储空间（Bucket）。因此，OSS缺少文件系统中类似于文件夹与子文件夹的层次结构。为了方便管理，OSS管理控制台将所有文件名以正斜线（/）结尾的文件显示为文件夹，实现类似于Windows文件夹的基本功能。

例如abc/efg/123.jpg这个路径的文件，在OSS管理控制台上看起来就是123.jgp存放在abc文件夹下的efg子文件夹中。

您可以通过以下方法上传或下载文件夹：

-   [OSS管理控制台](/intl.zh-CN/控制台用户指南/登录OSS管理控制台/OSS管理控制台概览.md)：图形化管理工具，提供类似Windows资源管理器的功能，使用简单。
    -   上传文件夹：在上传时，直接将文件夹拖拽到上传区域，即可保留文件夹的结构。详情请参见[上传文件](/intl.zh-CN/控制台用户指南/上传、下载和管理文件/上传文件.md)。
    -   下载文件夹：OSS控制台暂不支持直接下载文件夹，您可以在本地创建文件夹后，将Bucket中的文件批量下载到指定文件夹中。操作方式请参见[下载或分享文件](/intl.zh-CN/控制台用户指南/上传、下载和管理文件/下载文件.md)。
-   [ossbrowser](/intl.zh-CN/常用工具/图形化管理工具ossbrowser/快速开始.md)：图形化管理工具，提供类似Windows资源管理器的功能，使用简单。
    -   上传文件夹：在指定的Bucket或目录内，单击**目录**，之后选中需要上传的文件夹即可。您也可以直接将文件夹拖拽到ossbrowser中。详情请参见[上传文件](/intl.zh-CN/常用工具/图形化管理工具ossbrowser/快速开始.md)。
    -   下载文件夹：单击指定文件夹右侧的下载，即可下载文件夹。操作方式请参见[下载文件](/intl.zh-CN/常用工具/图形化管理工具ossbrowser/快速开始.md)。
-   [ossutil](/intl.zh-CN/常用工具/命令行工具ossutil/概述.md)：命令行管理工具，提供方便、简洁、丰富的OSS管理命令，操作性能好。
    -   上传文件夹：在上传文件时携带-r选项可上传文件夹。详情请参见[上传文件](/intl.zh-CN/常用工具/命令行工具ossutil/常用命令/cp/简介.md)。
    -   下载文件夹：在下载文件时携带-r选项可下载文件夹。详情请参见[下载文件](/intl.zh-CN/常用工具/命令行工具ossutil/常用命令/cp/简介.md)
-   [SDK](/intl.zh-CN/SDK 示例/简介.md)：提供丰富、完整的各类语言SDK demo，易于开发。
    -   上传文件夹：SDK不支持直接上传文件夹，您可以在上传时设置相同的文件名前缀，并使用正斜线（/）隔开。例如的上传a.txt、b.txt、c.txt三个文件到abc文件夹，在上传时设置ObjectName为abc/a.txt、abc/b.txt、abc/c.txt即可。
    -   下载文件夹：SDK不支持直接下载文件夹，您可以在下载时将文件下载到同一个本地文件夹中。

