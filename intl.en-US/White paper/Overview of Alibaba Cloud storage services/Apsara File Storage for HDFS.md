# Apsara File Storage for HDFS

Apsara File Storage for HDFS is a file storage service for computing resources such as Alibaba Cloud ECS instances and Container Service. You can manage and access data in Apsara File storage for HDFS in the same way as you do in Hadoop distributed file systems. You can use Apsara File Storage for HDFS without modifying your big data analytics applications. Apsara File Storage for HDFS provides unlimited capacity, performance scaling, unique namespace, multi-party sharing, high reliability, and high availability.

## Scenarios

[Apsara file storage for HDFS](https://common-buy.aliyun.com/?spm=5176.cnalidfs.0.0.26ff6948V1RRyZ&commodityCode=dfsstd#/open) is suitable for scenarios where high throughput is required, such as big data analytics and machine learning. Apsara File Storage for HDFS supports high-throughput and low-latency access. You do not need to migrate data to local computing resources.

After data is stored in Apsara File Storage for HDFS, ECS instances or other computing resources can directly access the data. You can deploy Hadoop or other machine learning applications on multiple computing resources so that applications can access data in Apsara File Storage for HDFS by using the Hadoopfs operation to perform online or offline computing. You can also export the computing results to Apsara File Storage for HDFS to permanently store the results.

## Performance

The performance of Apsara file storage for HDFS is measured by throughput. The practical throughput of an Apsara File Storage for HDFS file system cannot exceed the maximum bandwidth of the ECS instance to which the file system is attached. For example, if the bandwidth of the ECS instance is 1.5 Gbit/s, the throughput of the file system can reach a maximum of 187.5 Mbit/s. The throughput of an Apsara file storage for HDFS file system depends on the capacity of the file system.

## Data durability and service availability

Like Apsara File Storage NAS, Apsara File Storage for HDFS provides multiple replicas for each piece of data that is stored in a file system. These replicas reside in devices that are isolated across different fault domains for geo-redundancy. Apsara File Storage for HDFS provides data reliability of 99.999999999% \(eleven 9's\). This reduces a large number of data security risks.

## Scalability and elasticity

Apsara File Storage for HDFS provides your applications with optimal storage performance, including high throughput, high IOPS, and low latency. Additionally, the linear relationship between performance and capacity can meet your requirements on capacity and storage performance when your business grows.

## Security

Apsara File Storage for HDFS uses multiple security mechanisms to guarantee the security of data stored in file systems. These security mechanisms include network isolation based on VPCs, user isolation in classic networks, standard permission control for file systems, access control based on security groups, and RAM user authorization.

## Operations

Apsara File Storage for HDFS provides SDKs for file systems and management systems. Only SDKs for file systems are provided during the public preview. You can perform management operations in the Apsara File Storage for HDFS console. Apsara File Storage for HDFS SDK for Java implements operations based on Hadoop distributed file systems to provide Hadoop-compatible file systems. The SDK is provided as a JAR file whose name is in the following format: aliyun-sdk-dfs-x.y.z.jar. Computing and analysis applications based on Apache Hadoop, such as MapReduce, Hive, Spark, and Flink, can use the SDK to use Apsara File Storage for HDFS as the default file system without modifying and compiling code for better features and performance than the original HDFS.

If you prefer graphical web applications, you can use the console to manage Apsara File Storage for HDFS file systems.

## Cost model

Apsara File Storage for HDFS is billed based on the capacity and preset throughput of the file system. By default, Apsara File Storage for HDFS is billed based on the used resources on an hourly basis \(pay-as-you-go\). You can also purchase resource plans \(subscription\) in advance to obtain additional discounts. For more information, see [Pricing](https://www.aliyun.com/price/product?spm=5176.8030368.1058477.32.133d3aa4UEi2fc#/alidfs/detail).

