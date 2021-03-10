# Initialization

OSSClient serves as the Java client to manage OSS resources such as buckets and objects. To use OSS SDK for Java to initiate a request, you must initialize an OSSClient instance and modify the default configuration items of ClientConfiguration.

## Create an OSSClient instance

To create an OSSClient instance, you must specify an endpoint. For more information about endpoints, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md) and [Bind a custom domain name](/intl.en-US/Developer Guide/Buckets/Bind custom domain names.md).

-   Create the OSSClient instance based on an OSS endpoint

    The following code provides an example on how to create an OSSClient instance based on an OSS endpoint:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you log on with the AccessKey pair of an Alibaba Cloud account because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();                    
    ```

    You can use one or more OSSClient instances for your project. In other words, you can use multiple OSSClient instances simultaneously.

-   Create an OSSClient instance based on a custom domain name

    The following code provides an example on how to create an OSSClient instance based on a custom domain name:

    ```
    String endpoint = "<yourEndpoint>";
    // Security risks may arise if you log on with the AccessKey pair of an Alibaba Cloud account because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    // Create a ClientConfiguration instance and modify the default parameters.
    ClientBuilderConfiguration conf = new ClientBuilderConfiguration();
    // Configure whether CNAME is enabled. CNAME is used to bind the custom domain name to the bucket.
    conf.setSupportCname(true);
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret, conf);
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();                    
    ```

    **Note:** ossClient.listBuckets is unavailable when a custom domain name is used.

-   Create an OSSClient instance based on Apsara Stack or a private region

    The following code provides an example on how to create an OSSClient instance based on Apsara Stack or a private region:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you log on with the AccessKey pair of an Alibaba Cloud account because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    // Create a ClientConfiguration instance and modify the default parameters based on your requirements.
    ClientBuilderConfiguration conf = new ClientBuilderConfiguration();
    // Disable CNAME.
    conf.setSupportCname(false);
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret, conf);
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();                    
    ```

-   Create an OSSClient instance based on an IP address

    The following code provides an example on how to create an OSSClient instance based on an IP address:

    ```
    // In some special scenarios such as a private region, an IP address is used as the endpoint. In such scenarios, specify the actual IP address based on your requirements.
    String endpoint = "https://10.10.10.10";
    // Security risks may arise if you log on with the AccessKey pair of an Alibaba Cloud account because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    // Create a ClientConfiguration instance.
    ClientBuilderConfiguration conf = new ClientBuilderConfiguration();
    // By default, access from a second-level domain is disabled. Enable access from a second-level domain. You must configure this parameter for OSS SDK for Java V2.1.2 or earlier, because versions later than 2.1.2 automatically detect IP addresses.
    conf.setSLDEnabled(true);
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret, conf);
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();                    
    ```

-   Create an OSSClient instance based on STS

    The following code provides an example on how to create an OSSClient instance based on Security Token Service \(STS\):

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you log on with the AccessKey pair of an Alibaba Cloud account because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String securityToken = "<yourSecurityToken>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret, securityToken);
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();                    
    ```

    For more information, see [Authorized access](/intl.en-US/SDK Reference/Java/Authorized access.md).

-   Create an OSSClient instance by using STSAssumeRole

    The following code provides an example on how to use STSAssumeRole to create an OSSClient:

    ```
    // Specify the region that STSAssumeRole can access. The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String region = "cn-hangzhou";
    // Specify the AccessKey ID and AccessKey secret of the RAM user.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    // Specify the ARN of STSAssumeRole, which indicates the ID of the role.
    String roleArn = "<yourRoleArn>";   
    // Create an STSAssumeRoleSessionCredentialsProvider instance.
    STSAssumeRoleSessionCredentialsProvider provider = CredentialsProviderFactory.newSTSAssumeRoleSessionCredentialsProvider(
    region, accessKeyId, accessKeySecret, roleArn);
    
    // Use STSAssumeRole to access the endpoint of OSS. The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "https://oss-cn-hangzhou.aliyuncs.com";  
    // Create a ClientConfiguration instance.
    ClientBuilderConfiguration conf = new ClientBuilderConfiguration();
    
     
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, provider, conf);
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
    ```

    For more information, see [Access OSS with a temporary access credential provided by STS](/intl.en-US/Developer Guide/Data security/Access and control/Access OSS with a temporary access credential provided by STS.md).

-   Create an OSSClient instance by using EcsRamRole

    You can access OSS from an ECS instance by using a RAM role bound to the instance. You can bind a RAM role to an ECS instance to access OSS from the instance by using STS temporary credentials. STS temporary credentials are automatically generated and updated. Applications can obtain the STS temporary credentials by using the instance metadata URL.

    The following code provides an example on how to use EcsRamRole to create an OSSClient:

    ```
    // Use the EcsRamRole role to access OSS.
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "https://oss-cn-hangzhou.aliyuncs.com";     
    
    // Use EcsRamRole to obtain the access credential. The following code shows how to obtain the access credential by using a role named ecs-ram-role.
    InstanceProfileCredentialsProvider provider = CredentialsProviderFactory.newInstanceProfileCredentialsProvider("ecs-ram-role");
    
    // Create an OSSClient instance.
     OSS ossClient = new OSSClientBuilder().build(endpoint, provider, new ClientBuilderConfiguration());
    
    // Shut down the OSSClient instance.
    ossClient.shutdown(); 
    ```


## Configure an OSSClient instance

ClientConfiguration is a configuration class of OSSClient. You can use ClientConfiguration to configure parameters such as host proxies, connection timeouts, and the maximum number of connections. The following table describes the parameters of this operation.

|Parameter|Description|Method|
|:--------|:----------|:-----|
|MaxConnections|Specifies the maximum number of HTTP connections that are allowed. Default value: 1024.|ClientConfiguration.setMaxConnections|
|SocketTimeout|Specifies the timeout period for data transmission at the socket layer. Unit: milliseconds. Default value: 50000.|ClientConfiguration.setSocketTimeout|
|ConnectionTimeout|The timeout period to establish a connection. Unit: milliseconds. Default value: 50000.|ClientConfiguration.setConnectionTimeout|
|ConnectionRequestTimeout|Specifies the timeout period for obtaining a connection from the connection pool. Unit: milliseconds. By default, this operation never times out.|ClientConfiguration.setConnectionRequestTimeout|
|IdleConnectionTime|The timeout period for idle connections. The connection is closed when the timeout period ends. Default value: 60000.|ClientConfiguration.setIdleConnectionTime|
|MaxErrorRetry|The maximum number of retry attempts that are allowed when a request error occurs. Default value: 3.|ClientConfiguration.setMaxErrorRetry|
|SupportCname|Specifies whether CNAME can be used as an endpoint. By default, CNAME can be used as an endpoint.|ClientConfiguration.setSupportCname|
|SLDEnabled|Specifies whether access from second-level domains is enabled. By default, access from second-level domains is disabled.|ClientConfiguration.setSLDEnabled|
|Protocol|The protocol used to connect to OSS. Default value: HTTP. Valid values: HTTP and HTTPS.|ClientConfiguration.setProtocol|
|UserAgent|Specifies the user agent \(the User-Agent field in the HTTP header\). Default value: `aliyun-sdk-java`.|ClientConfiguration.setUserAgent|
|ProxyHost|The IP address to access the proxy host.|ClientConfiguration.setProxyHost|
|ProxyPort|The port for the proxy server.|ClientConfiguration.setProxyPort|
|ProxyUsername|The username verified by the proxy server.|ClientConfiguration.setProxyUsername|
|ProxyPassword|The password verified by the proxy server.|ClientConfiguration.setProxyPassword|
|RedirectEnable|Specifies whether HTTP redirection is enabled. **Note:** You can configure whether HTTP redirection is enabled for OSS SDK for Java V3.10.1 or later. By default, HTTP redirection is enabled for OSS SDK for Java V3.10.1 or later.

|ClientConfiguration.setRedirectEnable|
|VerifySSLEnable|Specifies whether SSL-based authentication is enabled. **Note:** You can configure whether SSL-based authentication or later is enabled for OSS SDK for Java V3.10.1. By default, SSL-based authentication is enabled for OSS SDK for Java V3.10.1 or later.

|ClientConfiguration.setVerifySSLEnable|

The following code provides an example on how to configure parameters for an OSSClient instance with ClientConfiguration:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you log on with the AccessKey pair of an Alibaba Cloud account because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Create a ClientConfiguration instance. ClientConfiguration is a configuration class of OSSClient. You can use ClientConfiguration to configure parameters such as host proxies, connection timeouts, and the maximum number of connections.
ClientBuilderConfiguration conf = new ClientBuilderConfiguration();

// Configure the maximum number of HTTP connections allowed by an OSSClient instance. Default value: 1024.
conf.setMaxConnections(200);
// Configure the timeout period for data transmission at the socket layer. Unit: milliseconds. Default value: 50000.
conf.setSocketTimeout(10000);
// Configure the timeout period for establishing a connection. Unit: milliseconds. Default value: 50000.
conf.setConnectionTimeout(10000);
// Configure the timeout period for obtaining a connection from the connection pool. Unit: milliseconds. By default, this parameter is not configured.
conf.setConnectionRequestTimeout(1000);
// Configure the timeout period for idle connections. The connection is closed when the timeout period expires. Unit: milliseconds. Default value: 60000.
conf.setIdleConnectionTime(10000);
// Configure the maximum number of allowed retry attempts when a request error occurs. Default value: 3.
conf.setMaxErrorRetry(5);
// Configure whether CNAME can be used as an endpoint. By default, CNAME can be used as an endpoint.
conf.setSupportCname(true);
// Configure whether access from second-level domains is enabled. By default, access from second-level domains is disabled.
conf.setSLDEnabled(true);
// Configure the protocol used to connect to OSS. Valid values: HTTP and HTTPS. Default value: HTTP.
conf.setProtocol(Protocol.HTTP);
// Configure the user agent in the HTTP header. Default value: aliyun-sdk-java.
conf.setUserAgent("aliyun-sdk-java");
// Configure the port for the proxy host.
conf.setProxyHost("<yourProxyHost>");
// Configure the username verified by the proxy host.
conf.setProxyUsername("<yourProxyUserName>");
// Configure the password verified by the proxy host.
conf.setProxyPassword("<yourProxyPassword>");
// Configure whether HTTP redirection is enabled. By default, HTTP redirection is enabled.
conf.setRedirectEnable(true);
// Configure whether SSL-based authentication is enabled. By default, SSL-based authentication is enabled.
conf.setVerifySSLEnable(true);

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret, conf);

// Shut down the OSSClient instance.
ossClient.shutdown();
            
```

