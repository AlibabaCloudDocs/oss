# Storage fees

OSS charges storage fees based on the size of objects you store and the storage duration of the objects.

**Note:** This topic describes the definition and billing methods of OSS storage fees. For more information about the detailed price, see [OSS pricing page](https://www.alibabacloud.com/product/oss/pricing).

OSS provides the following storage classes: Standard, IA, Archive, and Cold Archive. For more information, see [Overview](https://help.aliyun.com/document_detail/51374.html#concept-fcn-3xt-tdb).

Storage fees are charged based on actual storage usage and duration. The following table describes the storage fees incurred for different storage classes.

|Billable item|Description|Billing method|
|:------------|-----------|:-------------|
|Storage usage of Standard locally redundant storage \(LRS\)|Fees incurred when you store Standard LRS objects in OSS.|-   Pay-as-you-go: Storage fees = Storage usage \(GB\) × Unit price per month/30 \(days\)/24 \(hours\).
-   Subscription: Standard LRS resource plan. |
|Storage usage of Standard zone-redundant storage \(ZRS\)|Fees incurred when you store Standard ZRS objects in OSS.|-   Pay-as-you-go: Storage fees = Storage usage \(GB\) × Unit price per month/30 \(days\)/24 \(hours\).
-   Subscription: Not supported. |
|Storage usage of IA LRS|Fees incurred when you store IA LRS objects in OSS. IA storage has a minimum billable size of 64 KB for each object. Objects less than 64 KB in size are charged for 64 KB. Objects that are greater than or equal to 64 KB in size are charged based on their actual sizes.

|-   Pay-as-you-go: Storage fees = Billed storage usage \(GB\) × Unit price per month/30 \(days\)/24 \(hours\).
-   Subscription: IA LRS resource plan. |
|Storage usage of IA ZRS|Fees incurred when you store IA ZRS objects in OSS.IA storage has a minimum billable size of 64 KB for each object. Objects less than 64 KB in size are charged for 64 KB. Objects that are greater than or equal to 64 KB in size are charged based on their actual sizes.

|-   Pay-as-you-go: Storage fees = Billed storage usage \(GB\) × Unit price per month/30 \(days\)/24 \(hours\).
-   Subscription: Not supported. |
|Storage usage of Archive LRS|Fees incurred when you store Archive objects in OSS. Archive storage has a minimum billable size of 64 KB for each object. Objects less than 64 KB in size are charged for 64 KB. Objects that are greater than or equal to 64 KB in size are charged based on their actual sizes.

|-   Pay-as-you-go: Storage fees = Billed storage usage \(GB\) × Unit price per month/30 \(days\)/24 \(hours\).
-   Subscription: Not supported. |
|Storage usage of Cold Archive LRS|Fees incurred when you store Cold Archive objects in OSS. Cold Archive storage has a minimum billable size of 64 KB for each object. Objects less than 64 KB in size are charged for 64 KB. Objects that are greater than or equal to 64 KB in size are charged based on their actual sizes.

|-   Pay-as-you-go: Storage fees = Billed storage usage \(GB\) × Unit price per month/30 \(days\)/24 \(hours\).
-   Subscription: Not supported. |
|Storage usage of IA objects that are stored for less than the minimum storage duration|If an IA object is deleted after it is stored for less than 30 days \(720 hours\), the storage fees for the object is charged as it is stored for 30 days, including the remaining duration \(720 hours - Actual storage duration\).|-   Pay-as-you-go: Storage fees = Size of deleted objects \(GB\) × Unit price per month/30 \(days\)/24 \(hours\) × \(720 - Actual storage duration\).

Example: An IA object of 100 GB is deleted after it is stored for 20 days \(480 hours\). The fees are charged based on the following formula: Storage fees = 100 \(GB\) × USD 0.015/30 \(days\)/24 \(hours\) × \(720 - 480\) = USD 0.5.

-   Subscription: Not supported. |
|Storage usage of Archive objects that are stored for less than the minimum storage duration|If an Archive object is deleted after it is stored for less than 60 days \(1440 hours\), the storage fees for the object is charged as it is stored for 60 days, including the remaining duration \(1440 hours - Actual storage duration\).|-   Pay-as-you-go: Storage fees = Size of deleted objects \(GB\) × Unit price per month/30 \(days\)/24 \(hours\) × \(1,440 - Actual storage duration\).
-   Subscription: Not supported. |
|Storage usage of Cold Archive objects that are stored for less than the minimum storage duration|If a Cold Archive object is deleted after it is stored for less than 180 days \(4320 hours\), the storage fees for the object is charged as it is stored for 180 days, including the remaining duration \(4,320 hours - Actual storage duration\).|-   Pay-as-you-go: Storage fees = Size of deleted objects \(GB\) × Unit price per month/30 \(days\)/24 \(hours\) × \(4,320 - Actual storage duration\).
-   Subscription: Not supported. |

