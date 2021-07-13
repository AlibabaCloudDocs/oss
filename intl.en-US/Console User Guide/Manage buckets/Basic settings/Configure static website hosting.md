# Configure static website hosting

Static websites are websites that consist only of static content, including scripts such as JavaScript code running on the client. You can use the static website hosting feature to host your static website on an Object Storage Service \(OSS\) bucket and use the endpoint of the bucket to access the website.

## Usage notes

For security reasons, starting from August 13, 2018 for China regions, and September 25, 2019 for regions outside China, when you access web page objects whose MIME type is text or HTML and whose name extension is HTM, HTML, JSP, PLG, HTX, or STM by using browsers:

-   If you use the default endpoint of the bucket to access the objects, the following header is contained in the response: `Content-Disposition:'attachment=filename;'`. In this case, the web page objects are downloaded as attachments. The content of the object cannot be previewed.
-   If you use a custom domain name mapped to the bucket to access the objects, the `Content-Disposition:'attachment=filename;'` header is not contained in the response. In this case, you can preview the object content if your browser supports preview of web page objects. For more information about how to map a custom domain name to a bucket, see [Bind custom domain names](/intl.en-US/Developer Guide/Buckets/Map custom domain names.md).

For more information, see [Overview](/intl.en-US/Developer Guide/Static website hosting/Static website hosting.md).

## Scenarios

In this topic, a bucket named examplebucket is used in the example to show how to enable static website hosting for a bucket. The following structure shows the objects and directories in examplebucket:

```
examplebucket
 ├── index.html
 ├── error.html
 ├── example.txt
 └── subdir/
      └── index.html
```

The first example shows how to disable subfolder homepage when you configure static website hosting for examplebucket. In this case, when you access the subdir/ directory of examplebucket, the default homepage object named index.html in the root directory of examplebucket is returned instead of the object named index.html in the subdir/ directory. In addition, if you access an object that does not exist in examplebucket, the specified default 404 page is returned. For more information, see [Configure static website hosting and disable subfolder homepage](#section_50c_83g_usi).

The second example shows how to enable subfolder homepage and configure a subfolder 404 rule when you configure static website hosting for examplebucket. In this case, when you access the subdir/ directory of examplebucket, the default homepage object named index.html in the /subdir directory of examplebucket is returned. In addition, if you access an object that does not exist in examplebucket, a result is returned based on the specified subfolder 404 rule together with the specified default 404 page. For more information, see [Configure static website hosting and enable subfolder homepage](#section_nar_1ko_j0b).

## Configure static website hosting and disable subfolder homepage

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  In the left-side navigation pane, choose **Basic Settings** \> **Static Pages**. Click **Configure** in the **Static Pages** section. Configure the parameters listed in the following table.

    ![index1](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0721716261/p285593.jpg)

    |Parameter|Description|
    |---------|-----------|
    |**Default Homepage**|Configure the default homepage that is displayed when you use a browser to access the static website hosted on the OSS bucket. In this example, set this parameter to index.html.|
    |**Subfolder Homepage**|Specify whether to enable subfolder homepage for the bucket. In this example, select **Disable**. In this case, when you access the root directory of the bucket or a subdirectory whose URL ends with a forward slash \(/\), the default homepage object in the root directory of the bucket is returned.|
    |**Default 404 Page**|Specify the error page that is returned when the object that you want to access does not exist in the bucket and 404 HTTP status code is returned. Only HTML, JPG, PNG, BMP, and WebP objects in the root directory of the bucket can be specified as the default 404 page. In this example, set this parameter to error.html.|

4.  Click **Save**.


## Configure static website hosting and enable subfolder homepage

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  In the left-side navigation pane, choose **Basic Settings** \> **Static Pages**. Click **Configure** in the **Static Pages** section. Configure the parameters listed in the following table.

    ![error](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0721716261/p285612.jpg)

    |Parameter|Description|
    |---------|-----------|
    |**Default Homepage**|Configure the default homepage that is displayed when you use a browser to access the static website hosted on the OSS bucket. In this example, set this parameter to index.html.|
    |**Subfolder Homepage**|Select **Enable**. After you enable subfolder homepage for a bucket, if you access the root directory of the bucket, the default homepage in the root directory is returned. If you access a subdirectory whose URL ends with a forward slash \(/\), the default homepage in the subdirectory is returned. For example, if you access `https://examplebucket.oss-cn-hangzhou.aliyuncs.com/subdir/`, the default homepage object `index.html` in the subdir/ directory is returned.|
    |**Subfolder 404 Rule**|Configure the subfolder 404 rule for the bucket. When you access an object that does not exist in the bucket, OSS returns different results based on the specified subfolder 404 rule. For example, if you use the URL `https://examplebucket.oss-cn-hangzhou.aliyuncs.com/exampledir` to access an object named exampledir, which does not exist in examplebucket, OSS returns different results based on the value that you set for this parameter. Default value: Redirect.    -   **Redirect**: OSS checks whether the exampledir/index.html object exists.
        -   If this object exists, OSS returns 302 and redirects the request to `https://examplebucket.oss-cn-hangzhou.aliyuncs.com/exampledir/index.html`.
        -   If this object does not exist, OSS returns 404 and checks whether the `https://examplebucket.oss-cn-hangzhou.aliyuncs.com/error.html` object exists. If this object also does not exist, OSS returns 404.
    -   **NoSuckKey**: OSS returns 404 and checks whether the `https://examplebucket.oss-cn-hangzhou.aliyuncs.com/error.html` object exists.
    -   **Index**: OSS checks whether the exampledir/index.html object exists.
        -   If this object exists, OSS returns 200 and the content of this object.
        -   If this object does not exist, OSS checks whether the `https://examplebucket.oss-cn-hangzhou.aliyuncs.com/error.html` object exists. |
    |**Default 404 Page**|Specify the error page that is returned when the object that you want to access does not exist in the bucket and a 404 HTTP status code is returned. Only HTML, JPG, PNG, BMP, and WebP objects in the root directory of the bucket can be specified as the default 404 page. In this example, set this parameter to error.html.|

4.  Click **Save**.


## Create and upload a default homepage object

If you set the default homepage to index.html when you configure static website hosting for examplebucket, you must upload an object named index.html to the root directory of examplebucket. If you enable subfolder homepage for examplebucket, you must also upload index.html to the subdir/ directory of examplebucket.

1.  Create a local file named index.html. The following example shows the content of index.html:

    ```
    <html>
    <head>
        <title>My Website Home Page</title>
        <meta charset="utf-8">
    </head>
    <body>  
      <p>Now hosted on OSS.</p>
    </body>
    </html>
    ```

2.  Save index.html to the local computer.

3.  Upload index.html to the root directory and subdir/ directory of examplebucket. You must set the access control list \(ACL\) of index.html to public read.

    For more information about how to upload objects, see [Upload objects](/intl.en-US/Quick Start/OSS console/Upload objects.md).


## Create and upload a default 404 page

If you set the default 404 page to error.html when you configure static website hosting for examplebucket, you must upload an object named error.html to the root directory of examplebucket.

1.  Create a local file named error.html. The following example shows the content of error.html:

    ```
    <html>
    <head>
        <title>Hello OSS!</title>
        <meta charset="utf-8">
    </head>
    <body>  
      <p>This is error 404 page.</p>
    </body>
    </html>
    ```

2.  Save error.html to the local computer.

3.  Upload error.html to the root directory of examplebucket. You must set the ACL of error.html to public read.

    For more information about how to upload objects, see [Upload objects](/intl.en-US/Quick Start/OSS console/Upload objects.md).


