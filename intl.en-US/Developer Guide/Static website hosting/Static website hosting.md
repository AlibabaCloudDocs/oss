# Static website hosting

You can call the PutBucketWebsite operation to configure static website hosting for a bucket and access the static website by using the custom domain name bound to the bucket.

**Note:** For more information about the API operation used to configure static website hosting, see [PutBucketWebsite](/intl.en-US/API Reference/Bucket operations/Static websites/PutBucketWebsite.md).

## Implementation methods

|Implementation method|Description|
|---------------------|-----------|
|[Console](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure static website hosting.md)|A user-friendly and intuitive web application|
|[Java SDK](/intl.en-US/SDK Reference/Java/Buckets/Static website hosting.md)|SDK demos for various programming languages|
|[Python SDK](/intl.en-US/SDK Reference/Python/Buckets/Static website hosting.md)|
|[PHP SDK](/intl.en-US/SDK Reference/PHP/Buckets/Static website hosting.md)|
|[Go SDK](/intl.en-US/SDK Reference/Go/Buckets/Static website hosting.md)|
|[C SDK](/intl.en-US/SDK Reference/C/Buckets/Static website hosting.md)|
|[.NET SDK](/intl.en-US/SDK Reference/. NET/Buckets/Static website hosting.md)|
|[Node.js SDK](/intl.en-US/SDK Reference/Node. js/Buckets/Static website hosting.md)|
|[Ruby SDK](/intl.en-US/SDK Reference/Ruby/Buckets/Static website hosting.md)|

## Usage notes

OSS provides the following features to manage static websites hosted in OSS:

-   Index document support

    An index document links to a default index page returned by OSS when a user directly accesses the domain name of a static website. The index page functions similar to index.html.

-   Error document support

    An error document links to an error page returned by OSS if HTTP 4xx-related error messages such as 404 Not Found occur when a user accesses a static website. You can specify an error document to provide users with specific error messages when a page is not found.


For example, if the default homepage is set to index.html, the default 404 page is set to error.html, the bucket is oss-sample, and the endpoint used to access the bucket is oss-cn-hangzhou.aliyuncs.com:

-   When you access `http://oss-sample.oss-cn-hangzhou.aliyuncs.com/` and `http://oss-sample.oss-cn-hangzhou.aliyuncs.com/directory/`, you actually access `http://oss-sample.oss-cn-hangzhou.aliyuncs.com/index.html`.
-   If the object does not exist when you access `http://oss-sample.oss-cn-hangzhou.aliyuncs.com/object`, OSS returns `http://oss-sample.oss-cn-hangzhou.aliyuncs.com/error.html`.

## Usage notes

-   For security reasons, starting from August 13, 2018 for China regions, and September 25, 2019 for regions outside China, when you access web page objects whose MIME type is text or HTML and whose name extension is HTM, HTML, JSP, PLG, HTX, or STM by using browsers:
    -   If you use the default custom domain name, `Content-Disposition:'attachment=filename;'` is automatically included in the response header. Web page objects are downloaded as attachments. The content of the object cannot be previewed.
    -   If you use the custom domain name bound to the bucket, `Content-Disposition:'attachment=filename;'` is not included in the response header. You can preview the object content if your browser supports previewing web page objects. For more information about how to bind a custom domain name to a bucket, see [Bind custom domain names](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind custom domain names.md).
-   Static websites are websites in which all web pages consist of only static content, including scripts such as JavaScript code running on the client. A bucket in the static website hosting mode does not support content that needs to be processed by the server, such as PHP, JSP, and ASP.NET content.
-   When you configure static website hosting for a bucket, make sure that the default index document and the default 404 page document exist in the root folder and can be accessed by anonymous requests. You can set the ACL of the documents to public read or public read/write, or create a bucket policy to allow anonymous access.
-   When you configure static website hosting for a bucket:
    -   For anonymous access to the domain name of the static website, OSS returns the index page. For authorized access to the root domain name of the static website, OSS returns the result of the GetBucket operation.
    -   If you access an object that does not exist in OSS, OSS returns the specified default 404 page. You are charged for the generated traffic and the requests.

## References

[Tutorial: Configure static website hosting by using a custom domain name](/intl.en-US/Developer Guide/Static website hosting/Tutorial: Configure static website hosting by using a custom domain name.md)

