# 文件存储CPFS

文件存储CPFS（Cloud Paralleled File System）是一种并行文件系统。CPFS的数据存储在集群中的多个数据节点，并可由多个客户端同时访问，从而能够为大型高性能计算机集群提供高IOPS、高吞吐、低时延的数据存储服务。

## 适用场景

[文件存储CPFS](https://nas.console.aliyun.com/?spm=5176.cnnas_cpfs.0.0.3fa3c4feKZI3gy#/cpfs/list)针对高性能计算场景的性能要求进行了深度优化，提供对数据毫秒级的访问和高聚合IO、高IOPS的数据读写请求，可以用于AI深度训练、自动驾驶、基因计算、EDA仿真、石油勘探，气象分析、机器学习、大数据分析以及影视渲染等业务场景中。

## 性能

文件存储CPFS可以提供数百GB的带宽，数百万的IOPS以及亚毫秒级的延时。具体的带宽和IOPS与您购买的文件系统规模有关。

|容量|带宽|标准型（写IOPS）|标准型（读IOPS）|高级型（写IOPS）|高级型（读IOPS）|
|--|--|----------|----------|----------|----------|
|2 TB|1 GB/s|20 k|40 k|50 k|100 k|
|5 TB|1 GB/s|20 k|40 k|50 k|100 k|
|2.5 GB/s|60 k|150 k|100 k|350 k|
|10 TB|1.5 GB/s|40 k|100 k|80 k|200 k|
|2.5 GB/s|60 k|150 k|100 k|350 k|
|30 TB|2.5 GB/s|60 k|150 k|100 k|350 k|
|5 GB/s|80 k|200 k|140 k|500 k|
|50 TB|2.5 GB/s|60 k|150 k|100 k|350 k|
|7.5GB/s|100 k|300 k|200 k|600 k|

您可以使用FIO工具进行吞吐和IOPS的性能测试，并通过分片配置提高聚合带宽和IOPS，详情请参见[性能测试]()和[性能调优]()。

## 数据持久性和服务可用性

文件存储CPFS的数据持久化存储于阿里云自研的盘古分布式存储系统，支持多份数据拷贝，可以提供99.999999999%（11个9）的数据可靠性。

文件存储CPFS的所有节点均为高可用设计。实现集群内秒级别的故障检测，并由CPFS集群调度器自动将服务切换到其他节点，同时兼顾负载均衡。整个切换过程用户不感知，提供远高于传统两节点的高可用性。

## 扩展性和弹性

文件存储CPFS支持在线扩容。由于所有数据均以条带化的方式存储并且支持扩容以后的自动负载平衡，可满足性能的线性增长，并且即时利用扩容节点的吞吐和存储能力，满足业务增长需要的更多容量与性能诉求。

## 安全性

文件存储CPFS支持配置企业自建的LDAP（Lightweight Directory Access Protocol）服务，来控制CPFS文件系统的用户访问。不接入LDAP时，CPFS只允许root用户访问文件系统，其他用户访问时将返回permission denied错误。接入LDAP时，您需要提供LDAP服务器并确保LDAP服务的连通性。

## 接口

CPFS API支持HTTP或HTTPS协议进行请求通信，支持GET方法发送请求。如果您熟悉网络服务协议和一种以上编程语言，推荐您调用API管理您的文件存储CPFS，对文件系统进行创建、挂载、管理等操作，以及LDAP用户管理。

如果您更习惯使用图形化的Web应用程序，可以使用管理控制台来管理CPFS文件系统。

## 费用模型

文件存储CPFS的计费项包括存储容量和带宽。开通产品时默认按照实际使用量按小时计费（按量付费），同时也支持购买资源包（包年包月）的方式提前购买资源的使用额度和时长，获取更多的优惠。详情请参见[计量项和计费项]()。

