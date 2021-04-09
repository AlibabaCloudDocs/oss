# Simple upload

To implement simple upload, you can call PutObject to upload a single object. Simple upload includes streaming upload and object upload. Streaming upload uses InputStream as the data source of an object uploaded to OSS. Object upload uses a local file as the data source of an object uploaded to OSS. This topic describes how to use streaming upload and object upload to upload an object.

**Note:** For the complete code used to perform simple upload, visit [GitHub](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/SimpleGetObjectSample.java).

## Streaming upload

Streaming upload allows you to upload a data stream to an OSS object.

-   Upload a string

    The following code provides an example on how to upload a string to the exampleobject.txt object in the examplebucket bucket:

    ```
    // Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com.
    String endpoint = "yourEndpoint";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "yourAccessKeyId";
    String accessKeySecret = "yourAccessKeySecret";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Specify a string.
    String content = "Hello OSS";
    
    // Create a PutObjectRequest object.
    // Specify the name of the bucket and the full path of the object. The full path of the object cannot contain bucket names.
    PutObjectRequest putObjectRequest = new PutObjectRequest("examplebucket", "exampleobject.txt", new ByteArrayInputStream(content.getBytes()));
    
    // Optional. Specify the storage class and access control list (ACL) of the object.
    // ObjectMetadata metadata = new ObjectMetadata();
    // metadata.setHeader(OSSHeaders.OSS_STORAGE_CLASS, StorageClass.Standard.toString());
    // metadata.setObjectAcl(CannedAccessControlList.Private);
    // putObjectRequest.setMetadata(metadata);
    
    // Upload the string.
    ossClient.putObject(putObjectRequest);
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();                   
    ```

-   Upload a byte array

    The following code provides an example on how to upload a byte array to the exampleobject.txt object in the examplebucket bucket:

    ```
    // Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com.
    String endpoint = "yourEndpoint";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to https://ram.console.aliyun.com.
    String accessKeyId = "yourAccessKeyId";
    String accessKeySecret = "yourAccessKeySecret";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId,accessKeySecret);
    
    // Specify a byte array.
    byte[] content = "Hello OSS".getBytes();
    // Specify the name of the bucket and the full path of the object. The full path of the object cannot contain bucket names.
    ossClient.putObject("examplebucket", "exampleobject.txt", new ByteArrayInputStream(content));
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
    ```

-   Upload a network stream

    The following code provides an example on how to upload a network stream to the exampleobject.txt object in the examplebucket bucket:

    ```
    // Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com.
    String endpoint = "yourEndpoint";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "yourAccessKeyId";
    String accessKeySecret = "yourAccessKeySecret";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Specify the address of the network stream.
    InputStream inputStream = new URL("https://www.aliyun.com/").openStream();
    // Specify the name of the bucket and the full path of the object. The full path of the object cannot contain bucket names.
    ossClient.putObject("examplebucket", "exampleobject.txt", inputStream);
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
    ```

-   Upload a file stream

    The following code provides an example on how to upload a file stream to the exampleobject.txt object in the examplebucket bucket:

    ```
    // Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com.
    String endpoint = "yourEndpoint";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "yourAccessKeyId";
    String accessKeySecret = "yourAccessKeySecret";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Specify the full path of the local file to upload. If the path of the local file is not specified, the file stream is uploaded from the path of the project to which the sample program belongs.
    InputStream inputStream = new FileInputStream("D:\\localpath\\examplefile.txt");
    // Specify the name of the bucket and the full path of the object. The full path of the object cannot contain bucket names.
    ossClient.putObject("examplebucket", "exampleobject.txt", inputStream);
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
    ```


## Upload an object

Object upload allows you to upload a local file to an OSS object.

The following code provides an example on how to upload a local file named examplefile.txt to the exampleobject.txt object in the examplebucket bucket:

```
// Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com.
String endpoint = "yourEndpoint";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "yourAccessKeyId";
String accessKeySecret = "yourAccessKeySecret";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Create a PutObjectRequest object.
// Specify the name of the bucket, the full path of the object, and the full path of the local file. The full path of the object cannot contain bucket names.
// If the path of the local file is not specified, the file is uploaded from the path of the project to which the sample program belongs.
PutObjectRequest putObjectRequest = new PutObjectRequest("examplebucket", "exampleobject.txt", new File("D:\\localpath\\examplefile.txt"));

// Optional. Specify the storage class and ACL of the object.
// ObjectMetadata metadata = new ObjectMetadata();
// metadata.setHeader(OSSHeaders.OSS_STORAGE_CLASS, StorageClass.Standard.toString());
// metadata.setObjectAcl(CannedAccessControlList.Private);
// putObjectRequest.setMetadata(metadata);

// Upload the local file.
ossClient.putObject(putObjectRequest);

// Shut down the OSSClient instance.
ossClient.shutdown();            
```

