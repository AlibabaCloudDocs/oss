# Streaming download

If you want to download a large object or it takes a long time to download an object at a time, you can use streaming download to download the object incrementally until the entire object is downloaded.

If you do not close the ossObject object after it is used, connection leaks may occur. As a result, no connection is available and an exception occurs. The following code provides an example on how to close an ossObject object:

```
OSSObject ossObject = ossClient.getObject(bucketName, objectName);
ossObject.close();
        
```

The following code provides an example on how to use streaming download to download an object named exampleobject.txt from a bucket named examplebucket:

```
// Set yourEndpoint to the endpoint of the region where the bucket is located. For example, if your bucket is in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com.
String endpoint = "yourEndpoint";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "yourAccessKeyId";
String accessKeySecret = "yourAccessKeySecret";
// Specify the bucket name.
String bucketName = "examplebucket";
// Specify the complete path of the object, which does not include the bucket name.
String objectName = "exampleobject.txt";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// ossObject includes the bucket name, key (object name), object meta (meta information), and an InputStream.
OSSObject ossObject = ossClient.getObject(bucketName, objectName);

// Read the object content.
System.out.println("Object content:");
BufferedReader reader = new BufferedReader(new InputStreamReader(ossObject.getObjectContent()));
while (true) {
    String line = reader.readLine();
    if (line == null) break;

    System.out.println("\n" + line);
}
// You must close the stream obtained by using the ossClient.getObject API operation after the object is read. Otherwise, connection leaks may occur. Consequently, no connections are available and an exception occurs.
reader.close();

// Shut down the OSSClient instance.
ossClient.shutdown();
        
```

For the complete code of streaming download, visit [GitHub](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/SimpleGetObjectSample.java).

