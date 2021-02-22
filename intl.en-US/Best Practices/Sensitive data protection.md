# Sensitive data protection

This topic describes how to integrate Alibaba Cloud Object Storage Service \(OSS\) with Sensitive Data Discovery and Protection \(SDDP\) to identify, classify, and protect sensitive data.

-   SDDP is activated.

    For more information, see [Quick start](/intl.en-US/Quick Start/Quick start.md).

-   OSS is activated.

    For more information, see [Activate OSS](/intl.en-US/Console User Guide/Sign up for OSS.md).


Sensitive data is stored in a variety of forms across different storage systems, and can include high value data such as personal data, passwords, keys, and sensitive images. How to identify, locate, and protect sensitive data is essential. OSS provides a number of options to secure data, such as fine-grained [access control](/intl.en-US/Developer Guide/Data security/Access and control/Overview.md) and [data encryption](/intl.en-US/Developer Guide/Data security/Data encryption/Server-side encryption.md). OSS also provides data protection mechanisms such as [ZRS](/intl.en-US/Developer Guide/Data security/Disaster recovery/ZRS.md), [cross-region replication](/intl.en-US/Developer Guide/Data security/Disaster recovery/Cross-region replication.md), and [versioning](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md), as well as monitoring and audit capabilities such as [Logging](/intl.en-US/Developer Guide/Manage logs/Logging.md) and [Real-time log query](/intl.en-US/Developer Guide/Manage logs/Real-time log query.md). You can also integrate OSS with SDDP to better identify, classify, and protect sensitive data.

After you authorize SDDP to scan your OSS buckets, SDDP identifies sensitive data from your large amounts of data, classifies and displays sensitive data by risk level, and tracks how sensitive data is used. In addition, SDDP protects and audits sensitive data based on predefined security rules so that you can obtain the security status of your data assets in OSS buckets at any time. For more information, see [What is Sensitive Data Discovery and Protection?](/intl.en-US/Product Introduction/What is Sensitive Data Discovery and Protection?.md).

**Note:** After you authorize SDDP to scan your OSS buckets, SDDP scans all objects stored in your OSS buckets at the first scan and charges you for a full scan. If you add new objects to or modify objects in your OSS buckets after the first scan, SDDP will only scan the new or modified objects to minimize charges. For more information about billing methods, see [Pay-as-you-go](/intl.en-US/Pricing/Pay-as-you-go.md).

## Scenarios

-   Sensitive data identification

    Enterprises have large amounts of data, but they cannot accurately identify whether their data contains sensitive information or where the sensitive data is located. You can integrate OSS with SDDP to scan and classify data stored in OSS by using the built-in rules for algorithms of SDDP or by using custom rules that meet your industry requirements. You can also make further protection arrangements based on scan results. For example, OSS provides access control and encryption features to protect data.

-   Data masking

    If you share data for analysis or use without first masking it, sensitive data may be leaked. Built-in and custom masking algorithms are available when OSS is integrated with SDDP. You can use these algorithms to mask sensitive data in the production environment before data is transferred to other environments such as the development and testing environments. This ensures that the sensitive data remains secure while being usable in other environments.

-   Anomaly detection and audit

    SDDP uses an intelligent model to analyze and audit access to sensitive data in OSS. If a risk is detected, SDDP sends an alert to your data security team. This helps you improve risk prediction and prevention capabilities.


## Benefits

-   Visual
    -   SDDP displays sensitive data detection results on a graphical user interface \(GUI\), which allows you to clearly view the security status of your data.
    -   SDDP monitors data access and provides audit logs for you to trace anomalous activities, which reduces security risks for your data.
    -   SDDP increases the overall security transparency of your data assets and enhances data governance.
    -   SDDP reduces the cost of maintaining data security and provides fundamental data for you to formulate security rules that are suitable for your enterprise.
-   Intelligent
    -   SDDP uses big data and machine learning technologies as well as intelligent algorithms to detect and monitor sensitive data and high-risk activities such as anomalous data access and potential data leaks. Additionally, SDDP provides suggestions to resolve detected issues.
    -   SDDP allows you to customize the rules to detect sensitive data so that you can ensure that sensitive data is detected and protected more accurately and efficiently.
    -   SDDP integrates complex data formats and content to a unified data risk model and presents data in a standard manner for you to protect your key data assets.
-   Cloud-native
    -   SDDP takes advantage of cloud services and supports multiple cloud data sources.
    -   Compared with traditional sensitive data protection software, SDDP provides a more robust service architecture and higher availability at lower costs and features higher system security.

## Procedure

1.  Log on to the [SDDP console](https://yundun.console.aliyun.com/?p=sddp#/overview).

2.  In the left-side navigation pane, choose **Data asset authorization** \> **Data asset authorization**.

3.  On the OSS tab, click **Unauthorized**.

4.  Select the required OSS bucket and click **Batch Operation**.

    You can also click **Authorization** in the Open protection column corresponding to the required bucket to authorize a bucket.

5.  In the **Batch processing for selected assets** dialog box, configure the following parameters:

    -   **Identify permissions**: specifies whether to grant SDDP the sensitive data detection permission on the selected data assets.
    -   **Audit permissions**: specifies whether to grant SDDP the audit permission on the selected data assets.
    -   **Desensitization permissions**: specifies whether to grant SDDP the sensitive data de-identification permission on the selected data assets.
    -   **Sensitive data sampling**: the number of samples to be collected from the selected data assets. Valid values: 0, 5, and 10.
    -   **Audit log archiving**: the number of days for which audit logs are retained for the selected data assets. Unit: days. Valid values: 30, 90, and 180.

        **Note:** You do not need to activate Log Service to archive audit logs generated by SDDP.

6.  Click **OK**.

    After the authorization is completed, SDDP scans authorized OSS buckets for sensitive data. When SDDP accesses an OSS bucket for the first time, SDDP automatically scans all the data in the OSS bucket, and you are charged for the full scan. For more information, see [How long does it take to scan data in my data asset after I authorize SDDP to access the data asset?](/intl.en-US/FAQ/Sensitive data scan and detection.md).

    In the list of authorized data assets, you can modify the authorization configuration for a data asset or cancel the authorization for a data asset. After you cancel the authorization, SDDP no longer scans the OSS bucket.

    **Note:** SDDP scans only authorized OSS buckets and analyzes risks of sensitive data detected in these OSS buckets.

7.  Add a security policy.

    After sensitive data is scanned, you can start to take measures to harden security, such as configuring [server-side encryption](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure server-side encryption.md) and [access control](/intl.en-US/Developer Guide/Data security/Access and control/Overview.md) based on the scan results.


