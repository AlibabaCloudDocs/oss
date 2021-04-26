# Share objects

After you upload objects to a bucket, you can share the URLs of the objects with third parties for downloads or previews.

## Procedures

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the page that appears, click the name of the bucket in which the object that you want to share is stored.

3.  In the left-side navigation pane, click **Files**.

4.  Obtain the URLs of objects.

    -   Obtain the URL of a single object.
        1.  Click the name of the object that you want to share.
        2.  In the View Details panel, configure the parameters described in the following table. Then, click **Copy File URL**.

            |Parameter|Description|
            |---------|-----------|
            |**Validity Period**|If the ACL of the object that you want to share is private, you must set a validity period for the URL of the object.Valid values: 60 to 32400

Unit: seconds

To obtain a URL that has a longer validity period, we recommend that you use [ossutil](/intl.en-US/Tools/ossutil/Common commands/sign.md) or [ossbrowser](/intl.en-US/Tools/ossbrowser/Quick start.md). |
            |**Custom Domain Name**|To ensure that an image object or a web page object is previewed but not downloaded when the object is accessed by third parties, generate the URL of the object by using the custom domain name bound to the bucket.This parameter is available only when a custom domain name is bound to the bucket. For more information, see [Map custom domain names](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Map custom domain names.md). |
            |**HTTPS**|By default, the URL of an object is generated using HTTPS. To use HTTP to generate object URLs, turn off the **HTTPS** switch.|

    -   Obtain the URLs of objects in batches.
        1.  Select the objects that you want to share. Choose **Batch Operation** \> **Export URL List**.
        2.  In the Export URL List panel, configure the parameters described in the following table.

            |Parameter|Description|
            |---------|-----------|
            |**HTTPS**|By default, the URLs of objects are generated using HTTPS. To use HTTP to generate object URLs, turn off the **HTTPS** switch.|
            |**Validity Period**|If the ACL of the object that you want to share is private, you must set a validity period for the URL of the object.Valid values: 60 to 32400

Unit: seconds

To obtain a URL that has a longer validity period, we recommend that you use [ossutil](/intl.en-US/Tools/ossutil/Common commands/sign.md) or [ossbrowser](/intl.en-US/Tools/ossbrowser/Quick start.md). |
            |**Custom Domain Name**|To ensure that an image object or a web page object is previewed but not downloaded when the object is accessed by third parties, generate the URL of the object by using the custom domain name bound to the bucket.This parameter is available only when a custom domain name is bound to the bucket. For more information, see [Map custom domain names](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Map custom domain names.md). |
            |**Accelerate Endpoint**|If third parties located far from your data centers need to access the shared objects, we recommend that you use the accelerate endpoint of the bucket to generate the URLs of the objects.This parameter is available only when transfer acceleration is enabled for the bucket. For more information, see [Enable transfer acceleration](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Enable transfer acceleration.md). |

        3.  Click **OK**, and then save the URL list to a local file.
5.  Share the URL list file with third parties for previews or downloads.


