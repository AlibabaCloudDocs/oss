# Quick start

This topic describes how to install and use ossbrowser. ossbrowser is a graphical management tool developed by Alibaba Cloud. This tool provides functions similar to Windows Explorer. You can use ossbrowser to browse, upload, download, and manage objects.

## Usage notes

-   The object you want to upload is smaller than or equal to 48.8 TB in size.
-   The object you want to move or copy is smaller than or equal to 5 GB in size. To upload an object larger than 5 GB in size, we recommend that you use [ossutil](/intl.en-US/Tools/ossutil/Overview.md).
-   ossbrowser is supported on Linux, macOS, and Windows 7 or later. We recommend that you do not use ossbrowser on Windows XP or Windows Server.

## Installation

1.  Download and install ossbrowser.

    |Supported operating system|Current version|Download URL|
    |:-------------------------|---------------|:-----------|
    |Windows x32|1.13.0|[Windows x32](http://gosspublic.alicdn.com/oss-browser/1.13.0/oss-browser-win32-ia32.zip)|
    |Windows x64|[Windows x64](http://gosspublic.alicdn.com/oss-browser/1.13.0/oss-browser-win32-x64.zip)|
    |Mac|[Mac](http://gosspublic.alicdn.com/oss-browser/1.13.0/oss-browser-darwin-x64.zip)|
    |Linux x64|[Linux x64](http://gosspublic.alicdn.com/oss-browser/1.13.0/oss-browser-linux-x64.zip)|

    **Note:**

    -   To use ossbrowser in macOS, you must modify the system security configurations. For more information, see [What do I do if I cannot run ossbrowser in macOS?](/intl.en-US/Tools/ossbrowser/FAQ.md).
    -   To download more versions of ossbrowser, visit [GitHub](https://github.com/aliyun/oss-browser/blob/master/all-releases.md).
2.  Start ossbrowser and configure the parameters to log on to ossbrowser.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9814459951/p40359.png)

    |Parameter|Description|
    |---------|-----------|
    |**Endpoint**|Select the endpoint that you want to access.     -   **Default \(Public Cloud\)**: Log on to ossbrowser by using the default endpoint.
    -   **Customize**: Enter the endpoint that you want to use to log on to ossbrowser. You can use a URL that starts with "http" or "https" to log on to ossbrowser over HTTP or HTTPS. Example: https://oss-cn-beijing.aliyuncs.com. For more information about regions and endpoints, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).
    -   **cname**: Use the custom domain name that is bound to your OSS resources to log on to ossbrowser. For more information about how to bind custom domain names, see [Bind custom domain names](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind custom domain names.md). |
    |**HTTPS encryption**|When you set **Endpoint** to **Default \(Public Cloud\)**, you can configure this option. If you select this option, you can log on to ossbrowser over HTTPS. Otherwise, you can log on to ossbrowser over HTTP.|
    |**AccessKeyId** and **AccessKeySecret**|Enter the AccessKey pair of your account. To ensure data security, we recommend that you log on to ossbrowser by using the AccessKey pair of a RAM user. For more information about how to obtain AccessKey pairs, see [Create an AccessKey pair]().|
    |**Preset OSS Path**|Configure OSS paths. Only the paths that you configure are displayed. The account that you use to log on to ossbrowser must have the permission to access the configured paths. When you configure the path, you must specify the path and the corresponding region.     -   The path is in the following format: oss://bucketname/path.
    -   **Region**: Select the region to which the OSS resources belong.
Select **request payer** if pay-by-requester is enabled for the corresponding path. For more information about the pay-by-requester mode, see [Enable pay-by-requester](/intl.en-US/Developer Guide/Buckets/Enable pay-by-requester.md). |
    |**Keep me logged in**|If you select this option, ossbrowser remains logged on or automatically logs on when ossbrowser is opened.|
    |**Remember**|If you select this option, your AccessKey pair is saved. When you log on to ossbrowser, click **AK Histories** and select the saved AccessKey pair instead of entering the AccessKey pair again. Do not select this check box if you use a shared computer.|


## Manage buckets

-   Create a bucket
    1.  On the homepage of ossbrowser, click **Create Bucket**.
    2.  Configure bucket information.
        -   **Name**: The name of a bucket can be up to 63 characters in length. The name must be unique.
        -   **Region**: Set the region for the bucket.
        -   **ACL**: Set ACL for the bucket. For more information about ACLs, see [ACL](/intl.en-US/Developer Guide/Data security/Access and control/ACL.md).
        -   **Type**: Set the default storage class for the bucket. For more information about storage classes, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md).
    3.  After the configurations are complete, click **OK**.
-   Delete a bucket

    Select the bucket you want to delete. Choose **More** \> **Remove**. A bucket cannot be deleted when objects or parts are stored in it.


## Manage objects

-   Create a folder
    1.  On the homepage of ossbrowser, click the bucket in which you want to create a folder.
    2.  Click **Directory**.
    3.  Set the folder name and click **OK**.

        **Note:**

        -   The folder name must be UTF-8 characters and cannot contain emoticons.
        -   You can create only a single-level folder each time. For example, you can create a single-level folder abc, but not a multi-level folder abc/123.
        -   A subfolder cannot contain two consecutive periods \(..\) in its name.
        -   The folder name must be 1 to 254 characters in length.
-   Upload objects or folders

    In the specified bucket or folder, click **Files** or **Folder**. Select the objects or folders that you want to upload. You can upload multiple objects or folders at the same time.

-   Download objects or folders

    In the specified bucket or folder, select the objects or folders that you want to download. Click **Download**. You can download multiple objects or folders at the same time.

-   Copy objects or folders
    1.  In the specified bucket or folder, select the object or folder that you want to copy. Click **Copy**.
    2.  Go to the bucket or folder to which you want to copy the data. Click **Paste**. If the source and destination addresses of the copied object are the same, the original object is overwritten. If the storage class of the overwritten object is IA, Archive, or Cold Archive, and the object has been stored for less than the specified number of days, early deletion fees are incurred. For more information, see [Billing items and methods](/intl.en-US/Pricing/Billing items and methods/Overview.md).
-   Move objects or folders
    1.  In the specified bucket or folder, select the objects or folders that you want to move and choose **More** \> **Move**.
    2.  Go to the bucket or folder to which you want to copy the data. Click **Paste**.

        **Note:** If you move an object or folder, the object or folder is copied from the source address to the destination address. The object or folder in the source address is deleted. If the storage class of the moved object is IA, Archive, or Cold Archive, and the object has been stored for less than the specified number of days, early deletion fees are incurred.

-   Rename objects or folders

    In the specified bucket or folder, select the object or folder that you want to rename and choose **More** \> **Rename**. Enter the new name.

    **Note:**

    -   You can rename only objects smaller than 1 GB.
    -   If you rename an object or a folder, the object or folder is copied, renamed, and then saved. The original object or folder is deleted. If the storage class of the renamed object is IA, Archive, or Cold Archive, and the object has been stored for less than the specified number of days, early deletion fees are incurred.
-   Delete objects or folders

    Select the object or folder that you want to delete and choose **More** \> **Remove**. If the storage class of the deleted object is IA, Archive, or Cold Archive, and the object has been stored for less than the specified number of days, early deletion fees are incurred.

-   Generate a URL for an object
    1.  Select the specified object and choose **More** \> **Address**.
    2.  Enter the validity period of the link. Click **Generate**.

        **Note:** If the bucket is bound to a custom domain name, you can select the custom domain name to generate a URL for the object. For more information about how to bind a custom domain name, see [Bind custom domain names](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind custom domain names.md).

    3.  Click **Copy** or **Mail it** to send the URL to users who want to access the object. The generated QR code can also be used to access the object.
-   Modify HTTP headers for an object
    1.  Select an object and choose **More** \> **Http Headers**.
    2.  In the Http Headers dialog box, configure the HTTP header of the object. For more information about HTTP headers, see [Common HTTP headers](/intl.en-US/API Reference/Common HTTP headers.md).
    3.  After the configurations are complete, click **OK**.
-   Preview an object

    To preview an object, click the name of the object. ossbrowser allows you to preview objects in the TXT format and image objects smaller than 5 MB.

-   Manage parts

    Select the specified bucket. Click **Multipart**. You can delete parts that you no longer use.


