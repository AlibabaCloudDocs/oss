# Use ECS instances that run Ubuntu to configure reverse proxy for access to Alibaba Cloud OSS

IP addresses used to access a bucket dynamically change. You can use ECS instances to configure reverse proxy for access to OSS. This way, a static IP address can be used to access the bucket.

OSS uses Restful APIs to provide services. Users use OSS domain names or custom domain names to access OSS. In some scenarios, users need to use a static IP address to access OSS.

-   For security reasons, some enterprises need to configure outbound rules to specify that internal employees and business systems can access only the specified public IP address. However, the IP addresses used to access a bucket in OSS dynamically change. Enterprises need to frequently modify firewall rules.
-   Due to the architecture limits of Alibaba Fiance Cloud, internal network-specific buckets in Alibaba Finance Cloud can be accessed only within Alibaba Finance Cloud. These buckets cannot be accessed over the Internet.

To resolve these problems, you can use ECS instances to configure reverse proxy for access to OSS.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9454449951/p38572.png)

## Procedure

1.  Create an ECS instance that runs Ubuntu. Ensure that the instance and the specified bucket are located within the same region.

    The Ubuntu 18.04 \(64-bit\) system is used in this example. For more information, see [Create an ECS instance]().

2.  Log on to the ECS instance as the root user. Update the APT sources.

    ```
    root@test:~# apt-get update
    ```

3.  Install NGINX.

    ```
    root@test:~# apt-get install nginx
    ```

    **Note:** Default locations of NGINX:

    ```
     /usr/sbin/nginx       Stores the NGINX executable. 
     /etc/nginx            Stores configuration files. 
     /usr/share/nginx      Stores static files. 
     /var/log/nginx        Stores log files.
    ```

4.  Open the configuration file of NGINX.

    ```
    root@test:~# vi /etc/nginx/nginx.conf
    ```

5.  Add the following content to the http context of the configuration file:

    ```
    server {
            listen 80;
            server_name 47. **. **.73; 
    
            location / {
                proxy_pass http://bucketname.oss-cn-beijing-internal.aliyuncs.com; 
                #proxy_set_header Host $host; 
         }  
    }
    ```

    -   server\_name: the IP address used to provide the reverse proxy service, which is the public IP address of the ECS instance.
    -   proxy\_pass: the endpoint for redirection.
        -   When the ECS instance and the requested bucket are located within the same region, specify the internal endpoint of the bucket. For more information about endpoints, see [OSS domain names](/intl.en-US/Developer Guide/Endpoint/OSS domain names.md).
        -   When the ECS instance and the requested bucket are located within different regions, specify the public endpoint of the bucket.
        -   For security reasons, when an OSS domain name is used to access an OSS image or web page object by using a browser, the object is directly downloaded. To preview the object, use a custom domain name that is bound to the bucket. Add the custom domain name for this parameter. For more information about how to bind a custom domain name, see [Bind custom domain names](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind custom domain names.md).
    -   proxy\_set\_header Host $host: If you add this parameter, the host value is replaced with the IP address of the ECS instance when NGINX sends a request to OSS. You must add this parameter in the following situations:
        -   Signature errors.
        -   Your custom domain name is resolved to the public IP address of the ECS instance, and your user must browse image or web page objects in a bucket. You can bind a custom domain name to the bucket for which reverse proxy is configured. You do not need to configure CNAME. You can set proxy\_pass to the internal or public endpoint of the bucket. For more information about how to bind a custom domain name, see [Bind custom domain names](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind custom domain names.md).
    **Note:**

    -   This topic uses the demo environment. For data security reasons, we recommend that you configure the https context. For more information, see [Configure HTTPS for your custom domain name in OSS by using reverse proxy](https://www.alibabacloud.com/help/zh/faq-detail/39544.htm).
    -   You can configure reverse proxy for only one bucket if you use this configuration method.
6.  Go to the folder of the NGINX executable. Start NGINX.

    ```
    root@test:~# cd /usr/sbin/
    root@test:~# ./nginx
    ```

7.  Open TCP port 80 of the ECS instance.

    By default, NGINX uses TCP port 80. You must enable TCP port 80 when you configure a security group. For more information, see [Add security group rules](/intl.en-US/Security/Security groups/Add security group rules.md).

8.  Add the object path to the public IP address of the ECS instance to access OSS resources.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8454449951/p38580.png)


## References

-   [Use ECS instances that run Windows to configure reverse proxy for access to Alibaba Cloud OSS](/intl.en-US/Best Practices/Use ECS instances to configure reverse proxy for access to OSS/Use ECS instances that run Windows to configure reverse proxy for access to Alibaba
         Cloud OSS.md)
-   [Use ECS instances that run CentOS to configure reverse proxy for access to Alibaba Cloud OSS](/intl.en-US/Best Practices/Use ECS instances to configure reverse proxy for access to OSS/Use ECS instances that run CentOS to configure reverse proxy for access to Alibaba
         Cloud OSS.md)

