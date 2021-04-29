# Install and log on to ossbrowser

ossbrowser is a graphical management tool developed by Alibaba Cloud. This tool provides features similar to Windows Explorer. This topic describes how to install and log on to ossbrowser.

1.  Download and install ossbrowser.

    |Current version|Download link|Description|
    |---------------|-------------|-----------|
    |1.14.0|[Windows x32](https://gosspublic.alicdn.com/oss-browser/1.14.0/oss-browser-win32-ia32.zip)|Windows 7 and later versions are supported.|
    |[Windows x64](https://gosspublic.alicdn.com/oss-browser/1.14.0/oss-browser-win32-x64.zip)|
    |[macOS](https://gosspublic.alicdn.com/oss-browser/1.14.0/oss-browser-darwin-x64.zip)|Applications that are not verified by developers cannot be run on macOS. If this issue occurs, modify the security settings of your system. For more information, see [What do I do if I cannot run ossbrowser on macOS?](/intl.en-US/Tools/ossbrowser/Troubleshooting.md).|
    |[Linux x64](https://gosspublic.alicdn.com/oss-browser/1.14.0/oss-browser-linux-x64.zip) \(not recommended\)|If problems occur when you install ossbrowser on Linux, download and compile the [source code](https://github.com/aliyun/oss-browser) to troubleshoot the problems.|

2.  Log on to ossbrowser. In this topic, Windows is used in the example to describe the procedures.

    1.  Double-click `oss-browser.exe`.

    2.  Log on to ossbrowser by using one of the methods:

        -   Log on to ossbrowser by using an AccessKey pair

            You can use the AccessKey pair of an Alibaba Cloud account or a RAM user to log on to ossbrowser.

            ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9814459951/p40359.png)

            The following table describes the parameters that you can configure when you log on to ossbrowser by using an AccessKey pair.

            |Parameter|Description|
            |---------|-----------|
            |**Endpoint**|Select the endpoint of region that you want to access.             -   **Default \(Public Cloud\)**: Use the endpoint of the region in which the current bucket is located to log on to ossbrowser.

When you select this value, you can select **HTTPS encryption** to encrypt data transmission.

            -   **Customize**: Use the endpoint point of another region to log on to ossbrowser. Example: `https://oss-cn-beijing.aliyuncs.com`. For more information about regions and endpoints, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).
            -   **cname**: Use a custom domain name to access Object Storage Service \(OSS\) resources. You must map a custom domain name to the bucket that you want to access in advance. For more information, see [Map custom domain names](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Map custom domain names.md). After you map a custom domain name to the bucket, enter the custom domain name to access the bucket. |
            |**AccessKeyId** and **AccessKeySecret**|Enter the AccessKey pair of your account. For more information about how to obtain the AccessKey pair, see [Create an AccessKey pair](). **Note:** To ensure data security, we recommend that you log on to ossbrowser by using the AccessKey pair of a RAM user. Before you use the AccessKey pair of a RAM user to log on to ossbrowser, you must grant the following permissions to the RAM user: `AliyunOSSFullAccess`, `AliyunRAMFullAccess`, and `AliyunSTSAssumeRoleAccess`. For more information about how to grant permissions, see [Permission management](/intl.en-US/Tools/ossbrowser/Permission management.md). |
            |**Preset OSS Path**|If the current account has only permissions to access a specific bucket or a specific path in a bucket, you must specify this parameter in the following format: oss://bucketname/path. For example, if you are authorized to access only objects or subdirectories in the examplefolder directory of a bucket named examplebucket, enter oss://examplebucket/examplefolder/.

If pay-by-requester is enabled for the bucket that you are authorized to access and you are not the bucket owner, select **request payer**. Otherwise, the `AccessDenied` error is returned when you access the resources specified by Preset OSS Path. If you select **request payer**, you are charged for the traffic and requests that are generated when you access the resources specified by Preset OSS Path. For more information about how to enable pay-by-requester, see [Enable pay-by-requester](/intl.en-US/Developer Guide/Buckets/Enable pay-by-requester.md). |
            |**Region**|If you select **Default \(Public Cloud\)** for Endpoint, you must set **Region** to the bucket in which the resources specified by Preset OSS Path are stored.|
            |**Keep me logged in**|If you select this option, ossbrowser remains logged on when you start ossbrowser next time.|
            |**Remember**|If you select this option, your AccessKey pair used to log on to ossbrowser is saved. When you log on to ossbrowser next time, click **AK Histories** and select the saved AccessKey pair instead of entering the AccessKey pair again. Warning:

For security reasons, do not select this option if you use a shared computer. |

        -   Log on to ossbrowser by using an authorization code

            To authorize other users to temporarily access specific resources in your buckets, you can generate an authorization code that can be used by these users to log on to ossbrowser. The authorization code becomes invalid after it expires. For more information about how to use an authorization code to log on to ossbrowser, see [Log on to ossbrowser by using STS tokens](/intl.en-US/Tools/ossbrowser/Permission management.md).


