# Host SSL certificates

To use a custom domain name to access OSS resources over HTTPS, you must purchase an SSL certificate. You can purchase an SSL certificate from any certificate authority \(CA\) or from Alibaba Cloud SSL Certificates Service and host your certificate in OSS.

Host your certificate in either one of the following methods based on your actual condition:

-   If you bind a custom domain name to your bucket and do not enable CDN, see [Host a certificate for a custom domain name](#section_cu6_eyc_ek6).
-   If you bind a custom domain name to your bucket and enable CDN, see [Host a certificate for an accelerated domain name](#section_ffq_uwi_ajl).

## Host a certificate for a custom domain name

If you have bound a custom domain name to your bucket as instructed in [Bind a custom domain name](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Map custom domain names.md), perform the following steps to host your certificate in the OSS console:

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  Choose **Transmission** \> **Domain Names**

4.  On the Domain Names tab that appears, click **Upload Certificate** in the Actions column corresponding to the domain name for which you want to upload an SSL certificate.

5.  In the Upload Certificate dialog box that appears, enter the public key and private key used in your certificate.

    After you obtain the SSL certificate, you can view the public key and private key information from the following files:

    -   The file with the suffix of .pem or .crt in the certificate contains the public key in the following format:

        ```
        -----BEGIN CERTIFICATE-----
        ......
        -----END CERTIFICATE----- 
        ```

    -   The file with the suffix of .key in the certificate contains the private key in the following format:

        ```
        -----BEGIN RSA PRIVATE KEY-----
        ......
        -----END RSA PRIVATE KEY-----
        ```

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6767549951/p32024.png)

    **Note:** You can select **Show PEM Encoding Example** to view examples of the public key and private key. For more information about the certificate format, see [Overview of certificate formats](/intl.en-US/Domain Management/HTTPS/Certificate formats.md).

6.  Click **Upload**.


## Host a certificate for an accelerated domain name

If you have bound an accelerated domain name to your bucket as instructed in [Bind an accelerated domain name](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind accelerated domain names.md), perform the following steps to host your certificate in the CDN console:

1.  Log on to the [Alibaba Cloud CDN console](https://cdn.console.aliyun.com/domain/list).

2.  Click **Domain Names**. On the page that appears, click **Manage** in the Actions column corresponding to the domain name for which you want to upload an SSL certificate.

3.  Choose **HTTPS** \> **Modify**.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6767549951/p32575.png)

4.  In the Modify HTTPS Settings dialog box that appears, turn on **HTTPS Secure Acceleration**.

5.  Set **Certificate Type**.

    You can select **Alibaba Cloud Certificate**, **Custom**, or **Free Certificate**. Your certificate files must be in the PEM format.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6767549951/p32579.png)

    -   **Alibaba Cloud Certificate**: Select your SSL certificate.
    -   **Custom**: You must set the certificate name, and then upload the certificate content and private key. The uploaded certificate is stored in Alibaba Cloud SSL Certificates Service. You can view the certificate in the [SSL Certificates Service console](https://yundun.console.aliyun.com/?spm=5176.2020520110.all.12.16df56a1u1IhI6&p=cas#/cas/home).
    -   **Free Certificate**: Use the free Digicert DV SSL certificate provided by Alibaba Cloud. Free certificates are used only for the HTTPS Secure Acceleration service of CDN. Therefore, you cannot manage free certificates or view their public and private keys in the Alibaba Cloud Security console. A free certificate takes effect after about 10 minutes.
6.  Click **OK**.

    A purchased certificate takes effect after about an hour. You can access OSS resources over HTTPS. If https is displayed in green in the address bar of the browser, the SSL certificate is in effect.


