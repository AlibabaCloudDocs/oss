Use CDN to accelerate access to OSS 
========================================================

When you directly access OSS resources, the access speed depends on the region in which the buckets are located and is limited by the downstream bandwidth of OSS. Alibaba Cloud Content Delivery Network (CDN) provides a higher bandwidth. If you use CDN to access OSS resources, CDN caches the resources on CDN nodes closest to your region and distributes the resources to you from the nodes. This way, you can access OSS resources more quickly at lower costs. This topic describes how to use CDN to accelerate access to OSS.

Background information 
-------------------------------------------

The performance of traditional website architectures is bottlenecked as the access to websites grows because dynamic and static resources in these architectures are not separated. The following figure shows a traditional website architecture.

![Traditional website architecture](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4454412161/p132506.png)

You can use an architecture in which dynamic and static resources are separated to resolve the performance problem when the website is accessed by a large number of users. The following figure shows an architecture in which dynamic and static resources are separated.

![Architecture in which dynamic and static resources are separated](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5454412161/p131983.png)

This architecture separates and distributes resources in the following methods:

* Stores dynamic resources such as web applications and databases on ECS instances.

  

* Stores static resources such as images, video and audio files, and static scripts in OSS buckets.

  

* Uses OSS buckets as the origins of CDN and distributes objects cached on the node closest to the region in which the user is located to accelerate data access.

  




This architecture has the following benefits:

* The workload for web servers is reduced.

  OSS resources are cached on and distributed from CDN nodes closest to the regions in which users are located. This way, the transmission distance is minimized and data access is accelerated.
  

* The storage of a large amount of data is supported.

  The capacity of OSS buckets can be elastically expanded. You do not need to upgrade the storage architecture.
  

* Storage fees and traffic fees are reduced.

  In this architecture, storage fees for OSS buckets, downstream traffic fees for CDN, and a small amount of back-to-origin traffic fees are incurred. The storage fees for OSS buckets are 50% cheaper than those for the same capacity of cloud disks. The unit price of CDN traffic is only about 30% to 40% of the unit price of OSS outbound traffic over the Internet.
  




Prerequisites 
----------------------------------

* An OSS bucket is created. Resources are uploaded to the bucket. For more information, see [Upload objects](/intl.en-US/Console User Guide/Upload, download, and manage objects/Upload an object.md).

  

* Alibaba Cloud CDN is activated. For more information, see [Activate Alibaba Cloud CDN](https://www.alibabacloud.com/help/doc-detail/27272.htm).

  




Procedures 
-------------------------------

In the following steps, example.com is used as the domain name and oss.example.com is used as the accelerated domain name. You can specify an actual domain name as the accelerated domain name, such as a primary domain name, second-level domain name, or wildcard domain name.

1. Add a domain name.

   1. Log on to the [DCDN console](https://dcdn.console.aliyun.com/overview). Click **Domain Names** .

      
   
   2. Click **Add Domain Name** . On the page that appears, configure the following parameters:

      * **Domain Name to Accelerate** : Enter the domain name that you want to specify as the accelerated domain name. In this example, **oss.example.com** is used.

        
      
      * **Business Type** : Select **Dynamic Acceleration** .

        
      
      * **Origin Information** : Select **OSS Domain** . Select the domain name of the bucket for which you want to accelerate access, port, and acceleration region.

        
      

      
   
   3. Click **Next** , and then click **Return** .

      
   
   4. Wait until the status of the domain name is **Running** . Copy the CNAME of the domain name, which is **oss.example.com.w.kunluncan.com** in this example.

      
   

   

2. Resolve the domain name.

   1. Log on to the [Domains console](https://dc.console.aliyun.com/next/index). Click **Resolve** in the Actions column corresponding to the domain name **example.com** .

      
   
   2. On the **Add Record** page, configure the following parameters:

      * **Record Type** : Select **CNAME** from the drop-down list.

        
      
      * **Host** : Enter **oss** .

        
      
      * **Value** : Enter the copied CNAME **oss.example.com.w.kunluncan.com** .

        
      
      * Other parameters: Retain the default values.

        
      

      
   
   3. Click **Confirm** . Wait a few minutes, and then run the ping command to check whether the accelerated domain name takes effect. If the results showed in the following figure is returned, the accelerated domain name takes effect.

      ![ping](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1393412161/p132115.png)
      
   

   

3. Enable auto CDN cache update.

   1. Log on to the [OSS console](https://oss.console.aliyun.com/). In the left-side navigation pane, click **Buckets** . On the Buckets page, click the name of the bucket for which you want to accelerate access.

      
   
   2. Choose **Transmission** \> **Domain Names** .

      
   
   3. Turn on **Auto CDN Cache Update** for the accelerated domain name that you add.

      
   

   

4. View the URL of an object.

   1. Log on to the [OSS console](https://oss.console.aliyun.com/). In the left-side navigation pane, click **Buckets** . On the Buckets page, click the name of the bucket in which the object you want to view is stored.

      
   
   2. Click **Files** . Click **View Details** in the Actions column corresponding to the object that you want to view.

      
   
   3. In the **View Details** panel, select the accelerated domain name from the **Custom Domain Name** drop-down list. In this example, select **oss.example.com** . As shown in the following figure, the URL of the object starts with the accelerated domain name.

      ![url](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1393412161/p132112.png)
      
   
   4. Access the object by using the URL and use the developer tools of the browser to view the details. As shown in the following figure, the accelerated domain name takes effect and the object is cached on CDN.

      ![Turtle](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2393412161/p132501.png)
      
   

   

5. Remove the validity period of the URL.

   1. In the **View Details** panel, click **Set ACL** .

      
   
   2. Select **Public Read** . Click **OK** .

      
   

   

6. (Optional) Configure HTTPS certificates to access the object.

   1. In the **View Details** panel of the object, turn on **HTTPS** .

      
   
   2. Log on to the [DCDN console](https://dcdn.console.aliyun.com/overview). Click **Domain Names** . Click the accelerated domain name that you add.

      
   
   3. In the left-side navigation pane, click **HTTPS** . In the **HTTPS Certificate** section, click **Modify** .

      
   
   4. After you complete the settings, you can access the object by using HTTPS. For more information, see [Configure HTTPS certificates](https://www.alibabacloud.com/help/doc-detail/65101.htm).

      
   

   




Purchase resource plans 
--------------------------------------------

To further reduce the cost, click [OSS Resource Plan](https://common-buy-intl.alibabacloud.com/?spm=a3c0i.7950270.1797113820.4.660fab91fID4fo&commodityCode=oss_bag_intl#/buy) and [CDN Resource Plan](https://common-buy-intl.alibabacloud.com/?spm=a3c0i.7958120.7489544720.5.41bd7b74ibSWDs&commodityCode=%20cdn_bag_intl#/buy) to purchase resource plans.
