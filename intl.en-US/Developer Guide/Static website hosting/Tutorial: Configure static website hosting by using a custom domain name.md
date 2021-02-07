# Tutorial: Configure static website hosting by using a custom domain name

You can host a static website to an OSS bucket and allow users to access the website by using the custom domain name such as example.com that is bound to the bucket. This tutorial applies to scenarios where you want to host an existing static website to or build a new static website on an OSS bucket.

## Step 1: Register a domain name

Before you build a static website, you must register a domain name for your website. We recommend that you use Alibaba Cloud Domains to register a domain name for your website. For more information, see [How to register an Alibaba Cloud domain name](/intl.en-US/Quick Start/Register a generic domain name.md).

In this example, `example.com` is used as the domain name of the website.

**Note:** If you want to bind the registered domain name to a bucket that is located in regions in mainland China, you must apply for an ICP filing for the domain name at the Ministry of Industry and Information Technology of China. For more information, see [Filing](https://beian.aliyun.com/order/selfBaIndex.htm).

## Step 2: Create a bucket

You must create a public read bucket to host the static website and store website data.

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click **Create Bucket**.

3.  In the Create Bucket panel, configure parameters for the bucket. The following table describes the parameters that you must configure.

    |Parameter|Description|
    |---------|-----------|
    |**Bucket Name**|Specify the bucket name. In this example, specify the bucket name as **examplebucket**.|
    |**Region**|Select the region where the bucket is located. In this example, select **China \(Hangzhou\)**.|
    |**Storage Class**|Select **Standard**.|
    |**Access Control List \(ACL\)**|Select **Public Read**.|

    Retain the default values for other parameters. For more information, see [Create buckets](/intl.en-US/Console User Guide/Manage buckets/Create buckets.md).


## Step 3: Create and upload web page files

You must create the default homepage and 404 error page files for the website and upload the files to the created bucket.

1.  Create two local HTML files.

    -   Default homepage file

        In this example, the following content is used to generate the homepage file index.html of the static website. Customize the content of the homepage file based on your actual requirements.

        ```
        <html>
           <head>
               <title>Hello OSS! </title>
               <meta charset="utf-8">
           </head>
           <body>
               <P>Configure static website hosting for OSS bucket</p>
               <P>This is the index page</p>
           </body>
         </html>
        ```

    -   Default 404 error page file

        In this example, the following content is used to generate the 404 error page file error.html of the static website. Customize the content of the 404 error page file based on your actual requirements.

        ```
        <html>
        <head>
           <title>Hello OSS! </title>
           <meta charset="utf-8">
        </head>
        <body>
           <P>This is the 404 page</p>
        </body>
        </html>
        ```

2.  Upload the web page files to the bucket.

    1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

    2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket to which you want to upload the web page files.

    3.  In the left-side navigation pane of the page that appears, click **Files**. On the page that appears, click **Upload**.

    4.  In the **Upload** section of the Upload panel, click **Upload** and select the two web page files that you created. Retain the default values for other parameters.


## Step 4: Configure static website hosting

1.  In the left-side navigation pane, choose **Basic Settings** \> **Static Pages**.

2.  Click **Configure**. Set **Default Homepage** to index.html and **Default 404 Page** to error.html.

    **Note:** To redirect access to a subfolder to the index.html object in the subfolder, you can enable **Subfolder Homepage**. For more information, see [Configure static website hosting](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure static website hosting.md).

3.  Click **Save**.


## Step 5: Bind the custom domain name to the bucket

Bind the registered custom domain name `example.com` to the examplebucket bucket that you create to use the domain name to access the bucket.

1.  On the Overview page of the bucket to which you want to bind the custom domain name, choose **Transmission** \> **Domain Names** in the left-side navigation pane.

2.  Click **Bind Custom Domain Name**.

3.  Enter **example.com** in the **Custom Domain Name** field and turn on **Add CNAME Record Automatically**.

4.  Click **Submit**.


## Step 5: \(Optional\) Accelerate your website by using Alibaba Cloud CDN

You can use Alibaba Cloud CDN to improve the performance of your website. Alibaba Cloud CDN caches the files of your websites, such as HTML files, images, and videos, to data centers around the world. When a visitor requests a file from your website, Alibaba Cloud CDN redirects the request to a copy of the file cached in the data center closet to the region in which the visitor is located. This way, the download speed is accelerated.

1.  On the Overview page of the bucket, choose **Transmission** \> **Domain Names** in the left-side navigation pane.

2.  Click **Not Configured** on the right side of the domain name to go to the Alibaba Cloud CDN console.

3.  On the Add Domain Name page, set the required parameters. The following table describes the parameters.

    |Parameter|Description|
    |:--------|:----------|
    |**Domain Name to Accelerate**|Use the default settings.|
    |**Resource Groups**|Select Default Resource Group.|
    |**Business Type**|Content delivery varies by business type. Select the appropriate business type based on your stored content and the content usage. Select **Image and Small File** in this example.|
    |**Origin Info**|Use the default settings.|
    |**Port**|Select the port used by CDN. Select **Port 80** in this example.|
    |**Region**|Select the region that you want to accelerate. Select **Mainland China Only** in this example.|

4.  Click **Next**, and then click **Return to Domain Name List**.

5.  Record the CNAME value of the domain name **example.com.w.kunlunsl.com**. Modify the CNAME record that is added when you bind the custom domain name to the bucket.

    1.  Log on to the [Alibaba Cloud DNS console](https://dns.console.aliyun.com/#/dns/domainList).

    2.  On the Manage DNS page, click **Configure** in the Actions column corresponding to the domain name to which you want to add a CNAME record.

    3.  Find the CNAME record that is automatically added when you bind the custom domain name. Click **Modify**.

    4.  Change **Value** to **example.com.w.kunlunsl.com**. Retain the default values for other parameters.

    5.  Click **OK**.

6.  \(Optional\) On the **Domain Names** tab, turn on **Auto CDN Cache Update**.

    If you want that auto CDN cache update can be triggered by specified operations, you can[submit a ticket](https://workorder-intl.console.aliyun.com/#/ticket/createIndex) to apply for the feature. After you are authorized to configure auto CDN cache update for specified operations, click **Supported Operations** in the Auto CDN Cache Update column corresponding to the domain name and select the operations that can trigger auto CDN cache update. For more information about the supported operations, see [4](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind accelerated domain names.md).


## Step 6: Test the website

Visit the following URLs in the browser to verify whether the website is running properly.

-   Visit `http://example.com` to access the homepage of the static website. If static website hosting is correctly configured, the following page is displayed.

    ![11](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0234662161/p206863.png)

-   Visit `http://example.com/example.txt` to access an object that does not exist in the bucket. If static website hosting is configured, the following page is displayed.

    ![404](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0234662161/p206865.png)


## Step 7: Clean up resources

The resources created in this tutorial are used only to test the environment. We recommend that you clean up the created resources after the test is completed to avoid unnecessary fees.

-   To delete the domain name that is bound to the bucket, see [Delete an accelerated domain name](/intl.en-US/Quick Start/Delete an accelerated domain name.md).
-   To delete the CNAME record that is added in Alibaba Cloud DNS, see[Delete a DNS record](https://www.alibabacloud.com/help/zh/doc-detail/29726.htm).
-   To delete created buckets and objects uploaded to the bucket, see [Delete objects](/intl.en-US/Console User Guide/Upload, download, and manage objects/Delete objects.md) and [Delete buckets](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Delete buckets.md).

