# Bind accelerate endpoints

Alibaba Cloud OSS provides transfer acceleration to accelerate the upload and download of OSS objects over the Internet. Transfer acceleration provides an end-to-end acceleration solution by combining smart scheduling, protocol stack tuning, optimal route selection, and transfer algorithm optimization with OSS server-side configurations. This topic describes how to implement OSS transfer acceleration based on a custom domain name.

-   Transfer acceleration is enabled. For more information, see [Enable transfer acceleration](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Enable transfer acceleration.md).
-   If the custom domain name is bound to a bucket that resides in mainland China, you must apply for an ICP filing at the Ministry of Industry and Information Technology \(MIIT\).

After transfer acceleration is enabled, two public endpoints are generated for the bucket.

-   Global accelerate endpoint: The format is BucketName.oss-accelerate.aliyuncs.com. Transfer acceleration access points are distributed all over the world. You can use this endpoint to accelerate access to buckets all over the world.
-   Accelerate endpoint outside mainland China: The format is BucketName.oss-accelerate-overseas.aliyuncs.com. Transfer acceleration access points are distributed across the world except mainland China. You can use this endpoint to accelerate access to buckets that are located in regions outside mainland China. Buckets within mainland China cannot be accessed.

When you use accelerate endpoints to access OSS, data transfer is accelerated. To implement transfer acceleration based on a custom domain name, bind the domain name to a bucket and configure the CNAME record to map the custom domain name to an accelerate endpoint. You can select an accelerate endpoint to which a custom domain name maps.

|Bucket location|ICP filing|Accelerate endpoint for CNAME|
|---------------|----------|-----------------------------|
|All countries and regions|The ICP filing for the custom domain name is complete.|Global accelerate endpoint|
|China \(Hong Kong\) and other regions outside mainland China|The ICP filing for the custom domain name is not complete.|Accelerate endpoint outside mainland China|
|Mainland China|The ICP filing for the custom domain name is not complete.|N/A|

**Note:**

-   If your custom domain name is not filed at MIIT but mapped to a global accelerate endpoint, requests sent to access points in mainland China are blocked. Users in mainland China cannot access this global accelerate endpoint.
-   When you use the accelerate endpoint outside mainland China to access OSS in mainland China, the access speed may be slower than that of the global accelerate endpoint. We recommend that you map your custom domain name to the global accelerate endpoint. You can also use the global accelerate endpoint directly without binding the custom domain name.
-   Additional fees are charged based on the traffic and region where transfer acceleration is used. For more information, see [Transfer acceleration fees](/intl.en-US/Pricing/Billing items and methods/Transfer acceleration fees.md).

## Step 1: Bind a custom domain name to the bucket

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  Click **Bind Custom Domain Name**. In the Bind Custom Domain Name dialog box, set **Custom Domain Name**.

    **Note:**

    -   Ensure that **Add CNAME Record Automatically** is turned off.
    -   If a domain name conflict message appears, the domain name is already bound to another bucket. To resolve this problem, use one of the following methods:
        -   Use another domain name.
        -   Verify the ownership of the domain name and bind the domain name to your bucket. This operation unbinds the domain name from the previous bucket. For more information, see [Verify the ownership of a domain name](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind custom domain names.md).
4.  Click **Submit**.


## Step 2: Configure the CNAME record

You must add a CNAME record to the DNS of your DNS provider. Alibaba Cloud DNS is used in this example to describe the process of adding a CNAME record.

1.  On the Manage DNS page that appears, click **Configure** in the Actions column corresponding to a domain name.

2.  On the DNS Settings page, click **Add Record**. In the Add Record dialog box, configure parameters listed in the following table.

    |Parameter|Description|
    |:--------|:----------|
    |**Type**|Select **CNAME** from the drop-down list.|
    |**Host**|Enter the host record based on the prefix of the domain name.     -   To add a top-level domain, enter **@**. For example, if the domain name is aliyun.com, enter **@** as the host record.
    -   To add a second-level domain, enter the prefix of the second-level domain. For example, if the domain name is abc.aliyun.com, enter **abc** as the host record.
    -   To map all second-level domains to the accelerate endpoint, enter **\*** as the host record. |
    |**ISP Line**|Select the ISP line used to resolve the domain name. We recommend that you select Default to allow the system to select the optimal line.|
    |**Value**|Enter a global accelerate endpoint or accelerate endpoint outside mainland China. For more information, see [Scenarios](#p_zvp_ch2_lo2).|
    |**TTL**|Select the update interval of the record. In this example, keep the default value.|

3.  Click **Confirm**.

    **Note:** A new CNAME record takes effect immediately. A modified CNAME record requires up to 72 hours to take effect.


## Step 3: Verify whether the configurations take effect

After a CNAME record is configured, the time period required for the record to take effect varies depending on the DNS providers. You can use the following methods to verify whether the configurations take effect:

-   In Windows:

    Open the cmd.exe program. Run the nslookup command to resolve your custom domain name. If the request is redirected to the accelerate endpoint, the CNAME is in effect.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2767549951/p70082.png)

-   In Linux:

    Run the dig command to resolve your custom domain name. If the request is redirected to the accelerate endpoint, the CNAME is in effect.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2767549951/p70084.png)


