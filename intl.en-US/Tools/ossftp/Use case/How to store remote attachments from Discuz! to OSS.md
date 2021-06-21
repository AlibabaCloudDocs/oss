# How to store remote attachments from Discuz! to OSS

This topic describes how to store remote attachments from Discuz! to OSS.

-   OSS is activated and a bucket whose ACL is Public Read is created.
    -   For more information about how to activate OSS, see [Activate OSS](/intl.en-US/Console User Guide/Sign up for OSS.md).
    -   For more information about how to create a bucket, see [Create buckets](/intl.en-US/Console User Guide/Manage buckets/Create buckets.md).
-   A Discuz! forum is built.

The website remote attachment function allows you to directly store uploaded attachments to a remote storage server, which is usually a remote FTP server. Currently, Discuz! forums, PHPWind forums, and WordPress websites support the remote attachment function.

The version of Discuz! used for this topic is Discuz! X3.1.

## Procedure

1.  Log on to the Discuz! website with an administrator account.

2.  On the management page, choose **Global** \> **Upload Settings**.

3.  Click the **Remote Attachment** tab and configure parameters.

    |Configuration item|Description|
    |------------------|-----------|
    |**Enable remote attachment**|Specifies whether to enable remote attachment. Select **Yes**.|
    |**Enable SSL connection**|Specifies whether to enable SSL connection. Select **No**.|
    |**FTP server**|Specifies the IP address that runs ossftp. We recommend that you set this parameter to 127.0.0.1.|
    |**FTP server port number**|Specifies the port number of the FTP server. The default value is 2048.|
    |**FTP account**|Specifies the FTP account in AccessKeyID/BucketName format. Note that the forward slash \(/\) is a delimiter and does not indicate an alternative.|
    |**FTP password**|Specifies the FTP password AccessKey secret.|
    |**Passive mode connection**|Specifies whether to enable passive mode connection. Select **Yes**.|
    |**Remote upload directory**|Specifies the remote upload directory for attachments. We recommend that you set this parameter to a period \(.\) to create a directory for attachments in the root directory of the bucket.|
    |**Remote access URL**|Specifies the public endpoint of the bucket. The format is http://BucketName.Endpoint. In this example, the bucket resides in the China \(Hangzhou\) region, and the bucket name is test-hz-jh-002. Therefore, the URL is http://test-hz-jh-002.oss-cn-hangzhou.aliyuncs.com. For more information about endpoints, see [OSS domain names](/intl.en-US/Developer Guide/Endpoint/OSS domain names.md).|
    |**FTP timeout \(seconds\)**|Specifies the FTP timeout period. Set the value to 0, indicating that the default setting of the server applies.|

4.  After the configuration is complete, click **Test Remote Attachment** to verify whether the configuration is correct.

5.  Post a new article to verify whether the configuration is successful.

    1.  Upload an image attachment to the article.

    2.  Right-click the image and choose **Open Link in New Tab** from the shortcut menu.

        The URL in the following figure indicates that the image has been uploaded to the OSS bucket test-hz-jh-002.

        ![png](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1144424261/p285025.png)


