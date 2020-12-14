# Regions and endpoints

Regions indicate where the OSS data center is located. Endpoints are the domain names that the other services can use to access OSS. This topic describes the mappings between regions and endpoints.

## Regions and OSS endpoints for access over the classic network

Access over HTTPS is allowed in the classic network. The following table describes the public and internal endpoints of OSS in each region for access over the classic network.

|Region|Region ID|Public endpoint|Internal endpoint for access from ECS1|Accelerate endpoint2|
|:-----|:--------|:--------------|--------------------------------------|--------------------|
|China \(Hangzhou\)|oss-cn-hangzhou|oss-cn-hangzhou.aliyuncs.com|oss-cn-hangzhou-internal.aliyuncs.com|-   Global accelerate endpoint: oss-accelerate.aliyuncs.com
-   Accelerate endpoint of regions outside mainland China: oss-accelerate-overseas.aliyuncs.com |
|China \(Shanghai\)|oss-cn-shanghai|oss-cn-shanghai.aliyuncs.com|oss-cn-shanghai-internal.aliyuncs.com|-   Global accelerate endpoint: oss-accelerate.aliyuncs.com
-   Accelerate endpoint of regions outside mainland China: oss-accelerate-overseas.aliyuncs.com |
|China \(Qingdao\)|oss-cn-qingdao|oss-cn-qingdao.aliyuncs.com|oss-cn-qingdao-internal.aliyuncs.com|-   Global accelerate endpoint: oss-accelerate.aliyuncs.com
-   Accelerate endpoint of regions outside mainland China: oss-accelerate-overseas.aliyuncs.com |
|China \(Beijing\)|oss-cn-beijing|oss-cn-beijing.aliyuncs.com|oss-cn-beijing-internal.aliyuncs.com|-   Global accelerate endpoint: oss-accelerate.aliyuncs.com
-   Accelerate endpoint of regions outside mainland China: oss-accelerate-overseas.aliyuncs.com |
|China \(Zhangjiakou\)|oss-cn-zhangjiakou|oss-cn-zhangjiakou.aliyuncs.com|oss-cn-zhangjiakou-internal.aliyuncs.com|-   Global accelerate endpoint: oss-accelerate.aliyuncs.com
-   Accelerate endpoint of regions outside mainland China: oss-accelerate-overseas.aliyuncs.com |
|China \(Hohhot\)|oss-cn-huhehaote|oss-cn-huhehaote.aliyuncs.com|oss-cn-huhehaote-internal.aliyuncs.com|-   Global accelerate endpoint: oss-accelerate.aliyuncs.com
-   Accelerate endpoint of regions outside mainland China: oss-accelerate-overseas.aliyuncs.com |
|China \(Ulanqab\)|oss-cn-wulanchabu|oss-cn-wulanchabu.aliyuncs.com|oss-cn-wulanchabu-internal.aliyuncs.com|-   Global accelerate endpoint: oss-accelerate.aliyuncs.com
-   Accelerate endpoint of regions outside mainland China: oss-accelerate-overseas.aliyuncs.com |
|China \(Shenzhen\)|oss-cn-shenzhen|oss-cn-shenzhen.aliyuncs.com|oss-cn-shenzhen-internal.aliyuncs.com|-   Global accelerate endpoint: oss-accelerate.aliyuncs.com
-   Accelerate endpoint of regions outside mainland China: oss-accelerate-overseas.aliyuncs.com |
|China \(Heyuan\)|oss-cn-heyuan|oss-cn-heyuan.aliyuncs.com|oss-cn-heyuan-internal.aliyuncs.com|N/A|
|China \(Guangzhou\)|oss-cn-guangzhou|oss-cn-guangzhou.aliyuncs.com|oss-cn-guangzhou-internal.aliyuncs.com|-   Global accelerate endpoint: oss-accelerate.aliyuncs.com
-   Accelerate endpoint of regions outside mainland China: oss-accelerate-overseas.aliyuncs.com |
|China \(Chengdu\)|oss-cn-chengdu|oss-cn-chengdu.aliyuncs.com|oss-cn-chengdu-internal.aliyuncs.com|-   Global accelerate endpoint: oss-accelerate.aliyuncs.com
-   Accelerate endpoint of regions outside mainland China: oss-accelerate-overseas.aliyuncs.com |
|China \(Hong Kong\)|oss-cn-hongkong|oss-cn-hongkong.aliyuncs.com|oss-cn-hongkong-internal.aliyuncs.com|-   Global accelerate endpoint: oss-accelerate.aliyuncs.com
-   Accelerate endpoint of regions outside mainland China: oss-accelerate-overseas.aliyuncs.com |
|US \(Silicon Valley\)|oss-us-west-1|oss-us-west-1.aliyuncs.com|oss-us-west-1-internal.aliyuncs.com|-   Global accelerate endpoint: oss-accelerate.aliyuncs.com
-   Accelerate endpoint of regions outside mainland China: oss-accelerate-overseas.aliyuncs.com |
|US \(Virginia\)|oss-us-east-1|oss-us-east-1.aliyuncs.com|oss-us-east-1-internal.aliyuncs.com|-   Global accelerate endpoint: oss-accelerate.aliyuncs.com
-   Accelerate endpoint of regions outside mainland China: oss-accelerate-overseas.aliyuncs.com |
|Singapore|oss-ap-southeast-1|oss-ap-southeast-1.aliyuncs.com|oss-ap-southeast-1-internal.aliyuncs.com|-   Global accelerate endpoint: oss-accelerate.aliyuncs.com
-   Accelerate endpoint of regions outside mainland China: oss-accelerate-overseas.aliyuncs.com |
|Australia \(Sydney\)|oss-ap-southeast-2|oss-ap-southeast-2.aliyuncs.com|oss-ap-southeast-2-internal.aliyuncs.com|-   Global accelerate endpoint: oss-accelerate.aliyuncs.com
-   Accelerate endpoint of regions outside mainland China: oss-accelerate-overseas.aliyuncs.com |
|Malaysia \(Kuala Lumpur\)|oss-ap-southeast-3|oss-ap-southeast-3.aliyuncs.com|oss-ap-southeast-3-internal.aliyuncs.com|-   Global accelerate endpoint: oss-accelerate.aliyuncs.com
-   Accelerate endpoint of regions outside mainland China: oss-accelerate-overseas.aliyuncs.com |
|Indonesia \(Jakarta\)|oss-ap-southeast-5|oss-ap-southeast-5.aliyuncs.com|oss-ap-southeast-5-internal.aliyuncs.com|-   Global accelerate endpoint: oss-accelerate.aliyuncs.com
-   Accelerate endpoint of regions outside mainland China: oss-accelerate-overseas.aliyuncs.com |
|Japan \(Tokyo\)|oss-ap-northeast-1|oss-ap-northeast-1.aliyuncs.com|oss-ap-northeast-1-internal.aliyuncs.com|-   Global accelerate endpoint: oss-accelerate.aliyuncs.com
-   Accelerate endpoint of regions outside mainland China: oss-accelerate-overseas.aliyuncs.com |
|India \(Mumbai\)|oss-ap-south-1|oss-ap-south-1.aliyuncs.com|oss-ap-south-1-internal.aliyuncs.com|-   Global accelerate endpoint: oss-accelerate.aliyuncs.com
-   Accelerate endpoint of regions outside mainland China: oss-accelerate-overseas.aliyuncs.com |
|Germany \(Frankfurt\)|oss-eu-central-1|oss-eu-central-1.aliyuncs.com|oss-eu-central-1-internal.aliyuncs.com|-   Global accelerate endpoint: oss-accelerate.aliyuncs.com
-   Accelerate endpoint of regions outside mainland China: oss-accelerate-overseas.aliyuncs.com |
|UK \(London\)|oss-eu-west-1|oss-eu-west-1.aliyuncs.com|oss-eu-west-1-internal.aliyuncs.com|-   Global accelerate endpoint: oss-accelerate.aliyuncs.com
-   Accelerate endpoint of regions outside mainland China: oss-accelerate-overseas.aliyuncs.com |
|UAE \(Dubai\)|oss-me-east-1|oss-me-east-1.aliyuncs.com|oss-me-east-1-internal.aliyuncs.com|N/A|

**Note:**

-   1ECS instances can use internal endpoints to communicate with OSS only when the ECS instances and buckets are in the same region.
-   2Transfer acceleration endpoints include global accelerate endpoints and accelerate endpoints of regions outside mainland China. Accelerate endpoints take effect after transfer acceleration is enabled. For more information, see [Transfer acceleration](/intl.en-US/Developer Guide/Buckets/Transfer acceleration.md).
-   We recommend that you use third-level domains that are in the `Bucket` + `Endpoint` format to share links or bind custom domain names \(CNAME\). For example, the third-level domain for the oss-sample bucket in the China \(Shanghai\) region is `oss-sample.oss-cn-shanghai.aliyuncs.com`.
-   When you initialize endpoints for OSS SDKs, we recommend that you specify the parameter in the following format: `http(s)://` + `Endpoint`. For example, we recommend that you set the initialization parameter to `http://oss-cn-shanghai.aliyuncs.com` or `https://oss-cn-shanghai.aliyuncs.com` for the endpoint of the China \(Shanghai\) region, instead of the `http://bucket.oss-cn-shanghai.aliyuncs.com` third-level domain.
-   By default, the `oss.aliyuncs.com` endpoint maps to the public endpoint of the China \(Hangzhou\) region, and the `oss-internal.aliyuncs.com` endpoint maps to the internal endpoint of the China \(Hangzhou\) region.

## Regions and OSS endpoints in VPCs

Access over HTTPS is allowed in VPCs. You can use the following endpoints to access OSS from an ECS instance in the same region.

**Warning:** When you connect to OSS internal endpoints by using Cloud Enterprise Network \(CEN\), Express Connect, leased lines, or VPN, you must configure routes for network products. OSS divides internal VIP address ranges for each region into fixed address ranges. When you configure routes based on regions, you must configure complete VIP address ranges as listed in the following table.

|Region|Region ID|Endpoint in VPCs|VIP address range|
|:-----|:--------|----------------|-----------------|
|China \(Hangzhou\)|oss-cn-hangzhou|oss-cn-hangzhou-internal.aliyuncs.com|-   100.118.28.0/24
-   100.114.102.0/24
-   100.98.170.0/24
-   100.118.31.0/24 |
|China \(Shanghai\)|oss-cn-shanghai|oss-cn-shanghai-internal.aliyuncs.com|-   100.98.35.0/24
-   100.98.110.0/24
-   100.98.169.0/24
-   100.118.102.0/24 |
|China \(Qingdao\)|oss-cn-qingdao|oss-cn-qingdao-internal.aliyuncs.com|-   100.115.173.0/24
-   100.99.113.0/24
-   100.99.114.0/24
-   100.99.115.0/24 |
|China \(Beijing\)|oss-cn-beijing|oss-cn-beijing-internal.aliyuncs.com|-   100.118.58.0/24
-   100.118.167.0/24
-   100.118.170.0/24
-   100.118.171.0/24
-   100.118.172.0/24
-   100.118.173.0/24 |
|China \(Zhangjiakou\)|oss-cn-zhangjiakou|oss-cn-zhangjiakou-internal.aliyuncs.com|-   100.118.90.0/24
-   100.98.159.0/24
-   100.114.0.0/24
-   100.114.1.0/24 |
|China \(Hohhot\)|oss-cn-huhehaote|oss-cn-huhehaote-internal.aliyuncs.com|-   100.118.195.0/24
-   100.99.110.0/24
-   100.99.111.0/24
-   100.99.112.0/24 |
|China \(Ulanqab\)|oss-cn-wulanchabu|oss-cn-wulanchabu-internal.aliyuncs.com|-   100.114.11.0/24
-   100.114.12.0/24
-   100.114.100/24
-   100.118.214.0/24 |
|China \(Shenzhen\)|oss-cn-shenzhen|oss-cn-shenzhen-internal.aliyuncs.com|-   100.118.78.0/24
-   100.118.203.0/24
-   100.118.204.0/24
-   100.118.217.0/24 |
|China \(Heyuan\)|oss-cn-heyuan|oss-cn-heyuan-internal.aliyuncs.com|Submit a ticket to obtain VIP address ranges.|
|China \(Guangzhou\)|oss-cn-guangzhou|oss-cn-guangzhou-internal.aliyuncs.com|-   100.115.33.0/24
-   100.114.101.0/24 |
|China \(Chengdu\)|oss-cn-chengdu|oss-cn-chengdu-internal.aliyuncs.com|-   100.115.155.0/24
-   100.99.107.0/24
-   100.99.108.0/24
-   100.99.109.0/24 |
|China \(Hong Kong\)|oss-cn-hongkong|oss-cn-hongkong-internal.aliyuncs.com|-   100.115.61.0/24
-   100.99.103.0/24
-   100.99.104.0/24
-   100.99.106.0/24 |
|US \(Silicon Valley\)|oss-us-west-1|oss-us-west-1-internal.aliyuncs.com|Submit a ticket to obtain VIP address ranges.|
|US \(Virginia\)|oss-us-east-1|oss-us-east-1-internal.aliyuncs.com|-   100.115.60.0/24
-   100.99.100.0/24
-   100.99.101.0/24
-   100.99.102.0/24 |
|Singapore|oss-ap-southeast-1|oss-ap-southeast-1-internal.aliyuncs.com|-   100.118.219.0/24
-   100.99.213.0/24
-   100.99.116.0/24
-   100.99.117.0/24 |
|Australia \(Sydney\)|oss-ap-southeast-2|oss-ap-southeast-2-internal.aliyuncs.com|Submit a ticket to obtain VIP address ranges.|
|Malaysia \(Kuala Lumpur\)|oss-ap-southeast-3|oss-ap-southeast-3-internal.aliyuncs.com|-   100.118.165.0/24
-   100.99.125.0/24
-   100.99.130.0/24
-   100.99.131.0/24 |
|Indonesia \(Jakarta\)|oss-ap-southeast-5|oss-ap-southeast-5-internal.aliyuncs.com|Submit a ticket to obtain VIP address ranges.|
|Japan \(Tokyo\)|oss-ap-northeast-1|oss-ap-northeast-1-internal.aliyuncs.com|Submit a ticket to obtain VIP address ranges.|
|India \(Mumbai\)|oss-ap-south-1|oss-ap-south-1-internal.aliyuncs.com|-   100.118.211.0/24
-   100.99.122.0/24
-   100.99.123.0/24
-   100.99.124.0/24 |
|Germany \(Frankfurt\)|oss-eu-central-1|oss-eu-central-1-internal.aliyuncs.com|Submit a ticket to obtain VIP address ranges.|
|UK \(London\)|oss-eu-west-1|oss-eu-west-1-internal.aliyuncs.com|Submit a ticket to obtain VIP address ranges.|
|UAE \(Dubai\)|oss-me-east-1|oss-me-east-1-internal.aliyuncs.com|Submit a ticket to obtain VIP address ranges.|

## Use endpoints

-   For more information about the composition rules of OSS domain names and how to access OSS over the internal network and Internet, see [OSS domain names](/intl.en-US/Developer Guide/Endpoint/OSS domain names.md).
-   For more information about how to access OSS by using an OSS internal endpoint from an ECS instance in the same region, see [Access to OSS resources from ECS instances by using the internal endpoint of OSS](/intl.en-US/Developer Guide/Endpoint/Access to OSS resources from ECS instances by using the internal endpoint of OSS.md).

