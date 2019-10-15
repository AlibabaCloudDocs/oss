# 什么是对象存储 OSS {#concept_ybr_fg1_tdb .concept}

阿里云对象存储服务（Object Storage Service，简称 OSS），是阿里云提供的海量、安全、低成本、高可靠的云存储服务。其数据设计持久性不低于 99.9999999999%（12 个 9），服务设计可用性（或业务连续性）不低于99.995%。

OSS 具有与平台无关的RESTful API接口，您可以在任何应用、任何时间、任何地点存储和访问任意类型的数据。

您可以使用阿里云提供的API、SDK 接口或者OSS迁移工具轻松地将海量数据移入或移出阿里云OSS。数据存储到阿里云OSS以后，您可以选择标准存储（Standard）作为移动应用、大型网站、图片分享或热点音视频的主要存储方式，也可以选择成本更低、存储期限更长的低频访问存储（Infrequent Access）和归档存储（Archive）作为不经常访问数据的存储方式。

## 相关概念 {#section_h4j_rlb_n2b .section}

-   存储类型（Storage Class）

    OSS 提供标准、低频访问、归档三种存储类型，全面覆盖从热到冷的各种数据存储场景。其中标准存储类型提供高可靠、高可用、高性能的对象存储服务，能够支持频繁的数据访问；低频访问存储类型适合长期保存不经常访问的数据（平均每月访问频率 1 到 2 次），存储单价低于标准类型；归档存储类型适合需要长期保存（建议半年以上）的归档数据，在三种存储类型中单价最低。详情请参见[存储类型介绍](../../../../cn.zh-CN/开发指南/存储类型/存储类型介绍.md#)。

-   存储空间（Bucket）

    存储空间是您用于存储对象（Object）的容器，所有的对象都必须隶属于某个存储空间。存储空间具有各种配置属性，包括地域、访问权限、存储类型等。您可以根据实际需求，创建不同类型的存储空间来存储不同的数据。创建存储空间请参见[创建存储空间](../../../../cn.zh-CN/开发指南/存储空间（Bucket）/创建存储空间.md#)。

-   对象/文件（Object）

    对象是OSS存储数据的基本单元，也被称为OSS的文件。对象由元信息（Object Meta）、用户数据（Data）和文件名（Key）组成。对象由存储空间内部唯一的 Key 来标识。对象元信息是一组键值对，表示了对象的一些属性，比如最后修改时间、大小等信息，同时您也可以在元信息中存储一些自定义的信息。

-   地域（Region）

    地域表示 OSS 的数据中心所在物理位置。您可以根据费用、请求来源等选择合适的地域创建Bucket。详情请参见[OSS已开通的Region](../../../../cn.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md#)。

-   访问域名（Endpoint）

    Endpoint 表示 OSS 对外服务的访问域名。OSS以HTTP RESTful API的形式对外提供服务，当访问不同地域的时候，需要不同的域名。通过内网和外网访问同一个地域所需要的域名也是不同的。具体的内容请参见[各个Region对应的Endpoint](../../../../cn.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md#)。

-   访问密钥（AccessKey）

    AccessKey（简称 AK）指的是访问身份验证中用到的 AccessKeyId和AccessKeySecret。OSS通过使用AccessKeyId和AccessKeySecret对称加密的方法来验证某个请求的发送者身份。AccessKeyId 用于标识用户；AccessKeySecret是用户用于加密签名字符串和 OSS 用来验证签名字符串的密钥，必须保密。获取 AccessKey 的方法请参见[创建AccessKey](../../../../cn.zh-CN/通用参考/创建AccessKey.md#)。


## 相关服务 {#section_jcv_xlb_n2b .section}

您把数据存储到 OSS 以后，就可以使用阿里云提供的其他产品和服务对其进行相关操作。

以下是您会经常使用到的阿里云产品和服务：

-   图片处理（IMG）：对存储在 OSS 上的图片进行格式转换、缩放、裁剪、旋转、添加水印等各种操作。请参见[快速使用OSS图片处理服务](../../../../cn.zh-CN/数据处理/图片处理指南/快速使用OSS图片服务.md#)。
-   云服务器（ECS）：提供简单高效、处理能力可弹性伸缩的云端计算服务。请参见[ECS 产品详情页面](https://www.aliyun.com/product/ecs)。
-   内容分发网络（CDN）：将源站资源缓存到各区域的边缘节点，供您就近快速获取内容。请参见[CDN产品详情页面](https://www.aliyun.com/product/cdn)。
-   E-MapReduce：构建于 ECS 上的大数据处理的系统解决方案，基于开源的 Apache Hadoop 和 Apache Spark，方便您分析和处理自己的数据。请参见[E-MapReduce产品详情页面](https://www.aliyun.com/product/emapreduce)。
-   媒体处理：将存储于 OSS 的音视频转码成适合在 PC、TV 以及移动终端上播放的格式。并基于海量数据深度学习，对音视频的内容、文字、语音、场景多模态分析，实现智能审核、内容理解、智能编辑。请参见[媒体处理产品详情页面](https://www.aliyun.com/product/mts)。

## 管理 OSS {#section_o5k_1mb_n2b .section}

-   通过 OSS 控制台管理 OSS

    OSS 提供了 Web 服务页面，您可以登录[OSS 管理控制台](https://oss.console.aliyun.com/overview)，管理您的 OSS。详情请参见[控制台用户指南](../../../../cn.zh-CN/控制台用户指南/登录OSS管理控制台/使用阿里云账号登录OSS管理控制台.md#)。

-   通过 API 或 SDK 管理 OSS

    OSS 提供 RESTful API 和各种语言的 SDK 开发包，方便您快速进行二次开发。详情请参见[OSS API参考](../../../../cn.zh-CN/API 参考/API概览.md#)和 [OSS SDK 参考](OSS SDK 参考../DNOSS11814329/ZH-CN_TP_22258_V3.dita#concept_dcn_tp1_kfb)。

-   通过工具管理 OSS

    OSS 提供各类型的管理工具，您可以通过工具管理您的 OSS。详情请参见[OSS常用工具](../../../../cn.zh-CN/常用工具/OSS常用工具汇总.md#)。


## OSS 定价 {#section_sv5_nmb_n2b .section}

传统的存储服务供应商会要求您购买预定量的存储和网络传输容量，如果超出此容量，就会关闭对应的服务或者收取高昂的超容量费用；如果没有超过此容量，又需要您按照全部容量支付费用。

OSS 仅按照您的实际使用容量收费，您无需预先购买存储和流量容量，随着您业务的发展，您将享受到更多的基础设施成本优势。

关于 OSS 的价格，请参见[OSS 产品定价](https://www.aliyun.com/price/product#/oss/detail)。关于 OSS 的计量计费方式，请参见[计量项和计费项](../../../../cn.zh-CN/计量计费/计量项和计费项.md#)。

## 学习路径图 {#section_bxq_qdc_n2b .section}

您可以通过[OSS产品学习路径图](https://help.aliyun.com/learn/learningpath/oss.html)快速了解OSS，学习相关的基础操作，并利用丰富的 API、SDK 包和便捷工具进行二次开发。

## 视频 {#section_rmr_xzl_42b .section}

观看以下视频，快速了解OSS。  

## 常见问题 {#section_h5n_3lq_mow .section}

[对象存储OSS常见问题](cn.zh-CN/产品简介/常见问题.md#)列举了用户经常咨询的一些问题，帮助您进一步了解OSS。

## 文档用户群 {#section_cyd_asp_q7x .section}

您对阿里云文档的任何问题、需求、建议和吐槽，欢迎通过添加钉钉群号或扫描二维码入群讨论和交流。

-   钉钉群号：23331544
-   钉钉群二维码：

    ![OSS 钉钉群](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4313/156872947150213_zh-CN.png)


