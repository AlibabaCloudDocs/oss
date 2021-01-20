# Data processing fees

OSS charges data processing fees when you use the image processing, advanced image compression, video snapshots, SelectObject, or data retrieval feature.

**Note:** This topic describes the billable items and methods of data processing fees. For more information about the billing items, see [OSS pricing page](https://www.alibabacloud.com/product/oss/pricing).

|Billable item|Description|Billing method|
|-------------|-----------|--------------|
|Data scanned by SelectObject|Fees are billed based on the size of objects scanned by SelectObject.|-   Pay-as-you-go: Scanning fees = Scanned object size \(GB\) × Unit price of SelectObject-based scanning
-   Subscription: none. |
|Retrieved IA data|Fees are incurred when IA objects are accessed and billed based on the size of retrieved data. **Note:** If you use SelectObject or HTTP Range methods to obtain a part of an IA object, data retrieval fees are billed based on the size of the retrieved data in bytes. If you use other methods to access an IA object, data retrieval fees are billed based on the size of the object.

|-   Pay-as-you-go: Retrieval fees = Size of retrieved data \(GB\) × Unit price of data retrieval of IA objects.
-   Subscription: none. |
|Retrieved Archive data|Fees are incurred when Archive objects are restored and billed based on the size of restored object.No retrieval fees are incurred if an Archive object is accessed when it is in the restored state.

|-   Pay-as-you-go: Retrieval fees = Size of the restored object \(GB\) × Unit price of data retrieval of Archive objects.
-   Subscription: None. |
|Retrieved Cold Archive data|Fees are incurred when Cold Archive objects are restored and billed based on the size of restored object.No retrieval fees are incurred if a Cold Archive object is accessed when it is in the restored state.

|-   Pay-as-you-go: Retrieval fees = Size of the restored object \(GB\) × Unit price of the corresponding mode of data retrieval.
-   Subscription: none. |

