# Use an ECS instance that runs Windows to configure a reverse proxy for access to OSS

The IP address used to access an Object Storage Service \(OSS\) bucket dynamically changes. You can configure a reverse proxy on an Elastic Compute Service \(ECS\) instance to access the OSS bucket by using a static IP address.

You can access a bucket by using the default endpoint of the bucket or custom domain names that are mapped to the bucket. However, you may need to use a static IP address to access OSS in some scenarios.

-   For security reasons, some enterprises must configure outbound rules to specify that internal employees and business systems can access only the specified public IP addresses. However, the IP addresses used to access a bucket in OSS dynamically change. In this case, enterprises must frequently modify firewall rules.
-   Limited by the network architecture of Alibaba Finance Cloud, internal network-specific buckets in Alibaba Finance Cloud can be accessed only by requests from Alibaba Finance Cloud.

To resolve these issues, you can use an ECS instance to configure a reverse proxy for access to OSS.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9454449951/p38572.png)

## Procedure

1.  Create an ECS instance that runs Windows Server. Make sure that the instance and the bucket you want to access are located within the same region.

    The Windows Server 2019 Datacenter edition 64-bit system is used in this example. For more information, see [Quick start for Windows instances](/intl.en-US/Quick Start/Create an instance in the ECS console (detailed version)/Quick start for Windows instances.md).

2.  Download NGINX and decompress the package.

    NGINX-1.19.2 is used in this example. For more information about the downloading URLs of NGINX, visit [Nginx](http://nginx.org/en/download.html).

3.  Modify the configuration file.

    1.  Go to the conf directory and open the nginx.conf file by using a notepad.

    2.  Modify the content of the configuration file.

        ```
        worker_processes  1;
        events {
            worker_connections  1024;
        }
        
        http {
            include       mime.types;
            default_type  application/octet-stream;
        
            keepalive_timeout  65;
            server {
                listen       80;
                server_name  47.**.**.43;
        
                error_page   500 502 503 504  /50x.html;
                location = /50x.html {
                    root   html;
                }
        
               location / {
                    proxy_pass  http://bucketname.oss-cn-hangzhou-internal.aliyuncs.com;
                    #proxy_set_header Head $host;
                }
           }
        }
        ```

        -   server\_name: the IP address used to provide the reverse proxy service, which is the public IP address of the ECS instance.
        -   proxy\_pass: the endpoint for redirection.
            -   When the ECS instance and the bucket that you want to access are located within the same region, specify the internal endpoint of the bucket. For more information about endpoints, see [OSS domain names](/intl.en-US/Developer Guide/Endpoint/OSS domain names.md).
            -   When the ECS instance and the bucket that you want to access are located within different regions, specify the public endpoint of the bucket.
            -   For security reasons, when you access an image or web page object in a bucket by using the default domain name of the bucket in a browser, the object is downloaded. To preview the object in your browser, map a custom domain name to the bucket in which the object is stored and add the custom domain name to the value of proxy\_pass. For more information about how to map a custom domain name to a bucket, see [Map custom domain names](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Map custom domain names.md).
        -   proxy\_set\_header Head $host: If you add this parameter, the host value is replaced with the IP address of the ECS instance when NGINX sends a request to OSS. You must add this parameter in the following scenarios:
            -   Signature errors occur.
            -   The custom domain name that is mapped to the bucket is resolved to the public IP address of the ECS instance. You must preview image or web page objects in the bucket by using a browser. You can map the custom domain name to the bucket for which a reverse proxy is configured without adding a CNAME record for the custom domain name. In this case, you can set proxy\_pass to the internal or public endpoint of the bucket. For more information about how to map a custom domain name to a bucket, see [Map custom domain names](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Map custom domain names.md).
        **Note:**

        -   A demo environment is used in this topic as an example. For data security reasons, we recommend that you configure the https context.
        -   You can configure a reverse proxy for only one bucket if you use this configuration method.
4.  Go to the directory in which the NGINX executable file is located. Double-click nginx.exe to start NGINX.

5.  Enable TCP port 80 of the ECS instance.

    By default, NGINX uses TCP port 80. Therefore, you must enable TCP port 80 when you configure a security group for the ECS instance. For more information, see [Add security group rules](/intl.en-US/Security/Security groups/Add security group rules.md).

6.  Add the object path to the public IP address of the ECS instance to access OSS resources.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9454449951/p38588.png)


## References

-   [Use an ECS instance that runs Ubuntu to configure a reverse proxy for access to OSS](/intl.en-US/Best Practices/Use ECS instances to configure reverse proxy for access to OSS/Use an ECS instance that runs Ubuntu to configure a reverse proxy for access to OSS.md)
-   [Use an ECS instance that runs CentOS to configure a reverse proxy for access to OSS](/intl.en-US/Best Practices/Use ECS instances to configure reverse proxy for access to OSS/Use an ECS instance that runs CentOS to configure a reverse proxy for access to OSS.md)

