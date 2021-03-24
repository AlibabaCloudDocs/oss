# Regions and endpoints

OSS data centers are located in regions. Endpoints are the domain names that the other services can use to access OSS. This topic describes the mappings between regions and endpoints.

## Regions and OSS endpoints for access over the classic network

The following table describes the public endpoints, internal endpoints, and accelerate endpoints of OSS in each region.

|Region|Region ID|IPv6|Public endpoint|Internal endpoint1|
|:-----|:--------|----|:--------------|------------------|
|China \(Hangzhou\)|oss-cn-hangzhou|Supported|oss-cn-hangzhou.aliyuncs.com|oss-cn-hangzhou-internal.aliyuncs.com|
|China \(Shanghai\)|oss-cn-shanghai|Supported|oss-cn-shanghai.aliyuncs.com|oss-cn-shanghai-internal.aliyuncs.com|
|China \(Qingdao\)|oss-cn-qingdao|Not supported|oss-cn-qingdao.aliyuncs.com|oss-cn-qingdao-internal.aliyuncs.com|
|China \(Beijing\)|oss-cn-beijing|Supported|oss-cn-beijing.aliyuncs.com|oss-cn-beijing-internal.aliyuncs.com|
|China \(Zhangjiakou\)|oss-cn-zhangjiakou|Not supported|oss-cn-zhangjiakou.aliyuncs.com|oss-cn-zhangjiakou-internal.aliyuncs.com|
|China \(Hohhot\)|oss-cn-huhehaote|Supported|oss-cn-huhehaote.aliyuncs.com|oss-cn-huhehaote-internal.aliyuncs.com|
|China \(Ulanqab\)|oss-cn-wulanchabu|Not supported|oss-cn-wulanchabu.aliyuncs.com|oss-cn-wulanchabu-internal.aliyuncs.com|
|China \(Shenzhen\)|oss-cn-shenzhen|Supported|oss-cn-shenzhen.aliyuncs.com|oss-cn-shenzhen-internal.aliyuncs.com|
|China \(Heyuan\)|oss-cn-heyuan|Not supported|oss-cn-heyuan.aliyuncs.com|oss-cn-heyuan-internal.aliyuncs.com|
|China \(Guangzhou\)|oss-cn-guangzhou|Not supported|oss-cn-guangzhou.aliyuncs.com|oss-cn-guangzhou-internal.aliyuncs.com|
|China \(Chengdu\)|oss-cn-chengdu|Not supported|oss-cn-chengdu.aliyuncs.com|oss-cn-chengdu-internal.aliyuncs.com|
|China \(Hong Kong\)|oss-cn-hongkong|Supported|oss-cn-hongkong.aliyuncs.com|oss-cn-hongkong-internal.aliyuncs.com|
|US \(Silicon Valley\)|oss-us-west-1|Not supported|oss-us-west-1.aliyuncs.com|oss-us-west-1-internal.aliyuncs.com|
|US \(Virginia\)|oss-us-east-1|Not supported|oss-us-east-1.aliyuncs.com|oss-us-east-1-internal.aliyuncs.com|
|Singapore|oss-ap-southeast-1|Not supported|oss-ap-southeast-1.aliyuncs.com|oss-ap-southeast-1-internal.aliyuncs.com|
|Australia \(Sydney\)|oss-ap-southeast-2|Not supported|oss-ap-southeast-2.aliyuncs.com|oss-ap-southeast-2-internal.aliyuncs.com|
|Malaysia \(Kuala Lumpur\)|oss-ap-southeast-3|Not supported|oss-ap-southeast-3.aliyuncs.com|oss-ap-southeast-3-internal.aliyuncs.com|
|Indonesia \(Jakarta\)|oss-ap-southeast-5|Not supported|oss-ap-southeast-5.aliyuncs.com|oss-ap-southeast-5-internal.aliyuncs.com|
|Japan \(Tokyo\)|oss-ap-northeast-1|Not supported|oss-ap-northeast-1.aliyuncs.com|oss-ap-northeast-1-internal.aliyuncs.com|
|India \(Mumbai\)|oss-ap-south-1|Not supported|oss-ap-south-1.aliyuncs.com|oss-ap-south-1-internal.aliyuncs.com|
|Germany \(Frankfurt\)|oss-eu-central-1|Not supported|oss-eu-central-1.aliyuncs.com|oss-eu-central-1-internal.aliyuncs.com|
|UK \(London\)|oss-eu-west-1|Not supported|oss-eu-west-1.aliyuncs.com|oss-eu-west-1-internal.aliyuncs.com|
|UAE \(Dubai\)|oss-me-east-1|Not supported|oss-me-east-1.aliyuncs.com|oss-me-east-1-internal.aliyuncs.com|

**Note:**

-   1: The internal endpoint can be used by other Alibaba Cloud services in the same region to access OSS.
-   If you use an IPv6 address to access the public endpoint of a region that supports IPv6, the DNS server resolves the endpoint to the IPv6 address of the region.
-   By default, `oss.aliyuncs.com` maps to the public endpoint of the China \(Hangzhou\) region, and `oss-internal.aliyuncs.com` maps to the internal endpoint of the China \(Hangzhou\) region.
-   For more information about the usage and composition rules of OSS domain names , see [OSS domain names](/intl.en-US/Developer Guide/Endpoint/OSS domain names.md).
-   When you access OSS from ECS instances, we recommend that you use the internal endpoint of OSS. For more information about how to use the internal endpoint to access OSS, see [Access to OSS resources from ECS instances by using the internal endpoint of OSS](/intl.en-US/Developer Guide/Endpoint/Access to OSS resources from ECS instances by using the internal endpoint of OSS.md).

## Accelerate endpoint

If transfer acceleration is enabled for a bucket, the following transfer acceleration endpoints are added:

-   Global accelerate endpoint: `oss-accelerate.aliyuncs.com`. Transfer acceleration access points are distributed all over the world. You can use this endpoint to accelerate data transfer for buckets in all regions.
-   Accelerate endpoint of regions outside mainland China: `oss-accelerate-overseas.aliyuncs.com`. Transfer acceleration access points are distributed across regions outside mainland China. You can use the accelerate endpoint of regions outside mainland China when you want to bind a custom domain name for which you have not applied for an ICP filing to a bucket in the China \(Hong Kong\) region or a region outside mainland China.

For more information, see [Transfer acceleration](/intl.en-US/Developer Guide/Buckets/Transfer acceleration.md).

