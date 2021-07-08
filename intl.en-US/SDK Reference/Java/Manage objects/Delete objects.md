# Delete objects

You can delete a single object, multiple objects, objects whose names contain a specified prefix, or a directory and all objects stored in this directory.

**Warning:** Deleted objects cannot be recovered. Exercise caution when you delete objects.

## Delete a single object

The following code provides an example on how to delete an object named exampleobject.txt from a bucket named examplebucket:

```
// Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if the bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
String endpoint = "yourEndpoint";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to access Object Storage Service (OSS) because the account has permissions on all API operations. We recommend that you use a Resource Access Management (RAM) user to call API operations or perform routine O&M. To create a RAM user, log on to the RAM console. 
String accessKeyId = "yourAccessKeyId";
String accessKeySecret = "yourAccessKeySecret";
// Specify the name of the bucket. 
String bucketName = "examplebucket";
// Specify the full path of the object. The full path of the object cannot contain bucket names. 
String objectName = "exampleobject.txt";

// Create an OSSClient instance. 
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Delete the object or the directory. To delete a directory, make sure that the directory contains no objects. 
ossClient.deleteObject(bucketName, objectName);

// Shut down the OSSClient instance. 
ossClient.shutdown();
            
```

## Delete multiple objects

You can delete up to 1,000 objects each time. You can delete multiple objects, objects whose names contain a specified prefix, or a directory and all objects stored in this directory.

-   Parameters

    The following table describes the request parameters used to delete multiple objects.

    |Parameter|Description|Method|
    |:--------|:----------|:-----|
    |Keys|Specifies the objects that you want to delete.|setKeys\(List<String\>\)|
    |quiet|Specifies whether the mode in which the result can be returned is quiet. You can select one of the following modes:     -   Verbose: If quiet is not specified or is set to false, a list of all deleted objects is returned. This is the default return mode.
    -   Quiet: If quiet is set to true, only a list of objects that fail to be deleted is returned.
|setQuiet\(boolean\)|
    |encodingType|Specifies the method for encoding the object names in the result. Only URL encoding is supported.|setEncodingType\(String\)|

    The following table describes the response parameters.

    |Parameter|Description|Method|
    |:--------|:----------|:-----|
    |deletedObjects|Indicates the result of the operation. The list of deleted objects is returned when the Verbose mode is used. The list of objects that fail to be deleted is returned when the Quiet mode is used.|List<String\> getDeletedObjects\(\)|
    |encodingType|Indicates the method for encoding the objects names listed in deletedObjects. When the parameter is empty, the object names are not encoded.|getEncodingType\(\)|

-   Examples
    -   Delete multiple objects

        The following code provides an example on how to delete multiple objects from a bucket named examplebucket:

        ```
        // Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if the bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
        String endpoint = "yourEndpoint";
        // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to access OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine O&M. To create a RAM user, log on to the RAM console. 
        String accessKeyId = "yourAccessKeyId";
        String accessKeySecret = "yourAccessKeySecret";
        // Specify the name of the bucket. 
        String bucketName = "examplebucket";
        
        // Create an OSSClient instance. 
        OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
        
        // Delete the objects. 
        // Specify the full paths of the multiple objects that you want to delete. The full paths of the objects cannot contain bucket names. 
        List<String> keys = new ArrayList<String>();
        keys.add("exampleobjecta.txt");
        keys.add("testfolder/sampleobject.txt");
        keys.add("exampleobjectb.txt");
        
        DeleteObjectsResult deleteObjectsResult = ossClient.deleteObjects(new DeleteObjectsRequest(bucketName).withKeys(keys));
        List<String> deletedObjects = deleteObjectsResult.getDeletedObjects();
        
        // Shut down the OSSClient instance. 
        ossClient.shutdown();           
        ```

    -   Delete objects whose names contain a specified prefix

        The following code provides an example on how to delete objects whose names contain the "file" prefix from a bucket named examplebucket:

        ```
        // Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if the bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
        String endpoint = "yourEndpoint";
        // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to access OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine O&M. To create a RAM user, log on to the RAM console. 
        String accessKeyId = "yourAccessKeyId";
        String accessKeySecret = "yourAccessKeySecret";
        // Specify the name of the bucket. 
        String bucketName = "examplebucket";
        
        // Specify the prefix. 
        final String prefix = "file";
        
        // Create an OSSClient instance. 
        OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
        
        // List all objects whose names contain the specified prefix and delete the objects. 
        String nextMarker = null;
        ObjectListing objectListing = null;
        do {
            ListObjectsRequest listObjectsRequest = new ListObjectsRequest(bucketName)
                    .withPrefix(prefix)
                    .withMarker(nextMarker);
        
            objectListing = ossClient.listObjects(listObjectsRequest);
            if (objectListing.getObjectSummaries().size() > 0) {
                List<String> keys = new ArrayList<String>();
                for (OSSObjectSummary s : objectListing.getObjectSummaries()) {
                    System.out.println("key name: " + s.getKey());
                    keys.add(s.getKey());
                }
                DeleteObjectsRequest deleteObjectsRequest = new DeleteObjectsRequest(bucketName).withKeys(keys);
                ossClient.deleteObjects(deleteObjectsRequest);
            }
        
            nextMarker = objectListing.getNextMarker();
        } while (objectListing.isTruncated());
        
        // Shut down the OSSClient instance. 
        ossClient.shutdown();
        ```

    -   Delete a specified directory and all objects stored in the directory.

        The following code provides an example on how to delete a directory named testdir and all objects stored in the directory from a bucket named examplebucket:

        ```
        // Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if the bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
        String endpoint = "yourEndpoint";
        // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to access OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine O&M. To create a RAM user, log on to the RAM console. 
        String accessKeyId = "yourAccessKeyId";
        String accessKeySecret = "yourAccessKeySecret";
        // Specify the name of the bucket. 
        String bucketName = "examplebucket";
        
        
        // Specify the full path of the directory. The full path of the directory cannot contain bucket names. For example, the full path of the directory named testdir is testdir/. 
        final String prefix = "testdir/";
        
        // Create an OSSClient instance. 
        OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
        
        // Delete the directory and all objects stored in the directory. 
        String nextMarker = null;
        ObjectListing objectListing = null;
        do {
            ListObjectsRequest listObjectsRequest = new ListObjectsRequest(bucketName)
                    .withPrefix(prefix)
                    .withMarker(nextMarker);
        
            objectListing = ossClient.listObjects(listObjectsRequest);
            if (objectListing.getObjectSummaries().size() > 0) {
                List<String> keys = new ArrayList<String>();
                for (OSSObjectSummary s : objectListing.getObjectSummaries()) {
                    System.out.println("key name: " + s.getKey());
                    keys.add(s.getKey());
                }
                DeleteObjectsRequest deleteObjectsRequest = new DeleteObjectsRequest(bucketName).withKeys(keys);
                ossClient.deleteObjects(deleteObjectsRequest);
            }
        
            nextMarker = objectListing.getNextMarker();
        } while (objectListing.isTruncated());
        
        // Shut down the OSSClient instance. 
        ossClient.shutdown();
        ```


For the complete code that is used to delete multiple objects, visit [GitHub](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/DeleteObjectsSample.java).

