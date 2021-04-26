# Configure static website hosting

You can configure static website hosting for a bucket in the OSS console and access static websites by mapping a custom domain name to the bucket.

## Usage notes

For security reasons, starting from August 13, 2018 for China regions, and September 25, 2019 for regions outside China, when you access web page objects whose MIME type is text or HTML and whose name extension is HTM, HTML, JSP, PLG, HTX, or STM by using browsers:

-   If you use the default custom domain name, `Content-Disposition:'attachment=filename;'` is automatically included in the response header. Web page objects are downloaded as attachments. The content of the object cannot be previewed.
-   If you use a custom domain name mapped to the bucket, `Content-Disposition:'attachment=filename;'` is not included in the response header. You can preview the object content if your browser supports previewing web page objects. For more information about how to map a custom domain name to a bucket, see [Bind custom domain names](/intl.en-US/Developer Guide/Buckets/Bind custom domain names.md).

For more information, see [Static website hosting](/intl.en-US/Developer Guide/Static website hosting/Static website hosting.md).

## Procedures

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  In the left-side navigation pane, choose **Basic Settings** \> **Static Pages**. Click **Configure** in the **Static Pages** section. Configure the parameters listed in the following table.

    |Parameter|Description|
    |---------|-----------|
    |**Default Homepage**|Specify an index page that functions similar to index.html. Only HTML objects can be set to the index page. If you do not specify this parameter, static website hosting is disabled.|
    |**Default 404 Page**|Set the default 404 page that is displayed when the requested resource does not exist. Only HTML, JPG, PNG, BMP, or WebP objects in the root folder of the bucket can be set to the default 404 page. If you do not specify this parameter, Default 404 Page is disabled.|
    |**Error Page Status Code**|Set the HTTP status code that is returned with the error page. Valid values: **404** and **200**.|

    **Note:** Make sure that the objects set for **Default Homepage** and **Default 404 Page** are readable. Otherwise, the static web page cannot be accessed.

4.  Click **Save**.


