# Map custom domain names

After you upload objects to a bucket, OSS automatically generates URLs that include the public endpoint of the bucket for the uploaded objects. You can use these URLs to access the objects. If you want to access the objects by using custom domain names \(CNAMEs\), you must map the custom domain names to the bucket in which the objects are stored.

**Note:** For more information about how to obtain the URL of an object, see [How do I obtain the URL of an uploaded object?](/intl.en-US/Developer Guide/Objects/FAQ/How do I obtain the URL of an uploaded object?.md).

## Procedure

1.  Map a custom domain name to a bucket.

    1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

    2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket to which you want to map the custom domain name.

    3.  In the left-side navigation pane, choose **Transmission** \> **Domain Names**.

    4.  Click **Bind Custom Domain Name**.

    5.  In the Bind Custom Domain Name panel, enter the domain name that you want to map in the **Custom Domain Name** field.

        Domain names that contain wildcards are not supported. Example: `*.example.com`.

        If a domain name conflict message appears, the domain name is already mapped to another bucket. To resolve this issue, you can use another domain name or verify the ownership of the domain name and forcibly map the domain name to the bucket. This operation removes the mapping between the domain name and the previous bucket. For more information, see [Verify the ownership of a domain name](#section_n3e_bp4_l5n).

2.  Add a CNAME record.

    -   If the domain name is managed by your Alibaba Cloud account, perform the following steps to automatically add a CNAME record:
        1.  In the Bind Custom Domain Name panel, turn on **Add CNAME Record Automatically**.

            **Note:** If the domain name has an existing CNAME record, the existing record is updated to the new CNAME record.

        2.  Click **Submit**.
    -   If the domain name is not managed by your Alibaba Cloud account, manually add a CNAME record.

        If the domain name is not hosted by Alibaba Cloud, you must add a CNAME record to the Domain Name System \(DNS\) of your DNS provider.

        The following example shows how to use Alibaba Cloud DNS to manually add a CNAME record for a domain name that does not belong to an Alibaba Cloud account.

        1.  Log on to the [Alibaba Cloud DNS console](https://dns.console.aliyun.com/#/dns/domainList).
        2.  On the Manage DNS page, click **Configure** in the Actions column that corresponds to the domain name to which you want to add a CNAME record.
        3.  On the DNS Settings page, click **Add Record**. In the Add Record panel, configure parameters listed in the following table.

            |Parameter|Description|
            |:--------|:----------|
            |**Type**|Select the type of the record. In this example, select **CNAME**.|
            |**Host**|Enter the host record based on the prefix of the domain name.             -   To add a top-level domain such as `aliyun.com`, enter **@**.
            -   To add a second-level domain, enter the prefix of the second-level domain. Example: If the domain is `abc.aliyun.com`, enter **abc**.
            -   To map all second-level domains to the public endpoint of the bucket, enter **\***. |
            |**ISP Line**|Select the ISP line used to resolve the domain name. We recommend that you select **Default** to allow the system to select the optimal line.|
            |**Value**|Enter the public endpoint of the bucket. The public endpoint of a bucket is in the `BucketName.Endpoint` format. For example, the public endpoint of the China \(Hangzhou\) region is `oss-cn-hangzhou.aliyuncs.com`. If you create a bucket named examplebucket in the China \(Hangzhou\) region, the public endpoint of the bucket is `examplebucket.oss-cn-hangzhou.aliyuncs.com`.|
            |**TTL**|Select the update interval of the record. The default value is used in this example.|

        4.  Click **Confirm**.

            A new CNAME record takes effect immediately. A modified CNAME record requires up to 72 hours to take effect.


## Verify the CNAME status

You can run the ping or lookup command to check the status of an added CNAME. If the request is redirected to `*.oss-cn-*.aliyuncs.com`, the CNAME is in effect.

## Verify the ownership of a domain name

If a domain name conflict message appears, you can verify the ownership of the domain name and forcibly map the domain name to the bucket.

1.  Click **Obtain TXT**.

    OSS randomly generates a token for the domain name, which includes the following fields: **Domain**, **Host**, and **Value**. Store the token information in a secure location.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0767549951/p32020.png)

2.  Add a CNAME record to the DNS of your DNS provider. Enter the recorded values of the **Host** and **Value** fields and keep the default settings of other parameters.

    For more information about how to add a CNAME record, see [Manually add a CNAME record](#li_v8f_hvb_9xt).

3.  In the Bind Custom Domain Name panel, read and select **I have added the TXT record. Continue submission**.

    If your configuration is correct, OSS maps the custom domain name to the bucket.


## Remove a domain name mapping

You can remove the mapping of a custom domain name from a bucket when you no longer need to use the custom domain name.

1.  On the management page of the bucket from which you want to remove the custom domain name mapping, choose **Transmission** \> **Domain Names**.

2.  On the Domain Names tab, click **Manage Binding Configurations** in the Actions column that corresponds to the domain name mapping that you want to remove.

3.  In the Manage Binding Configurations panel, click **Unbind**. Then, click **OK**.


## References

-   To improve upload and download performance, you can map an accelerate endpoint to the bucket. For more information, see [Bind accelerate endpoints](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind accelerate endpoints.md).
-   To use HTTPS to access a custom domain name, you must upload your HTTPS certificate in the OSS console. For more information, see [Host SSL certificates](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Host SSL certificates.md).

