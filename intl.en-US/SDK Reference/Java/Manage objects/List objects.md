# List objects

This topic describes how to list all objects in a bucket, a specified number of objects in a bucket, and objects whose names contain a specified prefix in a bucket.

## Background information

You can call the GetBucket \(ListObjects\) or GetBucketV2 \(ListObjectsV2\) operation to list up to 1,000 objects in a bucket. You can configure the parameters to use different methods to list objects. For example, you can configure the parameters to list objects from a specified start position, list objects and subfolders in specified folders, and list objects by page. The GetBucket \(ListObjects\) and GetBucketV2 \(ListObjectsV2\) operations are different in the following aspects:

-   By default, when you use the GetBucket \(ListObjects\) operation to list objects, the bucket owner information is included in the response.
-   When you use the GetBucketV2 \(ListObjectsV2\) operation to list objects, you can set the fetch\_owner parameter to specify whether to include the information about the bucket owner in the response.

    **Note:** To list objects in a versioned bucket, we recommend that you use the GetBucketV2 \(ListObjectsV2\) operation.


The following tables describe the parameters that you must configure when you use the GetBucket \(ListObjects\) or GetBucketV2 \(ListObjectsV2\) operation to list objects.

-   Use GetBucket \(ListObjects\) to list objects

    When you call the GetBucket \(ListObjects\) operation to list objects, you can specify the parameters in the following three formats:

    -   ObjectListing listObjects\(String bucketName\): lists all objects in a bucket. By default, up to 100 objects can be listed by a single request.
    -   ObjectListing listObjects\(String bucketName, String prefix\): lists objects whose names contain a specified prefix in a bucket. By default, up to 100 objects can be listed by a single request.
    -   ObjectListing listObjects\(ListObjectsRequest listObjectsRequest\): provides multiple filtering features to perform flexible queries.
    The following table describes the parameters you can configure when you use GetBucket \(ListObjects\) to list objects.

    |Parameter|Description|Method|
    |:--------|:----------|:-----|
    |objectSummaries|The object metadata you want to return.|List<OSSObjectSummary\> getObjectSummaries\(\)|
    |prefix|The prefix that the names of returned objects must contain.|String getPrefix\(\)|
    |delimiter|The character used to group objects by name.|String getDelimiter\(\)|
    |marker|The position from which the list operation starts. Objects are returned in lexicographic order starting from this position.|String getMarker\(\)|
    |maxKeys|The maximum number of objects to list each time.|int getMaxKeys\(\)|
    |nextMarker|The position from which the next list operation starts.|String getNextMarker\(\)|
    |isTruncated|Indicates whether the listed objects are truncated.     -   If all objects are listed, the returned value of this parameter is false.
    -   If only part of the objects are listed, the returned value of this parameter is true.
|boolean isTruncated\(\)|
    |commonPrefixes|A set of substrings of object names. Objects whose names end with the delimiter and contain the same prefix are grouped as a single result element in commonPrefixes.|List<String\> getCommonPrefixes\(\)|
    |encodingType|The encoding type of the object names in the response.|String getEncodingType\(\)|

-   Use GetBucketV2 \(ListObjectsV2\) to list objects

    When you call the GetBucketV2 \(ListObjectsV2\) operation to list objects, you can specify the parameters in the following three formats:

    -   ListObjectsV2Result listObjectsV2\(String bucketName\): lists all objects in the bucket. By default, up to 100 objects can be listed by a single request.
    -   ListObjectsV2Result listObjectsV2\(String bucketName, String prefix\): lists objects whose names contain a specified prefix in a bucket. By default, up to 100 objects can be listed by a single request.
    -   ListObjectsV2Result listObjectsV2\(ListObjectsRequest listObjectsRequest\): provides multiple filtering features to perform flexible queries.
    The following table describes the parameters you can configure when you use GetBucketV2 \(ListObjectsV2\) to list objects.

    |Parameter|Description|Method|
    |:--------|:----------|:-----|
    |objectSummaries|The object metadata that you want to return.|List<OSSObjectSummary\> getObjectSummaries\(\)|
    |prefix|The prefix that the names of returned objects must contain.|String getPrefix\(\)|
    |delimiter|The character used to group objects by name.|String getDelimiter\(\)|
    |startAfter|The position from which the list operation starts. Objects are returned in lexicographic order starting from this position.|String getStartAfter\(\)|
    |maxKeys|The maximum number of objects to list each time.|int getMaxKeys\(\)|
    |continuationToken|The continuationToken used in the current list operation.|String getContinuationToken\(\)|
    |nextContinuationToken|The continuationToken used in the next list operation.|String getNextContinuationToken\(\)|
    |isTruncated|Indicates whether the listed objects are truncated.     -   If all objects are listed, the returned value of this parameter is false.
    -   If only part of the objects are listed, the returned value of this parameter is true.
|boolean isTruncated\(\)|
    |commonPrefixes|A set of substrings of object names. Objects whose names end with the delimiter and contain the same prefix are grouped as a single result element in commonPrefixes.|List<String\> getCommonPrefixes\(\)|
    |encodingType|The encoding type of the object names in the response.|String getEncodingType\(\)|
    |fetchOwner|Specifies whether to include the owner information in the response.     -   true: The response includes the owner information.
    -   false: The response does not include the owner information.
|String getFetchOwner\(\)|


## Simple list

You can use the GetBucket \(ListObjects\) or GetBucketV2 \(ListObjectsV2\) operation to list objects in a bucket.

-   Use GetBucket \(ListObjects\) to list objects

    The following code provides an example on how to use GetBucket \(ListObjects\) to list objects in a specified bucket. By default, 100 objects are listed by a request.

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    String KeyPrefix = "<yourKeyPrefix>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // List objects in the bucket. If you do not set KeyPrefix, all objects in the bucket are listed. If you set KeyPrefix, objects whose names contain the specified prefix in the bucket are listed.
    ObjectListing objectListing = ossClient.listObjects(bucketName, KeyPrefix);
    List<OSSObjectSummary> sums = objectListing.getObjectSummaries();
    for (OSSObjectSummary s : sums) {
        System.out.println("\t"   s.getKey());
    }
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
                        
    ```

-   Use GetBucketV2 \(ListObjectsV2\) to list objects

    The following code provides an example on how to use GetBucketV2 \(ListObjectsV2\) to list objects in a specified bucket. By default, 100 objects are listed by a request.

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // List objects in the bucket. If you do not set KeyPrefix, all objects in the bucket are listed. If you set KeyPrefix, objects whose names contain the specified prefix in the bucket are listed.
    ListObjectsV2Result result = ossClient.listObjectsV2(bucketName);
    List<OSSObjectSummary> ossObjectSummaries = result.getObjectSummaries();
    
    for (OSSObjectSummary obj : ossObjectSummaries) {
        System.out.println("\t"   s.getKey());
    }
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
    ```


## List objects by using ListObjectsRequest

You can configure parameters of ListObjectsRequest to perform flexible queries. The following table describes the parameters you can configure for ListObjectsRequest.

|Parameter|Description|Method|
|:--------|:----------|:-----|
|prefix|The prefix that the names of returned objects must contain.|setPrefix\(String prefix\)|
|delimiter|The character used to group objects by name. Objects whose names contain the same string ranged from the prefix to the next occurrence of the delimiter are grouped as a single result element in commonPrefixes.|setDelimiter\(String delimiter\)|
|marker|The position from which the list operation starts. All objects are returned in lexicographic order starting from this position.|setMarker\(String marker\)|
|maxKeys|The maximum number of objects that can be listed at a time. The default value is 100. The maximum value is 1000.|setMaxKeys\(Integer maxKeys\)|
|encodingType|The encoding type of the object names in the response. Only URL encoding is supported.|setEncodingType\(String encodingType\)|

-   List a specified number of objects

    The following code provides an example on how to list a specified number of objects:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Specifies the maximum number of objects that can be listed each time.
    final int maxKeys = 200;
    // List objects in the bucket.
    ObjectListing objectListing = ossClient.listObjects(new ListObjectsRequest(bucketName).withMaxKeys(maxKeys));
    List<OSSObjectSummary> sums = objectListing.getObjectSummaries();
    for (OSSObjectSummary s : sums) {
        System.out.println("\t"   s.getKey());
    }
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
                        
    ```

-   List objects whose names contain a specified prefix

    The following code provides an example on how to list objects whose names contain a specified prefix:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Specify the prefix.
    final String keyPrefix = "<yourkeyPrefix>";
    
    // List the objects whose names contain the specified prefix. By default, up to 100 objects are listed.
    ObjectListing objectListing = ossClient.listObjects(new ListObjectsRequest(bucketName).withPrefix(keyPrefix));
    List<OSSObjectSummary> sums = objectListing.getObjectSummaries();
    for (OSSObjectSummary s : sums) {
        System.out.println("\t"   s.getKey());
    }
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
                        
    ```

-   List objects from the object specified by marker

    You can configure the marker parameter to specify the name of the object from which the list operation starts. The following code provides an example on how to list objects from the object specified by marker:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    final String marker = "<yourMarker>";
    
    // List objects from the object specified by marker. By default, up to 100 objects are listed.
    ObjectListing objectListing = ossClient.listObjects(new ListObjectsRequest(bucketName).withMarker(marker));
    List<OSSObjectSummary> sums = objectListing.getObjectSummaries();
    for (OSSObjectSummary s : sums) {
        System.out.println("\t"   s.getKey());
    }
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
                        
    ```

-   List all objects by page

    The following code provides an example on how to list all objects in a specified bucket by page. You can configure the maxKeys parameter to specify the maximum number of objects that can be listed on each page.

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    final int maxKeys = 200;
    String nextMarker = null;
    ObjectListing objectListing;
    
    do {
        objectListing = ossClient.listObjects(new ListObjectsRequest(bucketName).withMarker(nextMarker).withMaxKeys(maxKeys));
    
        List<OSSObjectSummary> sums = objectListing.getObjectSummaries();
        for (OSSObjectSummary s : sums) {
            System.out.println("\t"   s.getKey());
        }
    
        nextMarker = objectListing.getNextMarker();
    
    } while (objectListing.isTruncated());
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
                        
    ```

-   List objects whose names contain a specified prefix by page

    The following code provides an example on how to list objects whose names contain a specified prefix by page:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Specify that up to 200 objects can be listed on each page.
    final int maxKeys = 200;
    final String keyPrefix = "<yourkeyPrefix>";
    String nextMarker = "<yourNextMarker>";
    ObjectListing objectListing;
    
    do {
        objectListing = ossClient.listObjects(new ListObjectsRequest(bucketName).
                withPrefix(keyPrefix).withMarker(nextMarker).withMaxKeys(maxKeys));
    
        List<OSSObjectSummary> sums = objectListing.getObjectSummaries();
        for (OSSObjectSummary s : sums) {
            System.out.println("\t"   s.getKey());
        }
    
        nextMarker = objectListing.getNextMarker();
    
    } while (objectListing.isTruncated());
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
                        
    ```

-   List objects and specify the encoding type of the object names

    If the name of an object contains any of the following special characters, you must encode the object name before you transmit the object. Only URL encoding is supported in OSS.

    -   Single quotation marks \('\)
    -   Double quotations marks \("\)
    -   Ampersands \(&\)
    -   Angle brackets \(<\>\)
    -   Pause marker \(、\)
    -   Chinese characters
    The following code provides an example on how to list objects and specify the encoding type of the object names in the response:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    final int maxKeys = 200;
    final String keyPrefix = "<yourkeyPrefix>";
    String nextMarker = "<yourNextMarker>";
    ObjectListing objectListing;
    
    do {
        ListObjectsRequest listObjectsRequest = new ListObjectsRequest(bucketName);
        listObjectsRequest.setPrefix(keyPrefix);
        listObjectsRequest.setMaxKeys(maxKeys);
        listObjectsRequest.setMarker(nextMarker);
    
        // Specify the encoding type of the object names in the response.
        listObjectsRequest.setEncodingType("url");
    
        objectListing = ossClient.listObjects(listObjectsRequest);
    
        // Decode the value of key in the response.
        for (OSSObjectSummary objectSummary: objectListing.getObjectSummaries()) {
            System.out.println("Key:"   URLDecoder.decode(objectSummary.getKey(), "UTF-8"));
        }
    
        // Decode the value of commonPrefixes in the response.
        for (String commonPrefixes: objectListing.getCommonPrefixes()) {
            System.out.println("CommonPrefixes:"   URLDecoder.decode(commonPrefixes, "UTF-8"));
        }
    
        // Decode the value of nextMarker in the response.
        if (objectListing.getNextMarker() ! = null) {
            nextMarker = URLDecoder.decode(objectListing.getNextMarker(), "UTF-8");
        }
    } while (objectListing.isTruncated());
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
                        
    ```


## Use ListObjectsV2Request to list objects

You can configure the parameters of ListObjectsV2Request to flexibly list objects. For example, you can list a specified number of objects, list objects whose names contain a specified prefix, and list all objects in a bucket by page.

The following table describes the parameters you can configure for ListObjectsV2Request.

|Parameter|Description|Method|
|:--------|:----------|:-----|
|prefix|The prefix that the names of returned objects must contain.|setPrefix\(String prefix\)|
|delimiter|The character used to group objects by name. Objects whose names contain the same string ranged from the prefix to the next occurrence of the delimiter are grouped as a single result element in commonPrefixes.|setDelimiter\(String delimiter\)|
|maxKeys|The maximum number of objects that can be listed at a time. The default value is 100. The maximum value is 1000.|setMaxKeys\(Integer maxKeys\)|
|startAfter|The position from which the list operation starts. All objects are returned in lexicographic order starting from this position.|setStartAfter\(String startAfter\)|
|continuationToken|The continuationToken used in the current list operation.|setContinuationToken\(String continuationToken\)|
|encodingType|The encoding type of the object names in the response. Only URL encoding is supported.|setEncodingType\(String encodingType\)|
|fetchOwner|Specifies whether to include the owner information in the response.|setFetchOwner\(boolean fetchOwner \)|

-   List a specified number of objects

    The following code provides an example on how to list a specified number of objects:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Specifies the maximum number of objects that can be listed each time.
    final int maxKeys = 200
    
    // List the objects. By default, 100 objects are returned by one request. In this example, up to 200 objects can be returned each time.
    ListObjectsV2Request listObjectsV2Request = new ListObjectsV2Request(bucketName);
    listObjectsV2Request.setMaxKeys(maxKeys);
    ListObjectsV2Result result = ossClient.listObjectsV2(listObjectsV2Request);
    List<OSSObjectSummary> ossObjectSummaries = result.getObjectSummaries();
    
    for (OSSObjectSummary obj : ossObjectSummaries) {
        System.out.println("\t"   s.getKey());
    }
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
    ```

-   List objects whose names contain a specified prefix

    The following code provides an example on how to list objects whose names contain a specified prefix:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Specifies the maximum number of objects that can be listed each time.
    final int maxKeys = 200
    String prefix = "<yourKeyPrefix>";
    
    // List the objects whose names contain the specified prefix.
    ListObjectsV2Request listObjectsV2Request = new ListObjectsV2Request(bucketName);
    listObjectsV2Request.setPrefix(prefix);
    ListObjectsV2Result result = ossClient.listObjectsV2(listObjectsV2Request);
    List<OSSObjectSummary> ossObjectSummaries = result.getObjectSummaries();
    
    for (OSSObjectSummary obj : ossObjectSummaries) {
        System.out.println("\t"   s.getKey());
    }
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
    ```

-   List objects from the object specified by startAfter

    The following code provides an example on how to list objects from the object specified by startAfter. All objects are returned in lexicographic order starting from the specified object.

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // List objects from the object specified by startAfter. The value of startAfter can be the name of an existing object or other strings.
    // In this example, all objects are returned in lexicographic order starting from the object named test-obj. The object named test-obj is not listed even if it exists.
    ListObjectsV2Request listObjectsV2Request = new ListObjectsV2Request(bucketName);
    listObjectsV2Request.setStartAfter("test-obj");
    ListObjectsV2Result result = ossClient.listObjectsV2(listObjectsV2Request);
    List<OSSObjectSummary> ossObjectSummaries = result.getObjectSummaries();
    
    for (OSSObjectSummary obj : ossObjectSummaries) {
        System.out.println("\t"   s.getKey());
    }
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
    ```

-   List objects and their owner information

    The following code provides an example on how to list objects and their owner information:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // By default, the owner information of the objects is not listed. To include the object owner information in the returned results, set the fetchOwner parameter to true.
    ListObjectsV2Request listObjectsV2Request = new ListObjectsV2Request(bucketName);
    listObjectsV2Request.setFetchOwner(true)
    ListObjectsV2Result result = ossClient.listObjectsV2(listObjectsV2Request);
    List<OSSObjectSummary> ossObjectSummaries = result.getObjectSummaries();
    
    for (OSSObjectSummary obj : ossObjectSummaries) {
        System.out.println("\t"   s.getKey());
        if (obj.getOwner() ! = null) {
            System.out.println("owner id:"   obj.getOwner().getId());
            System.out.println("owner id:"   obj.getOwner().getDisplayName());
        }
    }
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
    ```

-   List all objects by page

    The following code provides an example on how to list all objects in a specified bucket by page. You can configure the maxKeys parameter to specify the maximum number of objects that can be listed on each page.

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Specifies the maximum number of objects that can be listed each time.
    final int maxKeys = 200
    String nextContinuationToken = null;
    ListObjectsV2Result result = null;
    
    // List objects in the bucket by page by using the nextContinuationToken parameter included in the results of the previous list operation.
    do {
        ListObjectsV2Request listObjectsV2Request = new ListObjectsV2Request(bucketName).withMaxKeys(maxKeys);
        listObjectsV2Request.setContinuationToken(nextContinuationtoken);
        result = ossClient.listObjectsV2(listObjectsV2Request);
    
        List<OSSObjectSummary> sums = result.getObjectSummaries();
        for (OSSObjectSummary s : sums) {
            System.out.println("\t"   s.getKey());
        }
    
        nextContinuationToken = result.getNextContinuationToken();
    
    } while (result1.isTruncated());
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
    ```

-   List objects whose names contain a specified prefix by page

    The following code provides an example on how to list objects whose names contain a specified prefix by page:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    String KeyPrefix = "<yourKeyPrefix>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Specifies the maximum number of objects that can be listed each time.
    final int maxKeys = 200
    String nextContinuationToken = null;
    ListObjectsV2Result result = null;
    final String keyPrefix = "<yourkeyPrefix>";
    
    // List the objects whose names contain the specified prefix by page,
    do {
        ListObjectsV2Request listObjectsV2Request = new ListObjectsV2Request(bucketName).withMaxKeys(maxKeys);
        listObjectsV2Request.setPrefix(keyPrefix);
        listObjectsV2Request.setContinuationToken(nextContinuationToken);
        result = ossClient.listObjectsV2(listObjectsV2Request);
    
        List<OSSObjectSummary> sums = result.getObjectSummaries();
        for (OSSObjectSummary s : sums) {
            System.out.println("\t"   s.getKey());
        }
    
        nextContinuationToken = result.getNextContinuationToken();
    
    } while (result1.isTruncated());
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
    ```

-   List objects and specify the encoding type of the object names

    **Note:** For more information, see [List objects and specify the encoding type of the object names](#URL).

    The following code provides an example on how to list objects and specify the encoding type of the object names in the response:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Specifies the maximum number of objects that can be listed each time.
    final int maxKeys = 200
    String nextContinuationToken = null;
    ListObjectsV2Result result = null;
    final String keyPrefix = "<yourkeyPrefix>";
    
    // Specify that the returned results are URL-encoded. In this case, you must decode the values of the prefix, delimiter, startAfter, key, and commonPrefix parameters in the response. 
    do {
        ListObjectsV2Request listObjectsV2Request = new ListObjectsV2Request(bucketName).withMaxKeys(maxKeys);
        listObjectsV2Request.setPrefix(keyPrefix);
        listObjectsV2Request.setEncodingType("url");
        listObjectsV2Request.setContinuationToken(nextContinuationtoken);
        result = ossClient.listObjectsV2(listObjectsV2Request);
    
        // Decode the value of prefix in the response.
        if (result.getPrefix() ! = null) {
            String prefix = URLDecoder.decode(result.getPrefix(), "UTF-8");
            System.out.println("prefix: "   prefix);
        }
    
        // Decode the value of delimiter in the response.
        if (result.getDelimiter() ! = null) {
            String delimiter = URLDecoder.decode(result.getDelimiter(), "UTF-8");
            System.out.println("delimiter: "   prefix);
        }
    
        // Decode the value of startAfter in the response.
        if (result.getStartAfter() ! = null) {
            String startAfter = URLDecoder.decode(result.getStartAfter(), "UTF-8");
            System.out.println("startAfter: "   prefix);
        }
    
        // Decode the value of key in the response.
        for (OSSObjectSummary s : result.getObjectSummaries()) {
            String decodedKey = URLDecoder.decode(s.getKey(), "UTF-8");
            System.out.println("key: "   decodedKey);
        }
    
        // Decode the value of commonPrefixes in the response.
        for (String commonPrefix: result.getCommonPrefixes()) {
            String commonPrefix = URLDecoder.decode(commonPrefix, "UTF-8");
            System.out.println("CommonPrefix: " commonPrefix);
        }
    
        nextContinuationToken = result.getNextContinuationToken();
    
    } while (result1.isTruncated());
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
    ```


## List objects by folder

OSS does not use a hierarchical structure for objects, but instead uses a flat structure. All elements are stored as objects in buckets. A folder is an object whose size is 0 and whose name ends with a forward slash \(/\). You can upload and download this object. By default, objects whose names end with a forward slash \(/\) is displayed as a folder in the OSS console.

You can use the delimiter and prefix parameters to list objects by folder.

-   If you set prefix to a folder name in the request, objects and subfolders whose names contain the prefix are listed.
-   If you also set delimiter to a forward slash \(/\) in the request, the objects and subfolders whose names start with the specified prefix in the folder are listed. Each subfolder is listed as a single result element in commonPrefixes. The objects and folders in these subfolders are not listed.

For more information about folders, see [Folder simulation](/intl.en-US/Developer Guide/Objects/Manage files/View the object list.md). For the complete code used to create a folder, visit [GitHub](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/CreateFolderSample.java).

Example: The following four objects are stored in the bucket: oss.jpg, fun/test.jpg, fun/movie/001.avi, and fun/movie/007.avi. The forward slash \(/\) is used as the folder delimiter. The following examples describe how to list objects by simulating folders.

-   List all objects in a bucket
    -   The following code provides an example on how to use GetBucket \(ListObjects\) to list all objects in a bucket:

        ```
        // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
        String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
        // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
        String accessKeyId = "<yourAccessKeyId>";
        String accessKeySecret = "<yourAccessKeySecret>";
        String bucketName = "<yourBucketName>";
        
        // Create an OSSClient instance.
        OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
        
        // Create a ListObjectsRequest.
        ListObjectsRequest listObjectsRequest = new ListObjectsRequest(bucketName);
        
        // List objects in the bucket.
        ObjectListing listing = ossClient.listObjects(listObjectsRequest);
        
        // Traverse all objects to display them.
        System.out.println("Objects:");
        for (OSSObjectSummary objectSummary : listing.getObjectSummaries()) {
            System.out.println(objectSummary.getKey());
        }
        
        // Traverse all commonPrefix elements to display them.
        System.out.println("CommonPrefixes:");
        for (String commonPrefix : listing.getCommonPrefixes()) {
            System.out.println(commonPrefix);
        }
        
        // Shut down the OSSClient instance.
        ossClient.shutdown();
                                    
        ```

    -   The following code provides an example on how to use GetBucketV2 \(ListObjectsV2\) to list 10 objects in a specified bucket:

        ```
        // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
        String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
        // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
        String accessKeyId = "<yourAccessKeyId>";
        String accessKeySecret = "<yourAccessKeySecret>";
        String bucketName = "<yourBucketName>";
        
        // Create an OSSClient instance.
        OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
        
        // List objects in the bucket.
        ListObjectsV2Request listObjectsV2Request = new ListObjectsV2Request(bucketName);
        ListObjectsV2Result result = ossClient.listObjectsV2(listObjectsV2Request);
        
        // Traverse the objects to display them.
        System.out.println("Objects:");
        for (OSSObjectSummary objectSummary : result.getObjectSummaries()) {
            System.out.println(objectSummary.getKey());
        }
        
        // Traverse all commonPrefix elements to display them.
        System.out.println("CommonPrefixes:");
        for (String commonPrefix : result.getCommonPrefixes()) {
            System.out.println(commonPrefix);
        }
        
        // Shut down the OSSClient instance.
        ossClient.shutdown();
                                    
        ```

    -   Response

        The following results are returned when you use the preceding two methods to list all objects in the bucket:

        ```
        Objects:
        fun/movie/001.avi
        fun/movie/007.avi
        fun/test.jpg
        oss.jpg
        CommonPrefixes:                    
        ```

-   List all objects within a specified folder
    -   The following code provides an example on how to use GetBucket \(ListObjects\) to list all objects within a specified folder:

        ```
        // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
        String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
        // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
        String accessKeyId = "<yourAccessKeyId>";
        String accessKeySecret = "<yourAccessKeySecret>";
        String bucketName = "<yourBucketName>";
        
        // Create an OSSClient instance.
        OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
        
        // Create a ListObjectsRequest.
        ListObjectsRequest listObjectsRequest = new ListObjectsRequest(bucketName);
        // Set prefix to fun/ to list all objects within the fun/ folder.
        listObjectsRequest.setPrefix("fun/");
        
        // Lists all objects within the fun/ folder.
        ObjectListing listing = ossClient.listObjects(listObjectsRequest);
        
        // Traverse all objects to display them.
        System.out.println("Objects:");
        for (OSSObjectSummary objectSummary : listing.getObjectSummaries()) {
            System.out.println(objectSummary.getKey());
        }
        
        // Traverse all commonPrefix elements to display them.
        System.out.println("\nCommonPrefixes:");
        for (String commonPrefix : listing.getCommonPrefixes()) {
            System.out.println(commonPrefix);
        }
        
        // Shut down the OSSClient instance.
        ossClient.shutdown();
                                    
        ```

    -   The following code provides an example on how to use GetBucketV2 \(ListObjectsV2\) to list all objects within a folder:

        ```
        // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
        String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
        // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
        String accessKeyId = "<yourAccessKeyId>";
        String accessKeySecret = "<yourAccessKeySecret>";
        String bucketName = "<yourBucketName>";
        
        // Create an OSSClient instance.
        OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
        
        // Create a ListObjectsV2Request.
        ListObjectsV2Request listObjectsV2Request = new ListObjectsV2Request(bucketName);
        
        // Set prefix to fun/ to list all objects within the fun/ folder.
        listObjectsV2Request.setPrefix("fun/");
        
        // Initiate a list request.
        ListObjectsV2Result result = ossClient.listObjectsV2(listObjectsV2Request);
        
        // Traverse the objects to display them.
        System.out.println("Objects:");
        for (OSSObjectSummary objectSummary : result.getObjectSummaries()) {
            System.out.println(objectSummary.getKey());
        }
        
        // Traverse all commonPrefix elements to display them.
        System.out.println("\nCommonPrefixes:");
        for (String commonPrefix : result.getCommonPrefixes()) {
            System.out.println(commonPrefix);
        }
        
        // Shut down the OSSClient instance.
        ossClient.shutdown();
        ```

    -   The following results are returned when you use the preceding two methods to list all objects within a folder:

        ```
        Objects:
        fun/movie/001.avi
        fun/movie/007.avi
        fun/test.jpg
        CommonPrefixes:                    
        ```

-   List objects and subfolders within a folder
    -   The following code provides an example on how to use GetBucket \(ListObjects\) to list objects and subfolders within a folder:

        ```
        // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
        String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
        // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
        String accessKeyId = "<yourAccessKeyId>";
        String accessKeySecret = "<yourAccessKeySecret>";
        String bucketName = "<yourBucketName>";
        
        // Create an OSSClient instance.
        OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
        
        // Create a ListObjectsRequest.
        ListObjectsRequest listObjectsRequest = new ListObjectsRequest(bucketName);
        
        // Set the delimiter to a forward slash (/).
        listObjectsRequest.setDelimiter("/");
        
        // List all objects and subfolders within the fun folder.
        listObjectsRequest.setPrefix("fun/");
        
        ObjectListing listing = ossClient.listObjects(listObjectsRequest);
        
        // Traverse all objects to display them.
        System.out.println("Objects:");
        // In the response, objectSummaries lists the objects within the fun folder.
        for (OSSObjectSummary objectSummary : listing.getObjectSummaries()) {
            System.out.println(objectSummary.getKey());
        }
        
        // Traverse all commonPrefix elements to display them.
        System.out.println("\nCommonPrefixes:");
        // commonPrefixs lists the subfolders within the fun folder. The fun/movie/001.avi and fun/movie/007.avi objects are not listed because they are in the movie subfolder with in the fun folder.
        for (String commonPrefix : listing.getCommonPrefixes()) {
            System.out.println(commonPrefix);
        }
        
        // Shut down the OSSClient instance.
        ossClient.shutdown();
                                    
        ```

    -   The following code provides an example on how to use GetBucketV2 \(ListObjectsV2\) to list objects and subfolders within a folder:

        ```
        // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
        String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
        // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
        String accessKeyId = "<yourAccessKeyId>";
        String accessKeySecret = "<yourAccessKeySecret>";
        String bucketName = "<yourBucketName>";
        
        // Create an OSSClient instance.
        OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
        
        // Create a ListObjectsV2Request.
        ListObjectsV2Request listObjectsV2Request = new ListObjectsV2Request(bucketName);
        
        // Set prefix to fun/ to list all objects and subfolders within the fun/ folder.
        listObjectsV2Request.setPrefix("fun/");
        
        // Set the delimiter to a forward slash (/).
        listObjectsV2Request.setDelimiter("/");
        
        // Initiate a list request.
        ListObjectsV2Result result = ossClient.listObjectsV2(listObjectsV2Request);
        
        // Traverse the objects to display them.
        System.out.println("Objects:");
        // In the response, objectSummaries lists the objects within the fun folder.
        for (OSSObjectSummary objectSummary : result.getObjectSummaries()) {
            System.out.println(objectSummary.getKey());
        }
        
        // Traverse all commonPrefix elements to display them.
        System.out.println("\nCommonPrefixes:");
        // commonPrefixs lists the subfolders within the fun folder. The fun/movie/001.avi and fun/movie/007.avi objects are not listed because they are in the movie subfolder within the fun folder.
        for (String commonPrefix : result.getCommonPrefixes()) {
            System.out.println(commonPrefix);
        }
        
        // Shut down the OSSClient instance.
        ossClient.shutdown();
        ```

    -   The following results are returned when you use the preceding two methods to list objects and subfolders within a folder:

        ```
        Objects:
        fun/test.jpg
        
        CommonPrefixes:
        fun/movie/                    
        ```

-   List the sizes of objects within a specified folder
    -   The following code provides an example on how to use GetBucket \(ListObjects\) to list the sizes of objects within a specified folder:

        ```
        // Query the sizes of objects within a specified folder in a bucket.
        private static long calculateFolderLength(OSS ossClient, String bucketName, String folder) {
             long size = 0L;
             ObjectListing objectListing = null;
             do {
                // The default value of MaxKeys is 100. The maximum value of MaxKeys is 1000.
                ListObjectsRequest request = new ListObjectsRequest(bucketName).withPrefix(folder).withMaxKeys(1000);
                if (objectListing ! = null) {
                     request.setMarker(objectListing.getNextMarker());
                }
                objectListing = ossClient.listObjects(request);
                List<OSSObjectSummary> sums = objectListing.getObjectSummaries();
                for (OSSObjectSummary s : sums) {
                     size  = s.getSize();
                }
            } while (objectListing.isTruncated());
              return size;
        }
        
        public void Calculate() {
              // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
              String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
              // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
              String accessKeyId = "<yourAccessKeyId>";
              String accessKeySecret = "<yourAccessKeySecret>";
              String bucketName = "<yourBucketName>";
              // Create an OSSClient instance.
              OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
              // Specify the prefix. If you want to traverse the sizes of objects and folders within the root folder, leave this parameter empty.
              final String keyPrefix = "<YourPrefix>";
              ObjectListing objectListing = null;
              do {
                  // By default, a maximum of 100 objects can be listed each time.
                  ListObjectsRequest request = new ListObjectsRequest(bucketName).withDelimiter("/").withPrefix(keyPrefix);
                  if (objectListing ! = null) {
                       request.setMarker(objectListing.getNextMarker());
                  }
                  objectListing = ossClient.listObjects(request);
                  List<String> folders = objectListing.getCommonPrefixes();
                  for (String folder : folders) {
                      System.out.println(folder   " : "   (calculateFolderLength(ossClient, bucketName, folder) / 1024)   "KB");
                  }
                  List<OSSObjectSummary> sums = objectListing.getObjectSummaries();
                  for (OSSObjectSummary s : sums) {
                      System.out.println(s.getKey()   " : "   (s.getSize() / 1024)   "KB");
                  }
               } while (objectListing.isTruncated());
               ossClient.shutdown();
        }
        ```

    -   The following code provides an example on how to use GetBucketV2 \(ListObjectsV2\) to list the sizes of objects within a specified folder.

        ```
        // Query the sizes of objects within a specified folder in a bucket.
        private satic long calculateFolderLength(OSS ossClient, String bucketName, String folder) {
            long size = 0L;
            ListObjectsV2Result result = null;
            do {
                // The default value of MaxKeys is 100. The maximum value of MaxKeys is 1000.
                ListObjectsV2Request request = new ListObjectsV2Request(bucketName).withPrefix(folder).withMaxKeys(1000);
                if (result ! = null) {
                    request.setContinuationToken(result.getNextContinuationToken());
                }
                result = ossClient.listObjectsV2(request);
                List<OSSObjectSummary> sums = result.getObjectSummaries();
                for (OSSObjectSummary s : sums) {
                    size  = s.getSize();
                }
            } while (result.isTruncated());
            return size;
        }
        
        public void Calculate() {
            // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
            String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
            // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
            String accessKeyId = "<yourAccessKeyId>";
            String accessKeySecret = "<yourAccessKeySecret>";
            String bucketName = "<yourBucketName>";
            // Create an OSSClient instance.
            OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
            // Specify the prefix. If you want to traverse the sizes of objects and folders within the root folder, leave this parameter empty.
            final String keyPrefix = "<YourPrefix>";
            ListObjectsV2Result result = null;
            do {
                // By default, a maximum of 100 objects can be listed each time.
                ListObjectsV2Request request = new ListObjectsV2Request(bucketName).withDelimiter("/").withPrefix(keyPrefix);
                if (result ! = null) {
                    request.setContinuationToken(result.getNextContinuationToken());
                }
                result = ossClient.listObjectsV2(request);
                List<String> folders = result.getCommonPrefixes();
                for (String folder : folders) {
                    System.out.println(folder   " : "   (calculateFolderLength(ossClient, bucketName, folder) / 1024)   "KB");
                }
                List<OSSObjectSummary> sums = result.getObjectSummaries();
                for (OSSObjectSummary s : sums) {
                    System.out.println(s.getKey()   " : "   (s.getSize() / 1024)   "KB");
                }
            } while (result.isTruncated());
            ossClient.shutdown();
        }
        ```


