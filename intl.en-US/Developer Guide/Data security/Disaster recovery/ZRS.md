# ZRS

OSS uses the multi-zone mechanism to distribute user data across three zones within the same region. Even if one zone becomes unavailable, the data can still be accessed. The zone-redundant storage \(ZRS\) feature can provide data durability \(designed for\) of 99.9999999999% \(twelve 9's\) and service availability of 99.995%.

ZRS offers data center-level disaster recovery capabilities. When a data center is unavailable due to network disconnections, power outages, or other disaster events, OSS can continue to provide highly consistent services. This way, services are not interrupted and data is not lost during failover. This meets the strict requirements of key business systems that the recovery time objective \(RTO\) and the recovery point objective \(RPO\) must be zero.

## Implementation modes

You can configure ZRS for a bucket only when you create the bucket. For an existing bucket, you can use migration tools such as [ossimport](/intl.en-US/Tools/ossimport/Architectures and configurations.md) and [Data Transport]() to migrate data from the existing bucket to another bucket that has ZRS enabled.

You can use the following methods to enable ZRS when you create a bucket.

|Implementation mode|Description|
|-------------------|-----------|
|[Console](/intl.en-US/Console User Guide/Manage buckets/Create buckets.md)|A user-friendly and intuitive web application|
|[ossbrowser](/intl.en-US/Tools/ossbrowser/Quick start.md)|An easy-to-operate graphical tool|
|[ossutil](/intl.en-US/Tools/ossutil/Common commands/mb.md)|A high-performance command-line tool|
|[Java SDK](/intl.en-US/SDK Reference/Java/Buckets/Create buckets.md)|SDK demos in a variety of languages|
|[Python SDK](/intl.en-US/SDK Reference/Python/Buckets/Create a bucket.md)|
|[Go SDK](/intl.en-US/SDK Reference/Go/Buckets/Create buckets.md)|
|[C++ SDK](/intl.en-US/SDK Reference/C++/Buckets/Create buckets.md)|
|[Node.js SDK](/intl.en-US/SDK Reference/Node. js/Buckets/Create buckets.md)|

## Usage notes

-   Supported regions

    ZRS is available in the following regions: Singapore, China \(Shenzhen\), China \(Beijing\), China \(Hangzhou\), and China \(Shanghai\). This feature will be available in other regions.

-   Billing description

    ZRS incurs extra costs. For more information, see[Object Storage Service Pricing](https://www.alibabacloud.com/zh/product/oss/pricing).


## Supported storage classes

ZRS supports the Standard and Infrequent Access \(IA\) storage classes. The following table compares the two storage classes from different dimensions.

|Item|Standard|IA|
|:---|:-------|:-|
|Data durability \(designed for\)|99.9999999999% \(twelve 9's\)|99.9999999999% \(twelve 9's\)|
|Service availability|99.995%|99.50%|
|Minimum billable size of objects|N/A|64 KB|
|Minimum storage period|N/A|30 days|
|Data retrieval fees|N/A|Based on the size of retrieved data. Unit: GB.|
|Data access|Real-time access with low latency \(within milliseconds\)|Real-time access with low latency \(within milliseconds\)|
|IMG|Supported|Supported|

