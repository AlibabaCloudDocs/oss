# Overview

OSS provides the following storage classes to cover a variety of data storage scenarios from hot data storage to cold data storage: Standard, Infrequent Access \(IA\), Archive, and Cold Archive.

**Note:** For more information about the pricing of each storage class, see[Object Storage Service Pricing](https://www.alibabacloud.com/product/oss/pricing). For more information about the billing method for each storage class, see [Storage fees](/intl.en-US/Pricing/Billing items and methods/Storage fees.md).

## Standard

Standard provides high reliability, high availability, and high-performance object storage services that can handle frequent data access. Standard storage is suitable for storing images for social networking and sharing applications and data for audio and video applications, large websites, and big data analytics. Standard storage supports the following data redundancy storage mechanisms:

-   Standard locally redundant storage \(LRS\)

    If you use Standard LRS, OSS stores the copies of each object on multiple devices of different facilities in the same zone. This way, OSS ensures data durability and availability even if hardware failures occur.

-   Standard zone-redundant storage \(ZRS\)

    Standard ZRS uses the multi-zone mechanism to distribute user data across three zones within the same region. Even if one zone becomes unavailable, the data is still accessible.


## IA

OSS provides high-durability storage services for IA objects at prices lower than Standard. Objects of the IA storage class have a minimum storage period of 30 days and a minimum billable size of 64 KB. You can access objects of the IA storage class in a real-time manner. However, data retrieval fees are incurred. IA storage applies to scenarios where stored data is infrequently accessed \(once or twice each month\). IA storage supports the following data redundancy storage mechanisms:

-   IA LRS

    If you use IA LRS, OSS stores the copies of each object on multiple devices of different facilities in the same zone. This way, OSS ensures data durability and availability even if hardware failures occur.

-   IA ZRS

    IA ZRS uses the multi-zone mechanism to distribute user data across three zones within the same region. Even if one zone becomes unavailable, the data can still be accessed.


## Archive

OSS provides high-durability storage services for Archive objects at prices lower than Standard and IA. Objects of the Archive storage class have a minimum storage period of 60 days and a minimum billable size of 64 KB. Object of the Archive storage class must be restored before they can be accessed. The restoration takes about one minute, and you are charged for the data retrieval. Archive storage is suitable for data that needs to be stored for a long period, such as archival data, medical images, scientific materials, and video footage.

## Cold Archive

OSS provides high-durability storage services for Cold Archive objects at prices lower than Standard, IA, and Archive. Objects of the Cold Archive storage class have a minimum storage period of 180 days and a minimum billable size of 64 KB. Objects of the Cold Archive storage class must be restored before they can be accessed. The time required to restore an Cold Archive object depends on the object size and the restore mode. You are charged for the data retrieval when you restore a Cold Archive object. Cold Archive storage is suitable for storing extremely cold data for an ultra-long period of time. Such data includes data that must be retained for an extended period of time due to compliance requirements, raw data that is accumulated over an extended period of time in the big data and AI fields, retained media resources in the film and television industries, and archived videos from the online education industry.

**Note:** Cold Archive is in public preview in the following regions: China \(Beijing\), China \(Zhangjiakou\), China \(Hangzhou\), China \(Shanghai\), China \(Shenzhen\), China \(Chengdu\), Australia \(Sydney\), Singapore, US \(Silicon Valley\), Germany \(Frankfurt\), Malaysia \(Kuala Lumpur\), Indonesia \(Jakarta\), India \(Mumbai\), and China \(Hong Kong\). Contact[technical support](https://workorder-intl.console.aliyun.com/#/ticket/createIndex) to apply for a trial.

## Comparison of storage classes

|Item|Standard LRS|Standard ZRS|IA LRS|IA ZRS|Archive|Cold Archive|
|:---|:-----------|------------|------|:-----|:------|------------|
|Data durability \(designed for\)|99.999999999% \(eleven 9's\)|99.9999999999% \(twelve 9's\)|99.999999999% \(eleven 9's\)|99.9999999999% \(twelve 9's\)|99.999999999% \(eleven 9's\)|99.999999999% \(eleven 9's\)|
|Service availability|99.99%|99.995%|99.00%|99.50%|99.00% \(restored data\)|99.00% \(restored data\)|
|Minimum billable size|None|None|64 KB|64 KB|64 KB|64 KB|
|Minimum storage period|None|None|30 days|30 days|60 days|180 days|
|Data retrieval fees|None|None|Based on the size of retrieved data. Unit: GB.|Based on the size of retrieved data. Unit: GB.|Based on the size of restored data. Unit: GB.|Based on the size of restored data and the data retrieval capability that is selected. Unit: GB.|
|Data access|Real-time access and low latency within milliseconds|Real-time access and low latency within milliseconds|Real-time access and low latency within milliseconds|Real-time access and low latency within milliseconds|Supported after data is restored. It takes one minute for data to restore.|Supported after data is restored. The time required to restore a Cold Archive object to the readable state is determined based on the restoration priority of the object: -   Expedited: The object is restored within one hour.
-   Standard: The object is restored within two to five hours.
-   Batch: The object is restored within 5 to 12 hours. |
|IMG|Supported|Supported|Supported|Supported|Supported after data is restored.|Supported after data is restored.|
|Scenario|Suitable for storing images for social networking and sharing applications and data for audio and video applications, large websites, and big data analytics. Examples: program download and mobile applications.|Suitable for storing images for social networking and sharing applications and data for audio and video applications, large websites, and big data analytics. Standard ZRS applies to scenarios where higher durability and availability are required. Examples: important enterprise documents and sensitive information.|Suitable for storing data that is infrequently accessed \(once or twice each month\). Examples: hot backup data and surveillance video data.|Suitable for storing data that is infrequently accessed \(once or twice each month\). IA ZRS applies to scenarios where higher durability and availability are required. Examples: enterprise business data and recent medical records.|Suitable for storing data for a long period of time. Examples: archival data, medical images, scientific materials, and video footage.|Suitable for storing extremely cold data for an ultra-long period of time. Examples: data that must be retained for an extended period of time due to compliance requirements, raw data that has been accumulated over an extended period of time in the big data and AI fields, media resources that have been retained in the film and television industries, and archived videos from the online education industry.|

**Note:** OSS charges data retrieval fees based on the amount of data read from the underlying distributed storage system. Data transmitted over the Internet is billed as outbound traffic.

