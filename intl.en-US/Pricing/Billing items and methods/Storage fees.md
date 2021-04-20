# Storage fees

OSS charges you for storage fees based on the storage class, size, and storage duration of the objects that you store.

**Note:** This topic describes the billable items and billing methods of OSS. For more information about the price of billable items, see [OSS pricing page](https://www.alibabacloud.com/product/oss/pricing).

OSS provides the following storage classes: Standard, Infrequent Access \(IA\), Archive, and Cold Archive. The following table describes the billable items and billing methods of the storage classes.

|Billable item|Description|Billing method|
|:------------|-----------|:-------------|
|Storage usage of Standard locally redundant storage \(LRS\)|Billed based on the total size and storage duration of Standard LRS objects.|-   Pay-as-you-go: Storage fees = Storage usage \(GB\) × Unit price per month/30 \(days\)/24 \(hours\).
-   Subscription:
    -   Standard LRS storage plan: A storage plan can deduct the storage fees for Standard LRS objects of the same size as the capacity of the plan.
    -   Storage capacity units \(SCUs\):0.075 GB of SCUs can offset the storage fees for 1 GB of Standard LRS objects in mainland China regions. For more information about fee offset rules in other regions, see the [Pricing page](https://www.alibabacloud.com/zh/product/ecs). |
|Storage usage of Standard zone-redundant storage \(ZRS\)|Billed based on the total size and storage duration of Standard ZRS objects.|-   Pay-as-you-go: Storage fees = Storage usage \(GB\) × Unit price per month/30 \(days\)/24 \(hours\).
-   Subscription:

SCUs: 0.101 GB of SCUs can offset the storage fees for 1 GB of Standard ZRS objects in mainland China regions. For more information about fee offset rules in other regions, see the [Pricing page](https://www.alibabacloud.com/zh/product/ecs). |
|Storage usage of IA LRS|Billed based on the total size and storage duration of IA LRS objects. IA storage has a minimum billable size of 64 KB for each object. Objects smaller than 64 KB in size are charged as 64 KB. Objects that are larger than or equal to 64 KB in size are charged based on their actual sizes.

|-   Pay-as-you-go: Storage fees = Billed storage usage \(GB\) × Unit price per month/30 \(days\)/24 \(hours\).
-   Subscription:
    -   IA LRS storage plan: A storage plan can offset the storage fees for IA LRS objects. The offset amount of the storage fee is equivalent to the capacity of the plan.
    -   SCUs:0.052 GB of SCUs can offset the storage fees for 1 GB of IA LRS objects in mainland China regions. For more information about fee offset rules in other regions, see the [Pricing page](https://www.alibabacloud.com/zh/product/ecs). |
|Storage usage of IA ZRS|Billed based on the total size and storage duration of IA ZRS objects. IA storage has a minimum billable size of 64 KB for each object. Objects that are smaller than 64 KB in size are charged as 64 KB. Objects that are larger than or equal to 64 KB in size are charged based on their actual sizes.

|-   Pay-as-you-go: Storage fees = Billed storage usage \(GB\) × Unit price per month/30 \(days\)/24 \(hours\).
-   Subscription:

SCUs: 0.67 GB of SCUs can offset the storage fees for 1 GB of IA ZRS objects in mainland China regions. For more information about fee offset rules in other regions, see the [Pricing page](https://www.alibabacloud.com/zh/product/ecs). |
|Storage usage of Archive LRS|Billed based on the total size and storage duration of Archive LRS objects. Archive storage has a minimum billable size of 64 KB for each object. Objects smaller than 64 KB in size are charged as 64 KB. Objects that are larger than or equal to 64 KB in size are charged based on their actual sizes.

|-   Pay-as-you-go: Storage fees = Billed storage usage \(GB\) × Unit price per month/30 \(days\)/24 \(hours\).
-   Subscription:

SCUs: 0.022 GB of SCUs can offset the storage fees for 1 GB of Archive LRS objects in mainland China regions. For more information about fee offset rules in other regions, see the [Pricing page](https://www.alibabacloud.com/zh/product/ecs). |
|Storage usage of Cold Archive LRS|Billed based on the total size and storage duration of Cold Archive LRS objects. Cold Archive storage has a minimum billable size of 64 KB for each object. Objects smaller than 64 KB in size are charged as 64 KB. Objects that are larger than or equal to 64 KB in size are charged based on their actual sizes.

|-   Pay-as-you-go: Storage fees = Billed storage usage \(GB\) × Unit price per month/30 \(days\)/24 \(hours\).
-   Subscription: not supported. |
|Storage usage of IA objects that are stored for less than the minimum storage duration|If an IA object is deleted after it is stored for less than 30 days \(720 hours\), the storage fees for the object are charged as it is stored for 30 days, including the remaining duration \(720 hours - Actual storage duration\).|-   Pay-as-you-go: Storage fees = Size of deleted objects \(GB\) × Unit price per month/30 \(days\)/24 \(hours\) × \(720 - Actual storage duration\).

Example: An IA LRS object of 100 GB is deleted after it is stored for 20 days \(480 hours\). The fees are charged based on the following formula: Storage fees = 100 \(GB\) × USD 0.015/30 \(days\)/24 \(hours\) × \(720 - 480\) = USD 0.5.

-   Subscription: A storage plan can offset the storage fees for IA LRS objects of the same size as the capacity of the plan if the IA LRS objects are stored for less than the minimum storage duration. Storage plans cannot offset the storage fees for IA ZRS objects that are stored for less than the minimum storage duration. |
|Storage usage of Archive objects that are stored for less than the minimum storage duration|If an Archive object is deleted after it is stored for less than 60 days \(1,440 hours\), the storage fees for the object are charged as it is stored for 60 days, including the remaining duration \(1,440 hours - Actual storage duration\).|-   Pay-as-you-go: Storage fees = Size of deleted objects \(GB\) × Unit price per month/30 \(days\)/24 \(hours\) × \(1,440 - Actual storage duration\).
-   Subscription: A storage plan can offset the storage fees for Archive objects. The offset amount of the storage fee is equivalent to the capacity of the plan. |
|Storage usage of Cold Archive objects that are stored for less than the minimum storage duration|If a Cold Archive object is deleted after it is stored for less than 180 days \(4,320 hours\), the storage fees for the object are charged as it is stored for 180 days, including the remaining duration \(4,320 hours - Actual storage duration\).|-   Pay-as-you-go: Storage fees = Size of deleted objects \(GB\) × Unit price per month/30 \(days\)/24 \(hours\) × \(4,320 - Actual storage duration\).
-   Subscription: not supported. |

