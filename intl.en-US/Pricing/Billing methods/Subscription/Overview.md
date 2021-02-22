# Overview

By default, the billing method is pay-as-you-go after you activate OSS. For some billing items, you can purchase resource plans \(subscription\) to further minimize OSS costs. Subscription allows you to use resources only after you purchase resource plans. Resource plans are used to deduct fees incurred by resource usage. If the resources you use exceed the quotas of the resource plans, the additional resources are charged on a pay-as-you-go basis.

## Deduction rules of resource plans

The following table lists the resource plans supported by OSS and corresponding deduction rules of resource plans. A check sign \(√\) indicates that the corresponding operation applies to the resource plan. A cross sign \(×\) indicates that the corresponding operation does not apply to the resource plan.

|Resource plan|Deductible fee|Upgrade|Renew|Purchase multiple resource plans|
|-------------|--------------|-------|-----|--------------------------------|
|Standard locally redundant storage \(LRS\) plan|Fees for storage usage of Standard LRS: the fees incurred when you store Standard LRS objects in a bucket.|√|√|×|
|IA LRS plan|Fees for storage usage of IA LRS: the fees incurred when you store IA LRS objects in a bucket.|√|√|×|
|Downstream data transfer plan|Fees for outbound traffic over the Internet: the fees incurred when you browse or download data from OSS over the Internet.|×|√|√|

**Note:**

-   Billing items to which resource plans do not apply such as data processing, cross-region replication \(CRR\) traffic, requests, and object tagging, are charged on a pay-as-you-go basis.
-   For more information about billing items and methods, see [Overview](/intl.en-US/Pricing/Billing items and methods/Overview.md).
-   For more information about specifications and prices of resource plans, see [OSS Resource Plan](https://common-buy-intl.alibabacloud.com/?spm=5176.8465980.bucket-list.2.11df6765PslE4M&commodityCode=oss_bag_intl#/buy).

## Purchase resource plans

1.  Log on to the [OSS Resource Plan](https://common-buy-intl.alibabacloud.com/?spm=5176.8465980.bucket-list.2.11df6765PslE4M&commodityCode=oss_bag_intl#/buy) page.

2.  On the buy page, set parameters such as Plan Type, Region, Capacity, and Validity Period. Click **Buy Now**.

3.  Follow the purchase procedure to complete the payment.


The following table describes resource plans available in different regions:

|Resource plan|Region|
|-------------|------|
|Standard LRS plan|All regions|
|IA LRS plan|All regions|
|Downstream data transfer plan|All regions|

## Upgrade resource plans

OSS allows you to upgrade resource plans. For more information, see [Upgrade resource plans](/intl.en-US/Pricing/Billing methods/Subscription/Upgrade resource plans.md).

## Renew resource plans

You can renew resource plans that are about to expire to extend the subscription periods. For more information, see [Renew resource plans](/intl.en-US/Pricing/Billing methods/Subscription/Renew resource plans.md).

## Example of resource plans

A user purchasesa nationwide Standard LRS resource plan of 500 GB and a mainland China downstream data transfer plan of 100 GB.The resource usage in June includes:

-   300 GB of Standard LRS objects, 110 GB of outbound traffic over the Internet, and 100,000 API requests in the China \(Hangzhou\) region.
-   100 GB of Standard LRS objects that are stored and 200 GB of Standard ZRS objects that are stored in the China \(Shanghai\) region.

The following table describes the usage details of resource plans.

|Region|Deductible resource plan|Pay-as-you-go|
|------|------------------------|-------------|
|China \(Hangzhou\)|Standard LRS plan of 500 GB to deduct storage fees of 300 GB|100,000 API requests|
|Downstream data transfer plan of 100 GB to deduct fees of 100 GB of outbound traffic over the Internet|Outbound traffic over the Internet of 10 GB|
|China \(Shanghai\)|Standard LRS plan of 500 GB to deduct storage fees of 100 GB|Standard ZRS plan of 200 GB|

## FAQ

-   When does a resource plan take effect?

    The resource plan takes effect immediately after the payment. However, overdue payments before you purchase the resource plan cannot be completed by using the resource plan.

-   What do I do after a resource plan expires?

    If you fail to renew a resource plan after the resource plan expires, the additional resources are charged on a pay-as-you-go basis.

-   Can I request a refund for a resource plan?

    Yes, you can request a refund for a resource plan. For more information, see [How do I cancel resource plans?](/intl.en-US/Pricing/FAQ/How do I cancel resource plans?.md).

-   What do I do if the quota of a resource plan is exceeded?

    If the resources you use exceed the quota of the resource plan, the additional resources are charged on a pay-as-you-go basis.

-   Can I purchase multiple resource plans?
    -   No, you cannot purchase multiple storage plans.

        Storage plans in the same region can be purchased only once in the same period of time. If you require a storage plan with a higher specification or longer duration, you can [upgrade](#section_nnt_zug_lsv) or [renew](#section_axy_bot_o4m) the existing storage plan.

    -   You can purchase and renew multiple downstream data transfer plans, but cannot upgrade the purchased plans.

