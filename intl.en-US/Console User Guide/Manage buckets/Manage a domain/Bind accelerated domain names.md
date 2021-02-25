# Bind accelerated domain names

You can use OSS and Alibaba Cloud Content Delivery Network \(CDN\) together to accelerate downloads of static objects. These static objects are frequently downloaded by a large number of users in the same region at the same time. You can configure your OSS bucket as the origin and use CDN to publish the data in the bucket to edge nodes. When a large number of end users access an object in your bucket repeatedly, they can obtain the object cached in edge nodes to improve the response speed.

A custom domain name is bound to the bucket. For more information, see [Bind custom domain names](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind custom domain names.md).

To activate Alibaba Cloud CDN, you must map your custom domain name to an accelerated domain name. All requests destined for your custom domain name are forwarded to the edge nodes.

**Note:** For other scenarios such as object upload and download of objects that are frequently accessed, we recommend that you use the transfer acceleration feature provided by OSS to accelerate the data transfer. For more information, see [Transfer acceleration](/intl.en-US/Developer Guide/Buckets/Transfer acceleration.md).

## Step 1: Bind an accelerated domain name

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  In the list of domain names, click **Not Configured** in the **Alibaba Cloud CDN** column corresponding to the domain name that is to be bound. The CDN console appears.

4.  In the left-side navigation pane, click Domain Names. On the Domain Names page, click Add Domain Name. In the Add Domain Name dialog box, configure parameters listed in the following table.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5767549951/p32707.png)

    |Parameter|Description|
    |:--------|:----------|
    |**Domain Name to Accelerate**|The value is automatically set to the custom domain name that is bound to the bucket. Do not modify the value.|
    |**Resource Groups**|Select Default Resource Group.|
    |**Business Type**|Content delivery varies with business type. Select the appropriate business type based on your stored content and the content usage.|
    |**Origin Info**|Click OSS Domain. Select an OSS domain name for which you want to accelerate the content delivery.|
    |**Port**|Select an access port.|
    |**Region**|Select the region for which to accelerate your business.|

5.  Click **Next**.

    After you add an accelerated domain name, a CNAME is generated. You must add the CNAME to the DNS of your DNS provider to enable CDN.


## Step 2: Add a CNAME record

You must add a CNAME record to the DNS of your DNS provider. Alibaba Cloud DNS is used in this example to describe the process of adding a CNAME record.

1.  Log on to the [CDN console](https://cdn.console.aliyun.com/overview).

2.  In the left-side navigation pane, click **Domain Names**. Copy the CNAME corresponding to the domain name to which you want to add a record.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5767549951/p34242.png)

3.  On the Manage DNS page, click **Configure** in the Actions column corresponding to the domain name.

4.  On the DNS Settings page, click **Add Record**. In the Add Record dialog box, configure parameters listed in the following table.

    |Parameter|Description|
    |:--------|:----------|
    |**Type**|Select **CNAME** from the drop-down list.|
    |**Host**|    -   To add a top-level domain such as `aliyun.com`, enter **@**.
    -   To add a second-level domain, enter the prefix of the second-level domain name. Example: If the domain is `abc.aliyun.com`, enter **abc**.
    -   To map all second-level domains to the accelerated domain name, enter **\*** as the host record. |
    |**ISP lINE**|Select the ISP line used to resolve the domain name. We recommend that you select **Default** to allow the system to select the optimal line.|
    |**Value**|Enter the CNAME that was copied in [Step 2](#step_uf2_kam_nht).|
    |**TTL**|Select the update interval of the record. In this example, keep the default value.|

5.  Click **Confirm**.

    **Note:**

    -   A new CNAME record takes effect immediately. A modified CNAME record requires up to 72 hours to take effect.
    -   It takes about 10 minutes until the CNAME status is updated. Therefore, even if you have configured a CNAME record, the **You must add the CNAME record.** message may be displayed on the Domain Names tab in the Alibaba Cloud CDN console. In this case, you can ignore the message.

## Step 3: Enable auto CDN cache update

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  Find the target domain name that is bound to an accelerated domain name and turn on **Auto CDN Cache Update** for this domain name.

    The auto CDN cache update feature can be triggered by specified operations. You can [submit a ticket](https://workorder-intl.console.aliyun.com/#/ticket/createIndex) to apply for trial. After you are authorized to configure auto CDN cache update for specified operations, click **Supported Operations** in the Auto CDN Cache Update column corresponding to the domain name, and select the operations to which auto CDN cache update can be applied. Click **OK**.

    The following types of operations are supported.

    |Parameter|Description|
    |---------|-----------|
    |PutObject|Uploads an object by calling the PutObject operation. For more information, see [Simple upload](/intl.en-US/Developer Guide/Objects/Upload files/Simple upload.md).|
    |PostObject|Uploads an object by calling the PostObject operation. For more information, see [Form upload](/intl.en-US/Developer Guide/Objects/Upload files/Form upload.md).|
    |CopyObject|Modifies an object by calling the CopyObject operation. For more information, see [Copy objects](/intl.en-US/Developer Guide/Objects/Manage files/Copy objects.md).|
    |AppendObject|Uploads an object by calling the AppendObject operation. For more information, see [Append upload](/intl.en-US/Developer Guide/Objects/Upload files/Append upload.md).|
    |CompleteMultipartUpload|Uploads an object by using multipart upload or resumable upload. For more information, see [Multipart upload and resumable upload](/intl.en-US/Developer Guide/Objects/Upload files/Multipart upload and resumable upload.md).|
    |DeleteObject|Deletes a single object by calling the DeleteObject operation. For more information, see [Delete objects](/intl.en-US/Developer Guide/Objects/Manage files/Delete objects.md).|
    |DeleteObjects|Deletes multiple objects by calling the DeleteMultipleObjects operation. For more information, see [Delete objects](/intl.en-US/Developer Guide/Objects/Manage files/Delete objects.md).|
    |PutObjectACL|Modifies the ACL of an object by calling the PutObjectACL operation. For more information, see [Object ACL](/intl.en-US/Developer Guide/Data security/Access and control/ACL.md).|

    Objects that are deleted or whose storage classes are converted to other storage classes based on lifecycle rules are not updated to the CDN cache.


**Note:**

-   After you unbind a custom domain name from a bucket, you cannot configure auto CDN cache update in the OSS console. Instead, you must configure this feature in the Alibaba Cloud CDN console. For more information, see [Configure the refresh and prefetch features](/intl.en-US/Service Management/Refresh and prefetch/Configure the refresh and prefetch features.md).
-   You can access the data updated on the CDN cache by using the URL in the following format: `CNAME/object`. However, you can not access the updated data when you use URLs that contain request parameters, such as the parameters for IMG and video snapshots. For example, the accelerated domain name bound to a bucket is `test.aliyun.com` and the a.jpg object in the bucket is updated. You can access the data updated on the CDN cache by using `test.aliyun.com/a.jpg`. If you use `test.aliyun.com/a.jpg? x-oss-process=image/w_100`, you may access the object that has not been updated.

## FAQ: What do I do if AccessDenied is returned when I access a website?

After you bind a custom domain name, you can add the specific path to the custom domain name to access OSS resources. Example: http://mydomain.cn/test/1.jpg. If you directly access a custom domain name such as http://mydomain.cn, AccessDenied is returned.

## What to do next

-   To use an accelerated domain name to access OSS resources over HTTPS, you must host your certificate in OSS. For more information, see [Upload an SSL certificate](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Host SSL certificates.md).
-   To implement cross-origin access \(CORS\) when Alibaba Cloud CDN is activated , you must configure CORS rules in the CDN console. For more information, see [Configure CORS for Alibaba Cloud CDN](https://www.alibabacloud.com/help/zh/faq-detail/40183.htm).

