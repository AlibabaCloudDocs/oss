# Initialization

This topic describes how to initialize OSS SDK for Python.

OSS SDK for Python allows you to use oss2.Serviceand oss2.Bucketto perform most operations.

-   oss2.oss2.Service is used to list buckets.
-   oss2.oss2.Bucket is used to upload, download, and delete objects as well as configure buckets.

You must specify the endpoint when you initiate the two classes. Custom domain names cannot be used to accessthe oss2.Service class. For more information about endpoints, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md) and [Bind a custom domain name](/intl.en-US/Developer Guide/Buckets/Bind custom domain names.md).

## Initialize the oss2.Serviceclass

For more information, see [List buckets](/intl.en-US/SDK Reference/Python/Buckets/List buckets.md).

## Initialize the oss2.Bucketclass

-   Use the default domain name of a bucket to initialize the class

    The following code provides an example on how to use the default domain name of a bucket to initialize the oss2.Bucket class:

    ```
    # -*- coding: utf-8 -*-
    import oss2
    
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
    auth = oss2.Auth('yourAccessKeyId', 'yourAccessKeySecret')
    
    # Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
    endpoint = 'yourEndpoint'
    
    # Specify the bucket name. 
    bucket = oss2.Bucket(auth, endpoint, 'examplebucket')                    
    ```

-   Use a custom domain name that is mapped to a bucket to initialize the class

    The following code provides an example on how to use a custom domain name that is mapped to a bucket to initialize the oss2.Bucket class:

    ```
    # -*- coding: utf-8 -*-
    import oss2
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
    auth = oss2.Auth('yourAccessKeyId', 'yourAccessKeySecret')
    
    # In this example, my-domain.com is used as the custom domain name. 
    cname = 'http://my-domain.com'
    
    # Specify the bucket name. Set is_cname to True to enable CNAME. If CNAME is enabled, custom domain names can be mapped to the bucket. 
    bucket = oss2.Bucket(auth, cname, 'examplebucket', is_cname=True)                    
    ```

-   Configure the connection timeout period

    The following code provides an example on how to configure the connection timeout period:

    ```
    # -*- coding: utf-8 -*-
    import oss2
    
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
    auth = oss2.Auth('yourAccessKeyId', 'yourAccessKeySecret')
    
    // Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
    endpoint = 'yourEndpoint'
    
    # Specify the bucket name and set the connection timeout period to 30 seconds. 
    bucket = oss2.Bucket(auth, endpoint, 'examplebucket', connect_timeout=30)                    
    ```

-   Disable CRC

    By default, cyclic redundancy check \(CRC\) is enabled to ensure data integrity when you upload or download objects.

    **Warning:** We recommend that you do not disable CRC. If you disable CRC, data integrity may be affected during object upload and download.

    The following code provides an example on how to disable CRC:

    ```
    # -*- coding: utf-8 -*-
    import oss2
    
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
    auth = oss2.Auth('yourAccessKeyId', 'yourAccessKeySecret')
    
    # Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
    endpoint = 'yourEndpoint'
    
    # Specify the bucket name and set enable_crc to False to disable CRC. 
    bucket = oss2.Bucket(auth, endpoint, 'examplebucket', enable_crc=False)                   
    ```


