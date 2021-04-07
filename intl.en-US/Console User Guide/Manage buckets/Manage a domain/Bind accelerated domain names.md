# Bind accelerated domain names

You can use OSS and Alibaba Cloud Content Delivery Network \(CDN\) together to cache objects stored in OSS to the edge nodes of CDN. When a large number of users repeatedly access an object in your bucket, they can obtain a cached version of the object from edge nodes to improve the response speed.

An ICP filing is applied at the Ministry of Industry and Information Technology \(MIIT\) for the domain name bound to your bucket if the bucket is in mainland China regions.[https://beian.aliyun.com/order/selfBaIndex.htm](https://beian.aliyun.com/order/selfBaIndex.htm)

When you use OSS together with CDN, CDN outbound traffic fees, CDN back-to-origin outbound traffic fees, and request fees are incurred.

**Note:** For other scenarios such as object upload and download of objects that are frequently accessed, we recommend that you use the transfer acceleration feature provided by OSS to accelerate data transfer. For more information, see [Transfer acceleration](/intl.en-US/Developer Guide/Buckets/Transfer acceleration.md).

## Procedure

1.  Bind a custom domain name to a bucket.

    1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

    2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket to which you want to bind the custom domain name.

    3.  In the left-side navigation pane, choose **Transmission** \> **Domain Names**.

    4.  Click **Bind Custom Domain Name**. In the Bind Custom Domain Name panel, enter the domain name that you want to bind to the bucket in the **Custom Domain Name** field.

        Do not turn on the **Add CNAME Record Automatically** switch.

        If a domain name conflict message appears, the domain name is already bound to another bucket. To resolve this issue, you can use another domain name or verify the ownership of the domain name and forcibly bind the domain name to the bucket. This operation unbinds the domain name from the previous bucket. For more information, see [Verify the ownership of a domain name](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind custom domain names.md).

    5.  Click **Submit**.

2.  Configure CDN acceleration

    1.  On the Domain Names tab, click **Not Configured** in the Actions column corresponding to the domain name for which you want to configure CDN acceleration.

    2.  On the Add Domain Name page, configure the parameters described in the following table.

        |Parameter|Description|
        |:--------|:----------|
        |**Domain Name to Accelerate**|Use the default value.|
        |**Resource Groups**|Select Default Resource Group.|
        |**Business Type**|The business type determines how content is delivered. Select the appropriate business type based on your stored content and the content usage.|
        |**Origin Info**|Use the default value.|
        |**Port**|Select a port based on the protocol that you use to access OSS        -   If you use HTTP to access OSS, select **80**.
        -   If you use HTTPS to access OSS, select **443**. |
        |**Region**|Select **Region** based on your business. For example, if all of your users are located in mainland China, select **Mainland China Only** for **Region**.|

    3.  Click **Next** and then click **Return to Domain Names**.

    4.  In the domain name list, record the CNAME value of the domain name for which you want to configure CDN acceleration.

3.  Add a CNAME record

    If the domain name is not hosted by Alibaba Cloud, you must add a CNAME record to the DNS of your DNS provider.Alibaba Cloud DNS is used in this example to describe how to manually add a CNAME record for a domain name.

    1.  Log on to [Alibaba Cloud DNS console](https://dns.console.aliyun.com/#/dns/domainList).

    2.  Click **Manage DNS**. On the Manage DNS page, click **Configure** in the Actions column corresponding to the domain name to which you want to add a CNAME record.

    3.  On the DNS Settings page, click **Add Record**. In the Add Record panel, configure the parameters described in the following table.

        |Parameter|Description|
        |:--------|:----------|
        |**Type**|Select the type of the record. In this example, select **CNAME**.|
        |**Host**|Enter the host record based on the prefix of the domain name.         -   To add a top-level domain such as `aliyun.com`, enter **@**.
        -   To add a second-level domain, enter the prefix of the second-level domain name. Example: If the domain is `abc.aliyun.com`, enter **abc**.
        -   To map all second-level domains to the public endpoint of the bucket, enter **\***. |
        |**ISP Line**|Select the ISP line used to resolve the domain name. We recommend that you select **Default** to allow Alibaba Cloud DNS to select the optimal line.|
        |**Value**|Enter the CNAME that is recorded in [Step 2](#substep_gb2_ejz_v7n).|
        |**TTL**|Select the update interval of the record. In this example, keep the default value.|

    4.  Click **OK**.

        A new CNAME record takes effect immediately. A modified CNAME record may take up to 72 hours to take effect.

4.  Enable auto CDN cache update

    On the **Domain Names** tab, turn on the **Auto CDN Cache Update** switch.

    If you want that the auto CDN cache update feature can be triggered by specific operations,[submit a ticket](https://workorder-intl.console.aliyun.com/#/ticket/createIndex). After you are authorized to configure auto CDN cache update for specific operations, click **Supported Operations** in the Auto CDN Cache Update column corresponding to the domain name, and select the operations by which auto CDN cache update can be triggered. The following table describes the supported operations.

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

    Objects that are deleted or whose storage classes are converted based on lifecycle rules are not updated to the CDN cache.

    **Note:** You can access the data updated on the CDN cache by using the URL in the following format: `CNAME/ObjectName`. However, the updated data cannot be accessed by using URLs that contain request parameters, such as the parameters for IMG and video snapshots. For example, the accelerated domain name bound to a bucket is `example.com` and the a.jpg object in the root folder of the bucket is updated. You can access the object updated on the CDN cache by using `example/a.jpg`. If you use `example.com/a.jpg?x-oss-process=image/w_100` to access the object, you may access the earlier version of the object that is not updated.


## FAQ

-   What do I do if AccessDenied is returned when I access an accelerated domain name that is bound to a bucket?

    After you bind a custom domain name, you can add the specific path to the custom domain name to access OSS resources. Example: `http://example.com/test/1.jpg`. If you directly access a custom domain name such as `http://example.com`, AccessDenied is returned.

-   What do I do if the status of the domain name in the Alibaba Cloud CDN column alternates between configured and not configured when I refresh the Domain Names tab in the OSS console after CDN is configured for the domain name?

    The configurations of Alibaba Cloud CDN for a domain name take 10 minutes to take effect. Therefore, the status of Alibaba Cloud CDN may alternate on the Domain Names tab. We recommend that you wait 10 minutes after you configure CDN for the domain name and then refresh the tab to check the status.


## References

-   To use an accelerated domain name to access OSS resources over HTTPS, you must host your certificate in OSS. For more information, see [Host SSL certificates](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Host SSL certificates.md).
-   To implement cross-origin resource sharing \(CORS\) when Alibaba Cloud CDN is activated, you must configure CORS rules in the CDN console. For more information, see[Configure CORS for Alibaba Cloud CDN](https://www.alibabacloud.com/help/zh/faq-detail/40183.htm).

