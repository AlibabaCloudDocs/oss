# Share objects

After you upload objects to a bucket, you can share the URLs of the objects with third parties for download or preview. This topic describes how to share an object in the OSS console.

## Procedure

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket in which the object you want to share is stored.

3.  In the left-side navigation pane, click **Files**. Then, click the name of the object that you want to share, or click **View Details** in the Actions column corresponding to the object that you want to share.

4.  In the View Details panel, click **Copy File URL**.

    If the ACL of the object that you want to share is private, you must configure **Validity Period \(Seconds\)** during which the URL is valid. The default validity period of a URL is 3,600 seconds \(one hour\). The maximum validity period is 32,400 seconds \(nine hours\). To obtain a URL with a longer validity period, we recommend that you use [ossutil](/intl.en-US/Tools/ossutil/Common commands/sign.md), [ossbrowser](/intl.en-US/Tools/ossbrowser/Quick start.md), or [OSS SDKs](/intl.en-US/SDK Reference/Introduction.md).

    -   Preview an object by using the object URL

        To ensure that the object URL you share with third parties is used to preview the object, you must bind a custom domain name to your bucket and add a CNAME record. For more information, see [t2009223.md\#]().

    -   Download an object by using the object URL

        To ensure that the object URL you share with third parties is used to download the object, you must set the Content-Disposition field in the HTTP headers of the object to `attachment`. For more information, see [Configure object HTTP headers](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure object HTTP headers.md).


## Other implementation methods

|Operation|Implementation method|
|---------|---------------------|
|Share objects|[API operations](/intl.en-US/API Reference/Object operations/Basic operations/GetObject.md)|
|[Java SDK](/intl.en-US/SDK Reference/Java/Authorized access.md)|
|[Python SDK](/intl.en-US/SDK Reference/Python/Authorized access.md)|
|[PHP SDK](/intl.en-US/SDK Reference/PHP/Authorized access.md)|
|[Go SDK](/intl.en-US/SDK Reference/Go/Authorized access.md)|
|[C++ SDK](/intl.en-US/SDK Reference/C++/Authorized access.md)|
|[C SDK](/intl.en-US/SDK Reference/C/Authorized access.md)|
|[Node.js SDK](/intl.en-US/SDK Reference/Node. js/Authorized access.md)|
|[Browser.js SDK](/intl.en-US/SDK Reference/Browser.js/Authorized access.md)|

