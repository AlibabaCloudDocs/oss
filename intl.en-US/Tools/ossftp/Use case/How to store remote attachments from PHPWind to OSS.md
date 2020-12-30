# How to store remote attachments from PHPWind to OSS

This topic describes how to store remote attachments from PHPWind to OSS.

-   OSS is activated and a bucket whose ACL is Public Read is created.
    -   For more information about how to activate OSS, see [Activate OSS](/intl.en-US/Console User Guide/Sign up for OSS.md).
    -   For more information about how to create a bucket, see [Create buckets](/intl.en-US/Console User Guide/Manage buckets/Create buckets.md).
-   A PHPWind forum is built.

The website remote attachment function allows you to directly store uploaded attachments to a remote storage server, which is usually a remote FTP server. Currently, Discuz! forums, PHPWind forums, and WordPress websites support the remote attachment function.

PHPWind8.7 is used for this topic.

## Procedure

1.  Log on to the PHPWind website with an administrator account.

2.  On the management page, choose **Global** \> **Upload Settings** \> **Remote Attachments**.

    ![](../images/p2813.png)

3.  Click the **FTP Settings** tab and configure parameters.

    ![](../images/p2815.png)

    |Configuration item|Description|
    |------------------|-----------|
    |**Enable FTP uploads**|Specifies whether to enable FTP uploads. Select **Enable**.|
    |**Website attachment URL**|Specifies the public endpoint of the bucket. The format is http://BucketName.Endpoint. In this example, the bucket resides in the China \(Hangzhou\) region, and the bucket name is test-hz-jh-002. Therefore, the URL is http://test-hz-jh-002.oss-cn-hangzhou.aliyuncs.com. For more information about endpoints, see [OSS domain names](/intl.en-US/Developer Guide/Endpoint/OSS domain names.md).|
    |**FTP server**|Specifies the IP address that runs ossftp. We recommend that you set this parameter to 127.0.0.1.|
    |**FTP server port number**|Specifies the port number of the FTP server. The default value is 2048.|
    |**Remote upload directory**|Specifies the remote upload directory for attachments. We recommend that you set this parameter to a period \(.\) to create a directory for attachments in the root directory of the bucket.|
    |**FTP account**|Specifies the FTP account in AccessKeyID/BucketName format. Note that the forward slash \(/\) is a delimiter and does not indicate an alternative.|
    |**FTP password**|Specifies the FTP password AccessKey secret.|
    |**FTP timeout \(seconds\)**|Specifies the FTP timeout period. Set the value to 10. If no result is returned within 10 seconds, the system returns a timeout response.|

4.  Post a new article to verify whether the configuration is successful.

    1.  Upload an image attachment to the article.

        ![](../images/p2817.png)

    2.  Right-click the image and choose **Open Link in New Tab** from the shortcut menu.

        The URL in the following figure indicates that the image has been uploaded to the OSS bucket test-hz-jh-002.

        ![](../images/p2818.png)


