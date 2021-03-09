# OSS internal endpoints and VIP address ranges

To access an OSS bucket through the internal network from an ECS instance in another region or a device in an on-premises data center, you can use Cloud Enterprise Network \(CEN\), Express Connect, leased lines, or VPN to connect to the internal network of the region where the OSS bucket is located. Then you can configure a route that directs to the VIP address ranges of the internal network. This topic describes the VIP address ranges of OSS internal networks in each region.

**Warning:**

-   OSS divides the VIP address ranges of the internal network in each region into fixed address ranges. When you configure routes based on regions, you must configure complete routes based on the VIP address ranges listed in the following table.
-   When you access OSS through the internal network from an ECS instance, do not forbid any of the VIP address ranges described in the following table in the security group.

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
|China \(Heyuan\)|oss-cn-heyuan|oss-cn-heyuan-internal.aliyuncs.com|-   100.98.83.0/24
-   100.118.174.0/24 |
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
|Singapore \(Singapore\)|oss-ap-southeast-1|oss-ap-southeast-1-internal.aliyuncs.com|-   100.118.219.0/24
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

