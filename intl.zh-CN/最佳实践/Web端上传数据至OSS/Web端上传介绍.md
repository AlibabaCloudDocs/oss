# Web端上传介绍

本教程介绍如何利用Web前端技术将文件上传到OSS。

## 背景信息

每个OSS的用户都会用到上传服务。Web端常见的上传方法是用户在浏览器或App端上传文件到应用服务器，应用服务器再把文件上传到OSS。具体流程如下图所示。

![时序图](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7354449951/p140018.png)

和数据直传到OSS相比，以上方法有三个缺点：

-   上传慢：用户数据需先上传到应用服务器，之后再上传到OSS。网络传输时间比直传到OSS多一倍。如果用户数据不通过应用服务器中转，而是直传到OSS，速度将大大提升。而且OSS采用BGP带宽，能保证各地各运营商之间的传输速度。
-   扩展性差：如果后续用户多了，应用服务器会成为瓶颈。
-   费用高：需要准备多台应用服务器。由于OSS上传流量是免费的，如果数据直传到OSS，不通过应用服务器，那么将能省下几台应用服务器。

## 技术方案

目前通过Web前端技术上传文件到OSS，有三种技术方案：

-   利用OSS Browser.js SDK将文件上传到OSS

    该方案通过OSS Browser.js SDK直传数据到OSS，详细的SDK Demo请参见[上传文件](/intl.zh-CN/SDK 示例/Browser.js/上传文件.md)。在网络条件不好的状况下可以通过断点续传的方式上传大文件。该方案在个别浏览器上有兼容性问题，目前兼容IE10及以上版本浏览器，主流版本的Edge、Chrome、Firefox、Safari浏览器，以及大部分的Android、iOS、WindowsPhone手机上的浏览器。更多信息请参见[安装Browser.js SDK](/intl.zh-CN/SDK 示例/Browser.js/安装.md)。

-   使用表单上传方式，将文件上传到OSS

    利用OSS提供的[PostObject接口](/intl.zh-CN/API 参考/关于Object操作/基础操作/PostObject.md)，使用表单上传方式将文件上传到OSS。该方案兼容大部分浏览器，但在网络状况不好的时候，如果单个文件上传失败，只能重试上传。操作方法请参见[PostObject上传方案](/intl.zh-CN/最佳实践/Web端上传数据至OSS/Web端PostObject直传实践/Web端PostObject直传实践简介.md)。


