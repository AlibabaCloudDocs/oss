# Configure static website hosting

You can configure static website hosting for a bucket by using the OSS console and access static websites by using the custom domain name bound to the bucket.

## Limits

For security reasons, starting from August 13, 2018 for China regions, and September 25, 2019 for regions outside China, when you access OSS web page objects whose MIME type is text or HTML and whose name extension is HTM, HTML, JSP, PLG, HTX, or STM by using browsers:

-   When you use the default custom domain name, `Content-Disposition:'attachment=filename;'` is automatically included in the response header. Web page objects are downloaded as attachments. You cannot preview the object content.
-   If you use the bound custom domain name, `Content-Disposition:'attachment=filename;'` is not included in the response header. You can preview the object content as long as your browser supports previews of web page objects. For more information about how to bind a custom domain name, see [Bind custom domain names](/intl.en-US/Developer Guide/Buckets/Bind custom domain names.md).

For more information, see [Static website hosting](/intl.en-US/Developer Guide/Static website hosting/Static website hosting.md).

## Procedure

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  In the left-side navigation pane, choose **Basic Settings** \> **Static Pages**. Click **Configure** in the **Static Pages** section. Configure the parameters listed in the following table.

    |Parameter|Description|
    |---------|-----------|
    |**Default Homepage**|Specify an index page that functions similar to index.html. Only HTML objects can be set to the index page. Static website hosting is disabled if you do not specify this parameter.|
    |**Default 404 Page**|Set the default 404 page to display when the requested resource does not exist. Only the HTML, JPG, PNG, BMP, or WebP object in the root folder can be set to the default 404 page. Default 404 Page is disabled if you do not specify this parameter.|

    **Note:** You must ensure that both the index and error documents are readable. Otherwise, the static web page cannot be accessed.

4.  Click **Save**.


