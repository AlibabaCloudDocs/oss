# Quick Start

This topic describes how to use OSS SDK for Java to perform routine operations, such as creating buckets, uploading objects, and downloading objects.

## Sample projects

OSS SDK for Java provides sample projects for Maven and Ant. You can compile and run the sample projects on local devices or develop your own applications based on the sample projects. For more information about how to compile and run a project, see README.md in the project directory.

-   Sample project for Maven: [aliyun-oss-java-sdk-demo-mvn.zip](https://gosspublic.alicdn.com/sdks/java/aliyun-oss-java-sdk-demo-mvn-3.10.2.zip)
-   Sample project for Ant: [aliyun-oss-java-sdk-demo-ant.zip](https://gosspublic.alicdn.com/sdks/java/aliyun-oss-java-sdk-demo-ant-3.10.2.zip)

## Create buckets

A bucket is a global namespace in OSS. A bucket is a container for objects stored in OSS. The following code provides an example on how to create a bucket:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create your RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Create a bucket.
ossClient.createBucket(bucketName);

// Shut down the OSSClient instance.
ossClient.shutdown();            
```

For more information about bucket naming conventions, see the "Naming conventions" section in [Terms](/intl.en-US/Developer Guide/Terms.md). For more information about how to create a bucket, see [Create buckets](/intl.en-US/Console User Guide/Manage buckets/Create buckets.md).

For more information about endpoints, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).

## Upload objects

The following code provides an example on how to upload an object to OSS:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create your RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
// <yourObjectName> indicates the complete path of the object you want to upload to OSS, and must include the file extension of the object. Example: abc/efg/123.jpg.
String objectName = "<yourObjectName>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Upload the object to a specified bucket and save it with the specified object name.
String content = "Hello OSS";
ossClient.putObject(bucketName, objectName, new ByteArrayInputStream(content.getBytes()));

// Shut down the OSSClient instance.
ossClient.shutdown();            
```

For more information about how to upload objects, see [Upload objects](/intl.en-US/SDK Reference/Java/Upload objects/Overview.md).

## Download objects

The following code provides an example on how to download an object:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create your RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
// <yourObjectName> indicates the complete path of the object you want to download from OSS. The path must include the file extension of the object. For example, you can set <yourObjectName> to abc/efg/123.jpg.
String objectName = "<yourObjectName>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Use the ossClient.getObject method to obtain an OSSObject instance. The OSSObject instance contains the content and metadata of the object.
OSSObject ossObject = ossClient.getObject(bucketName, objectName);
// Use the ossObject.getObjectContent method to obtain an input stream of the object. Read the input stream to obtain the content of the object.
InputStream content = ossObject.getObjectContent();
if (content ! = null) {
    BufferedReader reader = new BufferedReader(new InputStreamReader(content));
    while (true) {
        String line = reader.readLine();
        if (line == null) break;
        System.out.println("\n" + line);
    }
    // You must close the connection after each use to prevent connection leaks. Otherwise, requests may be unable to establish connections and the program cannot run properly.
    content.close();
}

// Shut down the OSSClient.
ossClient.shutdown();            
```

For more information about how to download objects, see [Download objects](/intl.en-US/SDK Reference/Java/Download objects/Overview.md).

## List objects

The following code provides an example on how to list objects in a specified bucket. By default, up to 100 objects are listed.

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create your RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";

// Create an OssClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Use the ossClient.listObjects method to obtain an ObjectListing instance. The ObjectListing instance contains the response to the ListObject request.
ObjectListing objectListing = ossClient.listObjects(bucketName);
// Use the objectListing.getObjectSummaries method to obtain the descriptions of all objects.
for (OSSObjectSummary objectSummary : objectListing.getObjectSummaries()) {
    System.out.println(" - " + objectSummary.getKey() + "  " +
            "(size = " + objectSummary.getSize() + ")");
}

// Shut down the OSSClient instance.
ossClient.shutdown();            
```

For more information about how to list objects, see [List objects](/intl.en-US/SDK Reference/Java/Manage objects/List objects.md).

## Delete objects

For the complete code used to delete objects, visit [GitHub](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/GetStartedSample.java).

The following code provides an example on how to delete an object:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create your RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
// <yourObjectName> indicates the complete path of the object you want to delete, and must include the file extension of the object. Example: abc/efg/123.jpg.
String objectName = "<yourObjectName>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Delete an object.
ossClient.deleteObject(bucketName, objectName);

// Shut down the OSSClient instance.
ossClient.shutdown();            
```

For more information about how to delete objects, see [Delete objects](/intl.en-US/SDK Reference/Java/Manage objects/Delete objects.md).

