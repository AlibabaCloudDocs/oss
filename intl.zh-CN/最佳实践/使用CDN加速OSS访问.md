使用CDN加速OSS访问 
=================================

用户直接访问OSS资源，访问速度会受到OSS的下行带宽以及Bucket地域的限制。如果通过CDN来访问OSS资源，带宽上限更高，并且可以将OSS的资源缓存至就近的CDN节点，通过CDN节点进行分发，访问速度更快，且费用更低。本文介绍如何使用CDN来加速OSS的访问。

背景信息 
-------------------------

传统网站架构下，动态资源和静态资源不分离，随着访问量的增长，性能会成为瓶颈，如下图所示：

![传统](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0997298951/p132506.png)

如果采用动静分离的网站架构，就能够解决海量用户访问的性能瓶颈问题，如下图所示：

![动静分离](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0997298951/p131983.png)

该架构的要点如下：

* 将动态资源如Web程序、数据库等存放在云服务器ECS上。

  

* 将静态资源如图片、音视频、静态脚本等存放在对象存储OSS上。

  

* 将OSS作为CDN的源站，通过CDN加速分发，使用户通过CDN节点就近获得文件。

  




该架构有以下优势：

* 降低了Web服务器负载。

  OSS的资源缓存至就近的CDN节点，通过CDN节点进行分发，缩短了网络传输距离，加快了用户的调用速度。
  

* 支持海量存储。

  OSS的存储空间弹性无限扩展，您无需考虑存储架构升级。
  

* 降低了存储费用和流量费用。

  使用该架构会产生OSS的存储费用、CDN的下行流量费用，以及极少量的回源流量费用。其中OSS的存储费用仅为ECS云盘费用的一半，而CDN流量的单价约为OSS外网流量单价的30%～40%。
  




前提条件 
-------------------------

* 已创建一个OSS Bucket，且上传了相关资源。详情请参见[上传文件](/intl.zh-CN/控制台用户指南/上传、下载和管理文件/上传文件.md)。

  

* 已开通阿里云CDN服务。详情请参见[开通CDN服务](https://www.alibabacloud.com/help/doc-detail/27272.htm)。

  




操作步骤 
-------------------------

以下步骤以域名example.com为例，加速域名以oss.example.com为例。您可以根据自己的实际情况来选择加速域名，包括主域名、二级域名、泛域名等。

1. 添加域名。

   1. 登录[全站加速控制台](https://dcdn.console.aliyun.com/overview)，选择 **域名管理** 。

      
   
   2. 单击 **添加域名** ，设置以下参数：

      * **加速域名** ：输入加速域名，该示例为 **oss.example.com** 。

        
      
      * **业务类型** ：选择 **全站加速** 。

        
      
      * **加速区域** ：选择 **仅中国内地** 。

        
      
      * **源站信息** ：单击 **新增源站信息** ，然后选择 **OSS域名** 和需要加速的OSS域名（即之前创建的OSS Bucket对应的域名），其他参数保持默认值。单击 **确认** 。

        
      

      
   
   3. 单击 **下一步** ，然后单击 **返回域名列表** 。

      
   
   4. 等到域名状态为 **正常运行** 时，复制CNAME值，该示例为 **oss.example.com.w.kunluncan.com** 。

      
   

   

2. 解析域名。

   1. 进入[域名控制台](https://dc.console.aliyun.com/next/index)，找到域名 **example.com** ，单击 **解析** 。

      
   
   2. 在 **添加记录** 页面，配置以下参数：

      * **记录类型** ：选择 **CNAME** 。

        
      
      * **主机记录** ：输入 **oss** 。

        
      
      * **记录值** ：输入之前复制的CNAME值 **oss.example.com.w.kunluncan.com** 。

        
      
      * 其他参数：保留默认值。

        
      

      
   
   3. 单击 **确定** 。等待几分钟后，使用ping命令查看加速域名是否生效。下图表示已生效。

      ![ping](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0997298951/p132115.png)
      
   

   

3. 开启CDN缓存自动刷新。

   1. 进入[OSS控制台](https://oss.console.aliyun.com/)，单击左侧导航栏的 **Bucket列表** ，然后选择对应的Bucket。

      
   
   2. 选择 **传输管理** ，然后选择 **域名管理** 。

      
   
   3. 开启加速域名对应的 **CDN缓存自动刷新** 。

      
   

   

4. 查看文件的URL。

   1. 进入[OSS控制台](https://oss.console.aliyun.com/)，单击左侧导航栏的 **Bucket列表** ，然后选择对应的Bucket。

      
   
   2. 进入 **文件管理** ，然后单击文件对应的 **详情** ，进入文件的 **详情** 页面。

      
   
   3. 在文件的 **详情** 页面，从 **自有域名** 列表中选择加速域名，该示例为 **oss.example.com** 。可以看到文件的URL已经变为加速域名开头的URL。

      ![url](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0997298951/p132112.png)
      
   
   4. 直接访问上述的URL，通过开发者工具检查可以发现，CDN已经生效并成功缓存了这张图片。

      ![海龟](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1997298951/p132501.png)
      
   

   

5. 使文件的URL长期有效。

   1. 在文件的 **详情** 页面，单击 **设置读写权限** 。

      
   
   2. 选择 **公共读** ，然后单击 **确定** 。

      
   

   

6. （可选）配置证书加密访问。

   1. 在文件的 **详情** 页面，开启 **使用HTTPS** 。

      
   
   2. 在[全站加速控制台](https://dcdn.console.aliyun.com/overview)，选择 **域名管理** ，然后单击加速域名。

      
   
   3. 在左侧导航栏，单击 **HTTPS配置** ，然后在 **HTTPS证书** 区域单击 **修改配置** 。

      
   
   4. 完成配置后即可通过HTTPS加密访问，具体步骤请参见[配置HTTPS证书](https://www.alibabacloud.com/help/doc-detail/65101.htm)。

      
   

   




购买链接 
-------------------------

要进一步降低费用，请单击[OSS资源套餐包](https://common-buy-intl.alibabacloud.com/?spm=a3c0i.7950270.1797113820.4.660fab91fID4fo&commodityCode=oss_bag_intl#/buy)和[CDN资源套餐包](https://common-buy-intl.alibabacloud.com/?spm=a3c0i.7958120.7489544720.5.41bd7b74ibSWDs&commodityCode=%20cdn_bag_intl#/buy)购买相关折扣套餐。
