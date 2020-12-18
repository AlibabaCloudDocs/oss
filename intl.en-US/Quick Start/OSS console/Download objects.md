# Download objects

This topic describes how to download one or more objects from a bucket in the OSS console.

Objects have been uploaded to the bucket. For more information, see [Upload objects](/intl.en-US/Quick Start/OSS console/Upload objects.md).

## Use the OSS console

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket to which you want to upload objects.

3.  Click the **Files** tab. You can perform the operations listed in the following table.

    |Operation|Procedure|Description|
    |---------|---------|-----------|
    |Download one or more objects at a time|Select one or more objects. Choose **Batch Operation** \> **Download**.|You can download up to 100 objects at a time in the console.|
    |Download a single object|You can use any of the following methods to download a single object:     -   Choose **More** \> **Download** in the Actions column corresponding to the object.
    -   Click the name of an object, or click **View Details** in the Actions column corresponding to the object. In the View Details dialog box that appears, click **Download**.
|None.|
    |Preview objects|You can use one of the following methods to preview an object:     -   Preview an object by using the console

Click the name of an object, or click **View Details** in the Actions column corresponding to the object. If your browser supports previews of objects in this format, you can preview the object in the View Details dialog box that appears.

    -   Preview an object by using the object URL

Click the name of an object, or click **View Details** in the Actions column corresponding to the object. In the View Details dialog box that appears, click **Open File URL**.

|If your bucket is bound to a custom domain name, find the **Custom Domain Name** section in the **View Details** dialog box. Select the bound custom domain name or **None** from the drop-down list to generate different URLs of the object. For more information about how to bind a custom domain name, see [Bind custom domain names](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind custom domain names.md).     -   When you use a browser to access web page objects, such as HTM, HTML, JSP, PLG, HTX, and STM, by using an unmodified URL generated from an OSS domain name, the objects are downloaded. However, if you use a URL generated from a custom domain name, you can preview the object in the browser.
    -   When you access an image object by using an unmodified URL:

        -   If your bucket was created before September 23, 2019in China regions, you can use an object URL generated from an OSS domain name or a custom domain name to preview the object in the browser.
        -   If your bucket was created after September 23, 2019in China regions, when you access an image object through the browser by using an object URL generated from an OSS domain name, the image object is downloaded as an attachment. When you access an image object by using an object URL generated from a custom domain name, you can preview the object in the browser.
        -   For buckets in regions outside China, when you access an image object by using the OSS domain name or a custom domain name, you can preview the object.
**Note:** Images in formats which the browser does not support previews may be directly downloaded. To resolve this problem, you must install a plug-in that supports previews of objects in the corresponding format in your browser.

    -   When you access an object in another format by using the URL, the object is downloaded as an attachment. |
    |Share an object|Click the name of an object, or click **View Details** in the Actions column corresponding to the object. In the View Details dialog box that appears, click **Copy File URL**. Send the object URL to other visitors.|If you want to share a private object, you must set **Validity Period \(Seconds\)** during which the object can be shared. The default validity period of the URL is 3,600 seconds \(one hour\). The maximum validity period is 32,400 seconds \(nine hours\). To obtain a URL with a longer validity period, we recommend that you use [ossutil](/intl.en-US/Tools/ossutil/Common commands/sign.md), [ossbrowser](/intl.en-US/Tools/ossbrowser/Quick start.md), or OSS SDK.|


## Use ossbrowser

ossbrowser is a graphical object management tool for OSS. You can use ossbrowser to download objects you need. For more information, see [Quick start](/intl.en-US/Tools/ossbrowser/Quick start.md).

## Use ossutil

ossutil is a command-line tool for OSS. You can use ossutil to download objects you need. For more information, see [cp](/intl.en-US/Tools/ossutil/Common commands/cp/Overview.md).

## Use APIs and SDKs

OSS provides APIs and SDKs that use a variety of programming languages to facilitate secondary development. For more information, see the following topics:

-   API operation: [GetObject](/intl.en-US/API Reference/Object operations/Basic operations/GetObject.md)
-   Java SDK: [Overview](/intl.en-US/SDK Reference/Java/Download objects/Overview.md)
-   Python SDK: [Overview](/intl.en-US/SDK Reference/Python/Download objects/Overview.md)
-   PHP SDK: [Overview](/intl.en-US/SDK Reference/PHP/Download objects/Overview.md)
-   Go SDK: [Overview](/intl.en-US/SDK Reference/Go/Download objects/Overview.md)
-   C SDK: [Overview](/intl.en-US/SDK Reference/C/Download objects/Overview.md)

For more information about SDK examples in other programming languages, see [Introduction](/intl.en-US/SDK Reference/Introduction.md).

