# Tablestore

Tablestore is a data storage service developed by Alibaba Cloud to store a large amount of structured data. You can use Tablestore to efficiently query and analyze data. Tablestore provides the Wide Column model, Timeline model, and Timestream model that are compatible with HBase. It can store petabytes of data and provide tens of millions of transactions per second \(TPS\) and milliseconds of latency.

## Scenarios

[Tablestore](https://ots.console.aliyun.com/?spm=5176.54465.905680.btn5.bb556184XseDFD#/) can store petabytes of data in a single table. In addition, it supports tens of millions of query per second \(QPS\) and a variety of query methods, including global secondary index, full-text search, inverted index, and spatio-temporal index. Therefore, Tablestore is widely used in scenarios of structured data, such as social networks, Internet of Things \(IoT\), artificial intelligence \(AI\), metadata, and big data.

-   Metadata

    When you store a large amount of data such as documents and media files, it is important to store and analyze the metadata of the stored data. In scenarios where e-commerce orders, transaction histories of bank accounts, and phone bills from service providers are stored, you also need to store and analyze a large amount of metadata. Tablestore can help you efficiently manage the metadata of your data.

-   Message data

    Tablestore provides the Timeline model to store message data. This model constructs lightweight message queues that support a large number of topics to store a large amount of information in social network applications, such as instant messaging \(IM\) chats and Feed streams information such as comments, posts, and likes. The Timeline model is already used in a variety of IM systems, such as DingTalk, to support the synchronization of a large number of chat messages.

-   Trajectory tracing

    Tablestore provides the Timestream model to help you manage and analyze trajectory data in different scenarios, such as running, riding, walking, and food delivery.

-   Scientific big data

    Gridded data is a type of scientific big data used in geoscience fields such as meteorology, oceanography, geology, or geomorphology. Gridded data is widely used and the data volumes are growing. Scientists need to quickly browse data and query data online by using various methods, which requires a low latency. Tablestore can meet the high requirements on storage capacity and query performance in scientific big data scenarios.

-   Internet big data

    Product designers of e-commerce and information platforms on the Internet must collect and analyze the data of different platforms to determine the further development of their products. The public relations and marketing departments of enterprises also needs to handle issues in a timely manner based on public opinions. Tablestore can help you store and analyze tens of billions of public opinions.

-   IoT

    Tablestore can be used to store time series data from IoT devices and monitoring systems. It provides API operations to directly read SQL data and incremental data streams, which allow you to implement offline data analysis and real-time stream computing.


## Performance

Tablestore can store tens of petabytes of data and trillions of records in a single table and provide tens of millions of transactions per second \(TPS\) and milliseconds of latency. It supports automatic load balancing and hotspot migration without manual operations and maintenance \(O&M\). In addition, Tablestore can provide high throughput for write operations and stable read and write performance that can be predicted. For more information, see the [Tablestore performance white paper](/intl.en-US/Performance White Paper/Test environment.md).

## Data durability and service availability

Tablestore creates multiple backups of data and stores them in different servers across racks. When a backup fails, Tablestore immediately uses another backup to restore the data. This mechanism ensures data durability of 99.99% and service availability of 99.999999999% \(eleven 9's\).

## Scalability and elasticity

Tablestore uses shards and load balancing to implement seamless scalability. Tablestore adjusts the size of partitions to store more data in a table when the data stored in the table increases. Tablestore can store a minimum of 10 PB of data. A single table in Tablestore can store a minimum of 1 PB of data or one trillion records.

## Security

Tablestore performs authentication and authorization based on tables and operations. It also supports STS temporary authorization, custom authentication, and RAM users to isolate resources by user. For more information, see [RAM and STS](/intl.en-US/Function Introduction/Authorization management/RAM and STS.md). Tablestore supports access from the Internet, ECS instances, and VPCs and provides network access control.

## Operations

Tablestore provides standard RESTful API operations. You can use tools or Tablestore SDKs that encapsulate the API operations to develop applications. Tablestore provides SDKs for [various programming languages](/intl.en-US/SDK Reference/Introduction.md), including Java, Python, PHP, and Go. [TablestoreCli](/intl.en-US/Utilities/Tablestore CLI.md) is a command-line interface \(CLI\) tool that provides simple and clear commands to perform a variety of operations in Tablestore, including table operations, single-row operations, simple stress testing operations, and data backup operations. This tool supports Windows, Linux, and Mac operating systems.

The Tablestore console allows you to create instances, tables, and search indexes, perform basic read and write operations on data, and monitor the access data, such as QPS, latency, and number of requests, of instances and tables.

## Cost model

You can use the pay-as-you-go billing method to pay only for the Tablestore resources that you use. This way, you can handle traffic fluctuations and high concurrency requests that require low latency at low costs. You can also use the subscription billing method to purchase resource plans in advance to deduct fees incurred by resource usage. Tablestore is a pay-as-you-go service that is billed based on the following items: storage usage, read throughput, write throughput, and Internet outbound traffic. If you use the search index or global secondary index feature, additional fees are incurred. For more information, see [Billing overview](/intl.en-US/Pricing/Billing overview.md).

