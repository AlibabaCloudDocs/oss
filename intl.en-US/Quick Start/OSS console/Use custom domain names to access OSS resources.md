# Use custom domain names to access OSS resources

After you upload objects to a bucket, OSS automatically generates URLs that include the public endpoint of the bucket for the uploaded objects. You can use these URLs to access the objects. If you want to access the objects by using custom domain names \(CNAMEs\), you must map the custom domain names to the bucket in which the objects are stored.

The ICP filing for the custom domain name is complete. For more information, see [Alibaba Cloud ICP Filing System]().

1.  Map a custom domain name to a bucket.

    1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

    2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket to which you want to map the custom domain name.

    3.  In the left-side navigation pane, choose **Transmission** \> **Domain Names**.

    4.  Click **Bind Custom Domain Name**.

    5.  In the Bind Custom Domain Name panel, enter the domain name that you want to map in the **Custom Domain Name** field.

        If a domain name conflict message appears, the domain name is already mapped to another bucket. To resolve this issue, you can use another domain name or verify the ownership of the domain name and forcibly map the domain name to the bucket. This operation removes the domain name from the previous bucket. For more information, see [Verify the ownership of a domain name](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Map custom domain names.md).

2.  Add a CNAME record

    -   If the domain name is managed by your Alibaba Cloud account, perform the following steps to automatically add a CNAME record.
        1.  In the Bind Custom Domain Name panel, turn on **Add CNAME Record Automatically**.

            **Note:** If the domain name has an existing CNAME record, it is updated to the new CNAME record.

        2.  Click **Submit**.
    -   If the domain name is not managed by the current Alibaba Cloud account, manually add a CNAME record.

        If your domain name is not managed by Alibaba Cloud DNS, you must configure the DNS of your DNS provider, such as Tencent Cloud DNS \(DNSPod\) or Xinnet. For more information, see [Add a CNAME record for a domain name](/intl.en-US/Quick Start/Add a CNAME record for a domain name.md).

        The following example shows how to use Alibaba Cloud DNS to manually add a CNAME record for a domain name that does not belong to an Alibaba Cloud account.

        1.  Log on to the [Alibaba Cloud DNS console](https://dns.console.aliyun.com/#/dns/domainList).
        2.  On the Manage DNS page, click **Configure** in the Actions column corresponding to the domain name to which you want to add a CNAME record.
        3.  On the DNS Settings page, click **Add Record**. In the Add Record dialog box, configure parameters listed in the following table.

            |Parameter|Description|
            |:--------|:----------|
            |**Type**|Select the type of the record. In this example, select **CNAME**.|
            |**Host**|Enter the host record based on the prefix of the domain name.             -   To add a top-level domain such as `aliyun.com`, enter **@**.
            -   To add a second-level domain, enter the prefix of the second-level domain name. Example: If the domain is `abc.aliyun.com`, enter **abc**.
            -   To map all second-level domains to the public endpoint of the bucket, enter **\***. |
            |**ISP Line**|Select the ISP line used to resolve the domain name. We recommend that you select **Default** to allow the system to select the optimal line.|
            |**Value**|Enter the public endpoint of the bucket. The public endpoint of a bucket is in the `BucketName.Endpoint` format. For example, the public endpoint of the China \(Hangzhou\) region is `oss-cn-hangzhou.aliyuncs.com`. If you create a bucket named examplebucket in the China \(Hangzhou\) region, the public endpoint of the bucket is `examplebucket.oss-cn-hangzhou.aliyuncs.com`.|
            |**TTL**|Select the update interval of the record. The default value is used in this example.|

        4.  Click **OK**.

            A new CNAME record takes effect immediately. A modified CNAME record requires up to 72 hours to take effect.

3.  Use custom domain names to access OSS resources

    After you map a custom domain name to a bucket, the URLs of the objects in the bucket are in the following format: `https://YourDomainName/ObjectName`.

    For example, you create a bucket named examplebucket in the China \(Hangzhou\) region, store an object named exampleobject.jpg in the bucket, and then bind the domain name `img.example.com` to the bucket. In this case, you can access exampleobject.jpg by using the following URL: `https://img.example.com/exampleobject.jpg`.


