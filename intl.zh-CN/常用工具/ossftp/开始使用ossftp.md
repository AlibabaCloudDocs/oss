# 开始使用ossftp

ossftp工具是一个特殊的FTP server，它接收普通FTP请求后，将对文件、文件夹的操作映射为对OSS的操作，从而使得您可以基于FTP协议来管理存储在OSS上的文件。

-   ossftp工具主要面向个人用户使用，生产环境请使用OSS SDK。
-   FTP Server支持Python2.6和Python2.7。
-   在目前的1.0版本中，考虑到安装部署的简便，ossftp工具不支持TLS加密。由于FTP协议是明文传输的，为了防止您的密码泄漏，建议将FTP Server和Client运行在同一台机器上，通过127.0.0.1:port的方式来访问。

## 功能简介

-   主要特性
    -   跨平台：支持Windows、Linux、Mac，32位和64位操作系统，图形界面和命令行。
    -   免安装： 解压后可直接运行。
    -   免设置：无需设置即可运行。
    -   透明化： FTP工具基于Python，您可以看到完整的源码，后续将开源到GitHub。
-   主要功能
    -   支持文件和文件夹的上传、下载、删除等操作，不支持重命名和移动操作。
    -   通过Multipart方式，分片上传大文件。
    -   支持大部分FTP指令，可以满足日常FTP的使用需求。

## 下载地址

-   Windows：[ossftp-1.0.3-win.zip](https://gosspublic.alicdn.com/ossftp/ossftp-1.0.3-win.zip)

    Windows默认不会安装Python2.7，所以安装包中包含了Python2.7，解压后即可使用。

-   Linux/macOS：[ossftp-1.0.3-linux-mac.zip](https://gosspublic.alicdn.com/ossftp/ossftp-1.0.3-linux-mac.zip)

    Linux/macOS系统默认安装Python2.7或Python2.6，所以安装包中不再包含可执行的Python，只包含了相关依赖库。


## 快速运行

1.  下载和您系统匹配的安装包。

2.  将安装包解压后运行。

    **说明：** 安装包解压后的路径不能包含中文。

    -   Windows系统

        解压后双击运行start.vbs即可。如双击无反应，请升级您的IE浏览器或设置其他浏览器为默认浏览器。

    -   Linux系统
        1.  解压下载的文件：

            ```
            $ unzip ossftp-1.0.3-linux-mac.zip
            ```

        2.  进入解压后的文件夹，运行start.sh：

            ```
            $ cd ossftp-1.0.3-linux-mac
            $ bash start.sh
            ```

        3.  使用一台可以访问这台服务器的，有图形化界面的电脑，通过浏览器访问FTP服务器操作界面。访问域名：`http://ServerIP:8192`。
    -   macOS系统

        解压后双击start.command，或者在终端运行`$ bash start.command`。

3.  配置FTP Server，保存配置并重启生效。

    ![fteserver](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4348778061/p139746.png)

    **说明：**

    -   安装包运行后会启动一个FTP Server，默认监听在127.0.0.1的2048端口。为了方便您对FTP Server的状态进行管控，还会启动一个Web服务器，监听在127.0.0.1的8192端口。
    -   FTP Server的管理控制页面在低版本的IE中可能打不开。如果您的IE版本较低，请进行升级。
    FTP Server的配置参数如下：

    -   **ossftp监听地址**：填写需要使用FTP服务的客户端IP。如果是在本机上运行客户端，保持默认即可。
    -   **ossftp监听端口**：ossftp的监听端口，默认即可。
    -   **ossftp日志等级**：ossftp的日志输出等级，根据需求填写。
    -   **Bucket endpoints**：填写Bucket的访问域名（格式为bucket\_name.enpoint），多个域名以英文逗号（,）隔开。
    -   **Language**：选择ossftp的显示语言。
4.  下载[FileZilla客户端](https://filezilla-project.org/?spm=a2c4g.11186623.2.6.bqHidZ)并安装。

5.  配置OSS访问信息后单击**快速连接**。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5224459951/p2520.png)

    **说明：** 同一时间内只能存在一个服务器和一个连接。如果在一个服务器已连接的情况下新建连接，则之前连接会直接断开。

    -   **主机**：配置服务器IP地址，若服务器和客户端在一台设备上，使用默认地址：127.0.0.1。
    -   **用户名**：填写拥有Bucket访问权限的AccessKey ID和Bucket名称，格式为AccessKey ID/bucket\_name，例如`tSxyi******wPMEp/test-hz-jh-002`。关于AccessKey ID的获取，请参见[创建AccessKey]()。
    -   **密码**：填写拥有Bucket访问权限的AccessKey Secret。关于AccessKey Secret的获取，请参见[创建AccessKey]()。
    -   **端口**：填写服务器配置的监听端口，默认填写2048。

