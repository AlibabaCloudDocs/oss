# How to store remote attachments from WordPress websites to OSS

This topic describes how to store remote attachments from WordPress websites to OSS.

-   OSS is activated and a bucket whose ACL is public read is created.
    -   For more information about how to activate OSS, see [Activate OSS](/intl.en-US/Console User Guide/Sign up for OSS.md).
    -   For more information about how to create a bucket, see [Create buckets](/intl.en-US/Console User Guide/Manage buckets/Create buckets.md).
-   A WordPress website is built.

The website remote attachment feature allows you to directly store uploaded attachments to a remote storage server such as a remote FTP server. Discuz! forums, PHPWind forums, and WordPress websites support the remote attachment feature.

WordPress does not provide native support for this feature, but you can use a third-party plug-in to implement this feature. In this topic, the WordPress version is 4.3.1 and the plug-in is Hacklog Remote Attachment.

## Procedure

1.  Log on to the WordPress website by using an administrator account.

2.  Click **Plug-in**. Enter **FTP** in the Keyword search bar. Press Enter.

3.  Find Hacklog Remote Attachment. Click **Install Now**.

4.  After the plug-in is installed, choose **Settings** \> **Hacklog Remote Attachment**.

5.  In the **Hacklog Remote Attachment Options** dialog box, set the FTP service information.

    |Parameter|Description|
    |---------|-----------|
    |**FTP server**|Specifies the IP address that runs ossftp. We recommend that you set this parameter to 127.0.0.1.|
    |**FTP server port number**|Specifies the port number of the FTP server. The default value is 2048.|
    |**FTP account**|Specifies the FTP account in the AccessKeyID/BucketName format. Note that the forward slash \(/\) is a delimiter and does not indicate an alternative.|
    |**FTP password**|Specifies the FTP password, which is the AccessKey secret.|
    |**FTP timeout**|Specifies the FTP timeout period. The default value is 30 seconds.|
    |**Remote basic URL**|Specifies the public endpoint of the bucket. The format is http://BucketName.Endpoint. In this example, the bucket resides in the China \(Hangzhou\) region, and the bucket name is test-hz-jh-002. Therefore, the URL is http://test-hz-jh-002.oss-cn-hangzhou.aliyuncs.com/wp. For more information about endpoints, see [OSS domain names](/intl.en-US/Developer Guide/Endpoint/OSS domain names.md).|
    |**FTP remote path**|Specifies the path for storing the attachments in the bucket. In the example, wp indicates that all attachments are stored in the wp directory in the bucket. The remote basic URL must correspond to the FTP remote path.********|
    |**HTTP remote path**|Specifies the HTTP remote path. We recommend that you set this parameter to a period \(.\).|

6.  Click **Save**.

    A test on the configuration is triggered when you click **Save**. The test result is shown in the upper part of the page.

7.  Post a new article to verify whether the configuration is successful.

    1.  After you write a new article, click **Add Media** to upload an attachment.

    2.  Click **Post**. You can view the article that you have just written.

    3.  Right-click the image and choose **Open Link in New Tab** from the shortcut menu.


