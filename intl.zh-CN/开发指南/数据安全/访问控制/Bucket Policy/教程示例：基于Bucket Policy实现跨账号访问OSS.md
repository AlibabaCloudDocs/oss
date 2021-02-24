# 教程示例：基于Bucket Policy实现跨账号访问OSS

阿里云OSS的资源默认都是私有的，若您希望您的合作伙伴可以访问您的OSS资源，可以通过Bucket Policy授予合作伙伴访问Bucket的权限。

公司A希望其合作公司B可以访问自己的OSS资源，但又不方便开放RAM用户给B公司。此时，A公司可以通过Bucket Policy授予合作伙伴访问Bucket的权限。B公司账号获得授权之后，可以在控制台添加A公司OSS资源的访问路径进行访问。

## 添加Bucket Policy

-   B公司账号
    1.  登录[RAM访问控制台](https://ram.console.aliyun.com)，创建RAM用户。

        详细配置方法请参见[创建RAM用户](/intl.zh-CN/用户管理/创建RAM用户.md)。

    2.  在RAM访问控制台，单击**用户**。
    3.  单击刚刚创建的RAM用户的用户名，查看并记录RAM用户的UID号。
-   A公司账号
    1.  登录阿里云[OSS控制台](https://oss.console.aliyun.com)。
    2.  单击**Bucket列表**，之后单击目标Bucket名称。
    3.  单击**文件管理** \> **授权** \> **新增授权**。

        **说明：** 您也可以单击**权限管理** \> **Bucket授权策略** \> **设置** \> **新增授权**。

    4.  在**新增授权**的对话框，填写授权策略。其中，**授权用户**选择**其他账号**，并填写B公司RAM用户的UID号。其他参数请参考[Bucket Policy](/intl.zh-CN/控制台用户指南/上传、下载和管理文件/添加Bucket授权策略（Bucket Policy）.md)。
    5.  单击**确定**。

## 登录RAM用户账号并添加访问路径

Bucket Policy添加完成之后，您还需要登录B公司RAM用户账号，添加A公司的Bucket访问路径。配置步骤如下：

1.  通过[RAM账号登录链接](http://signin.aliyun.com)登录B公司RAM用户。

2.  打开[OSS控制台](https://oss.console.aliyun.com)。

3.  单击左侧菜单栏**我的访问路径**后的加号（+），添加A公司授权访问的Bucket路径信息。

    -   **区域**：下拉选择A公司授权访问的Bucket所在地域。
    -   **Bucket**：输入A公司授权访问的Bucket名称。
    -   **访问路径**：添加A公司授权访问的Bucket访问路径。例如，仅允许访问根目录abc下test目录，则添加abc/test。

您也可以[创建AccessKey]()，并通过AccessKey使用[ossutil](/intl.zh-CN/常用工具/命令行工具ossutil/概述.md)、[ossbrowser](/intl.zh-CN/常用工具/图形化管理工具ossbrowser/快速开始.md)等工具访问被授权的Bucket。

