# OSS高防

OSS高防是OSS结合DDoS高防推出的DDoS攻击代理防护服务。当受保护的存储空间（Bucket）遭受大流量攻击时，OSS高防会将攻击流量牵引至高防集群进行清洗，并将正常访问流量回源到目标Bucket，确保业务的正常进行。

**说明：** OSS高防已在华东1（杭州）、华东2（上海）、华北1（青岛）、华北2（北京）、华南1（深圳）地域公测，请联系[技术支持](https://selfservice.console.aliyun.com/ticket/createIndex)申请试用。

## 适用场景

DDoS攻击是近年来对企业业务危害最大的攻击手段之一。当企业遭受DDoS攻击时，可能会导致业务中断，进而导致企业的形象受损、客户流失、收益受损等，严重影响企业业务的正常运营。

为此，OSS深度结合[DDoS高防](/cn.zh-CN/阿里云DDoS防护产品介绍/什么是DDoS高防（新BGP&国际）.md)产品，提供最高T级DDoS防护能力、百万QPS防护、秒级攻击切换能力，可有效抵御SYN Flood、ACK Flood、ICMP Flood、UDP Flood、NTP Flood 、SSDP Flood、DNS Flood、HTTP Flood等攻击。OSS高防适用于业务经常遭恶意攻击、勒索、刷单、刷流量等安全防护场景。

## 防护原理

OSS高防的防护原理如下图所示：

![OSS高防](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7717039061/p169031.png)

OSS默认使用[DDoS原生防护](/cn.zh-CN/阿里云DDoS防护产品介绍/DDoS原生防护/什么是DDoS原生防护.md)保护您的Bucket。但是，当攻击频率超出DDoS原生防护的防御阈值时，原生防护将无法提供有效防护，可能会出现Bucket访问异常的情况。

您开通了OSS高防后，当攻击频率超出DDoS原生防护的防御阈值时，OSS会自动将受攻击Bucket的所有访问流量牵引至高防集群。恶意攻击流量将在高防流量清洗中心进行清洗过滤，正常访问流量通过端口协议转发的方式返回给目标Bucket，保证Bucket在受攻击时的稳定访问。

当攻击结束后，受攻击Bucket会切回至DDoS原生防护进行保护。

## 使用限制

-   高防OSS实例创建后需至少使用7天，若实例在7天内被删除，OSS会收取剩余时间的高防基础资源费用。
-   每个地域可以创建一个高防OSS实例，每个实例最多只能绑定同一地域下的10个Bucket。
-   OSS高防仅防护已绑定自定义域名的Bucket。如果您已购买DDoS高防产品，且已配置同名或泛域名的转发规则，则需要前往[DDoS高防控制台](https://yundun.console.aliyun.com/?p=ddoscoo)解绑该域名。否则，在该Bucket受到攻击时，无法通过该自定义域名正常访问OSS。

    为Bucket绑定自定义域名的具体步骤，请参见[绑定自定义域名](/cn.zh-CN/控制台用户指南/存储空间管理/传输管理/绑定自定义域名.md)。

    配置同名或泛域名转发规则的具体步骤，请参见[添加网站](/cn.zh-CN/DDoS高防（新BGP&国际）用户指南/接入DDoS高防/域名接入/添加网站.md)。


## 配置方式

您只需在OSS管理控制台进行简单的配置即可开启Bucket的高防保护。具体配置方式，请参见[配置OSS高防](/cn.zh-CN/控制台用户指南/配置OSS高防.md)。

