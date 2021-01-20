# SCU

Storage capacity units \(SCUs\) are subscription storage resource plans that can be used to deduct the storage fees of a variety of Alibaba Cloud storage services. Compared with storage plans that are dedicated to specific storage services, SCUs are more flexible and cost-effective.

## Purchase method

For more information about how to purchase SCUs, see [Create an SCU](/intl.en-US/Block Storage/Storage capacity units/Create an SCU.md).

**Note:** You can purchase multiple SCUs in the same region within the same billing periods to deduct your storage fees.

## Deduction rules

SCUs can be used to deduct the storage fees of OSS, ECS, and NAS. The following table describes the rules that apply when you use SCUs to deduct the storage fees of different storage classes of OSS resources in mainland China regions.

**Note:** For more information about the deduction rules for other Alibaba Cloud storage services and regions, see the [Pricing page](https://www.aliyun.com/price/product#/ecs/detail).

|Storage class|Deduction factor|Description|
|-------------|----------------|-----------|
|Standard LRS|0.075|0.075 GB of SCU capacity can deduct the storage fees for 1 GB of Standard LRS objects.|
|Standard ZRS|0.101|0.101 GB of SCU capacity can deduct the storage fees for 1 GB of Standard ZRS objects.|
|IA LRS|0.052|0.052 GB of SCU capacity can deduct the storage fees for 1 GB of IA LRS objects.|
|IA ZRS|0.67|0.67 GB of SCU capacity can deduct the storage fees for 1 GB of IA ZRS objects.|
|Archive LRS|0.022|0.022 GB of SCU capacity can deduct the storage fees for 1 GB of Archive LRS objects.|

## Deduction priority

Storage plans and SCUs are used to deduct the storage fees of OSS in the following priority: storage plans for specific regions \> storage plans for all mainland China regions \> SCUs \> pay-as-you-go.

