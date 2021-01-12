# List objects

This topic describes how to list objects in a versioned bucket, including all objects, a specified number of objects, and objects whose names contain a specified prefix.

## List the versions of all objects in a bucket

The following code provides an example on how to list the versions of all objects including delete markers within a specified bucket:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// List the versions of all objects including delete markers within the bucket.
String nextKeyMarker = null;
String nextVersionMarker = null;
VersionListing versionListing = null;
do {
    ListVersionsRequest listVersionsRequest = new ListVersionsRequest()
            .withBucketName(bucketName)
            .withKeyMarker(nextKeyMarker)
            .withVersionIdMarker(nextVersionMarker);

    versionListing = ossClient.listVersions(listVersionsRequest);
    for (OSSVersionSummary ossVersion : versionListing.getVersionSummaries()) {
        System.out.println("key name: " + ossVersion.getKey());
        System.out.println("versionid: " + ossVersion.getVersionId());
        System.out.println("Is latest: " + ossVersion.isLatest());
        System.out.println("Is delete marker: " + ossVersion.isDeleteMarker());
    }

    nextKeyMarker = versionListing.getNextKeyMarker();
    nextVersionMarker = versionListing.getNextVersionIdMarker();
} while (versionListing.isTruncated());

// Shut down the OSSClient instance.
ossClient.shutdown();
```

## List the versions of objects whose names contain a specified prefix

The following code provides an example on how to list all versions of objects whose names contain a specified prefix:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Specify to return all versions of objects whose names contain the "test-" prefix.
String prefix = "test-";
String nextKeyMarker = null;
String nextVersionMarker = null;
VersionListing versionListing = null;
do {
    ListVersionsRequest listVersionsRequest = new ListVersionsRequest()
            .withBucketName(bucketName)
            .withKeyMarker(nextKeyMarker)
            .withVersionIdMarker(nextVersionMarker)
            .withEncodingType(OSSConstants.URL_ENCODING)
            .withPrefix(prefix);

    versionListing = ossClient.listVersions(listVersionsRequest);
    // Display the version information about the returned objects.
    for (OSSVersionSummary ossVersion : versionListing.getVersionSummaries()) {
        System.out.println("key name: " + ossVersion.getKey());
        System.out.println("versionid: " + ossVersion.getVersionId());
        System.out.println("Is latest: " + ossVersion.isLatest());
        System.out.println("Is delete marker: " + ossVersion.isDeleteMarker());
    }

    nextKeyMarker = versionListing.getNextKeyMarker();
    nextVersionMarker = versionListing.getNextVersionIdMarker();
} while (versionListing.isTruncated());

// Shut down the OSSClient instance.
ossClient.shutdown();
```

## List a specified number of object versions

The following code provides an example on how to list a specified number of object versions:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Specify to return up to 200 versions.
ListVersionsRequest listVersionsRequest = new ListVersionsRequest()
            .withBucketName(bucketName)
            .withMaxResults(200);

versionListing = ossClient.listVersions(listVersionsRequest);
for (OSSVersionSummary ossVersion : versionListing.getVersionSummaries()) {
    System.out.println("key name: " + ossVersion.getKey());
    // If versioning is not enabled for the bucket, the version IDs of the returned object are none.
    System.out.println("versionid: " + ossVersion.getVersionId());
    System.out.println("Is latest: " + ossVersion.isLatest());
    System.out.println("Is delete marker: " + ossVersion.isDeleteMarker());
}

// Shut down the OSSClient instance.
ossClient.shutdown();
```

## List the versions of all objects by page

The following code provides an example on how to list the versions of all objects by page:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// List the versions of all objects in the bucket by page.
String nextKeyMarker = null;
String nextVersionMarker = null;
VersionListing versionListing = null;
do {
    ListVersionsRequest listVersionsRequest = new ListVersionsRequest()
            .withBucketName(bucketName)
            .withKeyMarker(nextKeyMarker)
            .withVersionIdMarker(nextVersionMarker);

    versionListing = ossClient.listVersions(listVersionsRequest);
    for (OSSVersionSummary ossVersion : versionListing.getVersionSummaries()) {
        System.out.println("key name: " + ossVersion.getKey());
        System.out.println("versionid: " + ossVersion.getVersionId());
        System.out.println("Is latest: " + ossVersion.isLatest());
        System.out.println("Is delete marker: " + ossVersion.isDeleteMarker());
    }

    nextKeyMarker = versionListing.getNextKeyMarker();
    nextVersionMarker = versionListing.getNextVersionIdMarker();
} while (versionListing.isTruncated());

// Shut down the OSSClient instance.
ossClient.shutdown();
```

## List the versions of objects whose names contain special characters

If the name of an object contains any of the following special characters, you must encode the object name before you transmit the object. Only URL encoding is supported in OSS.

-   Single quotation marks \('\)
-   Double quotations marks \("\)
-   Ampersands \(&\)
-   Angle brackets \(<\>\)
-   Pause markers \(、\)
-   Chinese characters

The following code provides an example on how to list objects whose names contain special characters:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Specify that the object names in the response are URL-encoded.
String nextKeyMarker = null;
String nextVersionMarker = null;
VersionListing versionListing = null;
do {
    ListVersionsRequest listVersionsRequest = new ListVersionsRequest()
            .withBucketName(bucketName)
            .withKeyMarker(nextKeyMarker)
            .withVersionIdMarker(nextVersionMarker)
            .withEncodingType(OSSConstants.URL_ENCODING)

    versionListing = ossClient.listVersions(listVersionsRequest);
      // Display the information about the versions of returned objects.
    for (OSSVersionSummary ossVersion : versionListing.getVersionSummaries()) {
        System.out.println("key name: " + ossVersion.getKey());
        System.out.println("versionid: " + ossVersion.getVersionId());
        System.out.println("Is latest: " + ossVersion.isLatest());
        System.out.println("Is delete marker: " + ossVersion.isDeleteMarker());
    }

    nextKeyMarker = versionListing.getNextKeyMarker();
    nextVersionMarker = versionListing.getNextVersionIdMarker();
} while (versionListing.isTruncated());

// Shut down the OSSClient instance.
ossClient.shutdown();
```

## List objects by folder

OSS does not use a hierarchical structure for objects, but instead uses a flat structure. All elements are stored as objects in buckets. A folder is an object whose size is 0 and whose name ends with a forward slash \(/\). You can upload and download this object. By default, objects whose names end with a forward slash \(/\) is displayed as a folder in the OSS console.

You can specify the delimiter and prefix parameters to list objects by folder.

-   If you set prefix to a folder name in the request, objects and subfolders whose names contain the prefix are listed.
-   If you also set delimiter to a forward slash \(/\) in the request, the objects and subfolders whose names start with the specified prefix in the folder are listed. Each subfolder is listed as a single result element in commonPrefixes. The objects and folders in these subfolders are not listed.

Example: The following four objects are stored in the bucket: oss.jpg, fun/test.jpg, fun/movie/001.avi, and fun/movie/007.avi. The forward slash \(/\) is specified as the folder delimiter. The following examples describe how to list objects in simulated folders.

-   List the versions of objects within the root folder of a bucket

    The following code provides an example on how to list the versions of object within the root folder of a bucket:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Set the delimiter parameter to a forward slash (/) to list the versions of objects and the names of subfolders within the root folder.
    String delimiter = "/";
    String nextKeyMarker = null;
    String nextVersionMarker = null;
    VersionListing versionListing = null;
    do {
        ListVersionsRequest listVersionsRequest = new ListVersionsRequest()
                .withBucketName(bucketName)
                .withKeyMarker(nextKeyMarker)
                .withVersionIdMarker(nextVersionMarker)
                .withEncodingType(OSSConstants.URL_ENCODING)
                .withDelimiter(delimiter);
    
        versionListing = ossClient.listVersions(listVersionsRequest);
        // Display the information about the versions of returned objects.
        for (OSSVersionSummary ossVersion : versionListing.getVersionSummaries()) {
            System.out.println("key name: " + ossVersion.getKey());
            System.out.println("versionid: " + ossVersion.getVersionId());
            System.out.println("Is latest: " + ossVersion.isLatest());
            System.out.println("Is delete marker: " + ossVersion.isDeleteMarker());
        }
        // Display the name of folders whose names are end with a forward slash (/) in the root folder.
        for (String commonPrefix : versionListing.getCommonPrefixes()) {
            System.out.println("commonPrefix: " + commonPrefix);
        }
    
        nextKeyMarker = versionListing.getNextKeyMarker();
        nextVersionMarker = versionListing.getNextVersionIdMarker();
    } while (versionListing.isTruncated());
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
    ```

    Returned results

    ```
    key name: oss.jpg
    versionid: CAEQEhiBgMCw8Y7FqBciIGIzMDE3MTEzOWRiMDRmZmFhMmRlMjljZWI0MWU4****
    Is latest: true
    Is delete marker: false
    
    commonPrefix: fun/
    ```

-   List objects and subfolders within a folder

    The following code provides an example on how to list the objects and subfolders within a specified folder:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Configure the prefix parameter to list all objects and subfolders within the fun folder. Set the delimiter parameter to a forward slash (/).
    String prefix = "fun/";
    String delimiter = "/";
    String nextKeyMarker = null;
    String nextVersionMarker = null;
    VersionListing versionListing = null;
    do {
        ListVersionsRequest listVersionsRequest = new ListVersionsRequest()
                .withBucketName(bucketName)
                .withKeyMarker(nextKeyMarker)
                .withVersionIdMarker(nextVersionMarker)
                .withEncodingType(OSSConstants.URL_ENCODING)
                .withDelimiter(delimiter)
                .withPrefix(prefix);
    
        versionListing = ossClient.listVersions(listVersionsRequest);
        // Display the information about the versions of returned objects.
        for (OSSVersionSummary ossVersion : versionListing.getVersionSummaries()) {
            System.out.println("key name: " + ossVersion.getKey());
            System.out.println("versionid: " + ossVersion.getVersionId());
            System.out.println("Is latest: " + ossVersion.isLatest());
            System.out.println("Is delete marker: " + ossVersion.isDeleteMarker());
        }
        // Display the names of the subfolders within the fun folder.
        for (String commonPrefix : versionListing.getCommonPrefixes()) {
            System.out.println("commonPrefix: " + commonPrefix);
        }
    
        nextKeyMarker = versionListing.getNextKeyMarker();
        nextVersionMarker = versionListing.getNextVersionIdMarker();
    } while (versionListing.isTruncated());
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
    ```

    Returned results

    ```
    key name: fun/test.jpg
    versionid: CAEQEhiBgIC48Y7FqBciIDU4NjAzMjczMTY5NDRjYmVhNGY4NTM2YTdjY2Ji****
    Is latest: true
    Is delete marker: false
    
    commonPrefix: fun/movie/
    ```


