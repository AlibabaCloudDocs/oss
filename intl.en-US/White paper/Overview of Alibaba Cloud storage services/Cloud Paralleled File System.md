# Cloud Paralleled File System

Cloud Paralleled File System \(CPFS\) is a parallel file system provided by Alibaba Cloud. CPFS stores data across multiple data nodes in a cluster and allows data to be simultaneously accessed by multiple clients. Therefore, CPFS can provide data storage services with high input/output operations per second \(IOPS\), high throughput, and low latency for large-sized, high-performance computing clusters.

## Scenarios

[CPFS](https://nas.console.aliyun.com/?spm=5176.cnnas_cpfs.0.0.3fa3c4feKZI3gy#/cpfs/list) is optimized for high-performance computing scenarios, and enables users to access data in milliseconds and read and write data with highly aggregated I/O and high IOPS. It can be used in a variety of scenarios such as AI deep training, autonomous driving, genomics computing, EDA simulation, petroleum exploration, meteorological analysis, machine learning, big data analytics, and film rendering.

## Performance

CPFS can provide a bandwidth of hundreds of Gbit/s, an IOPS of millions, and sub-millisecond level latency. The bandwidth and IOPS vary with the specification of the file system that you purchase.

|Capacity|Bandwidth|Write IOPS of Standard Edition|Read IOPS of Standard Edition|Write IOPS of Advanced Edition|Read IOPS of Advanced Edition|
|--------|---------|------------------------------|-----------------------------|------------------------------|-----------------------------|
|2 TB|1 Gbit/s|20,000|40 k|50 k|100 k|
|5 TB|1 Gbit/s|20,000|40,000|50,000|100,000|
|2.5 Gbit/s|60,000|150,000|100,000|350,000|
|10 TB|1.5 Gbit/s|40,000|100,000|80,000|200,000|
|2.5 Gbit/s|60,000|150,000|100,000|350,000|
|30 TB|2.5 Gbit/s|60,000|150,000|100,000|350,000|
|5 Gbit/s|80,000|200,000|140,000|500,000|
|50 TB|2.5 Gbit/s|60,000|150,000|100,000|350,000|
|7.5 Gbit/s|100,000|300,000|200,000|600,000|

You can use the FIO tool to test the throughput and IOPS of a CPFS file system, and configure the number and size of stripes to improve the aggregated bandwidth and IOPS of the file system. For more information, see [Performance]() and [Performance optimization]().

## Data durability and service availability

CPFS persistently stores data in the Apsara Distributed File System developed by Alibaba Cloud. CPFS supports multiple data copies and provides 99.999999999% \(eleven 9's\) data reliability.

All nodes in CPFS are designed in the highly available mode. This allows CPFS to detect faults in the cluster within a few seconds and use the cluster scheduler to automatically switch services to other nodes with load balancing at the same time. The failover process is imperceptible to users. Therefore, CPFS provides much higher availability than the traditional two-node mode.

## Scalability and elasticity

CPFS supports online scale-out. CPFS stores all data as stripes and supports automatic load balancing after scale-out. This allows you to improve the performance of file systems linearly and use the throughput and storage capabilities of added nodes immediately to meet the growing requirements on capacity and performance.

## Security

CPFS allows you to control access to file systems by using an enterprise-created Lightweight Directory Access Protocol \(LDAP\) service. If LDAP is not integrated, CPFS allows only the root user to access file systems. If other users attempt to access the file systems, the permission denied error is returned. If you use LDAP to access CFPS, you must specify the parameters of the LDAP server and make sure that the LDAP service is available.

## Operations

CPFS provides API operations that you can use to send GET requests by using HTTP or HTTPS. If you are familiar with network protocols and one or more programming languages, we recommend that you call API operations provided by CPFS to create, mount, and manage file systems and manage LDAP users.

If you prefer graphical web application, you can use the console to manage CPFS file systems.

## Cost model

CPFS are billed based on the storage capacity and bandwidth of file systems. By default, CPFS is billed based on the used resources on an hourly basis \(pay-as-you-go\). You can also purchase resource plans \(subscription\) in advance to obtain additional discounts. For more information, see [Billing Methods]().

