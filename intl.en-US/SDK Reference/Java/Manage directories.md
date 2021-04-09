# Manage directories

After the hierarchical namespace feature is enabled for a bucket, you can create, rename, and delete a directory for the bucket. This topic describes how to create, rename, and delete a directory for a bucket.

**Note:** For more information about how to enable the hierarchical namespace feature for a bucket, see [Create buckets](/intl.en-US/SDK Reference/Java/Buckets/Create buckets.md).

## Create a directory

The following code provides an example on how to create a directory named exampledir for examplebucket:

```
// Set yourEndpoint to the endpoint of the region where the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com.
String endpoint = "yourEndpoint";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "yourAccessKeyId";
String accessKeySecret = "yourAccessKeySecret";
// Specify the bucket name.
String bucketName = "examplebucket";
// Specify the absolute path of the directory. The absolute path of the directory cannot contain bucket names.
String directoryName = "exampledir";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Create a directory.
CreateDirectoryRequest createDirectoryRequest = new CreateDirectoryRequest(bucketName, directoryName);
ossClient.createDirectory(createDirectoryRequest);

// Shut down the OSSClient instance.
ossClient.shutdown();
```

For more information about how to create a directory, see [CreateDirectory](/intl.en-US/API Reference/Object operations/Directory management/CreateDirectory.md).

## Rename a directory

After the hierarchical feature is enabled for a bucket, you can rename directories in the bucket.

The following code provides an example on how to rename the exampledir directory newexampledir for examplebucket:

```
// Set yourEndpoint to the endpoint of the region where the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com.
String endpoint = "yourEndpoint";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "yourAccessKeyId";
String accessKeySecret = "yourAccessKeySecret";
// Specify the bucket name.
String bucketName = "examplebucket";
// Specify the absolute path of the source directory. The absolute path of the directory cannot contain bucket names.
String sourceDir = "exampledir";
// Specify the absolute path of the destination directory that is in the same bucket as the source directory. The absolute path of the directory cannot contain bucket names.
String destnationDir = "newexampledir";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Change the absolute path of the source directory to that of the destination directory.
RenameObjectRequest renameObjectRequest = new RenameObjectRequest(bucketName, sourceDir, destnationDir);
ossClient.renameObject(renameObjectRequest);

// Shut down the OSSClient instance.
ossClient.shutdown();
```

For more information about how to rename a directory, see [Rename](/intl.en-US/API Reference/Object operations/Directory management/Rename.md).

## Delete a directory

After the hierarchical namespace feature is enabled for a bucket, you can use the non-recursive delete or recursive delete method to delete directories from the bucket.

-   Non-recursive delete

    If you use the non-recursive delete method to delete a directory, you can delete the directory only when the directory is empty.

    The following code provides an example on how to use the non-recursive delete method to delete the exampledir directory from examplebucket:

    ```
    // Set yourEndpoint to the endpoint of the region where the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com.
    String endpoint = "yourEndpoint";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "yourAccessKeyId";
    String accessKeySecret = "yourAccessKeySecret";
    // Specify the bucket name.
    String bucketName = "examplebucket";
    // Specify the absolute path of the directory. The absolute path of the directory cannot contain bucket names.
    String directoryName = "exampledir";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Delete the directory. By default, the non-recursive delete method is used. Make sure that all objects and subdirectories are deleted from this directory.
    DeleteDirectoryRequest deleteDirectoryRequest = new DeleteDirectoryRequest(bucketName, directoryName);
    DeleteDirectoryResult deleteDirectoryResult = ossClient.deleteDirectory(deleteDirectoryRequest);
    
    // Display the absolute path of the deleted directory.
    System.out.println("delete dir name :" + deleteDirectoryResult.getDirectoryName());
    // Display the total number of deleted objects and directories.
    System.out.println("delete number:" + deleteDirectoryResult.getDeleteNumber());
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
    ```

-   Recursive delete

    If you use the recursive delete method to delete a directory, you can delete the directory along with the objects and subdirectories from this directory. Exercise caution when you use recursive delete.

    The following code provides an example on how to use the recursive delete method to delete the exampledir directory as well as the objects and subdirectories from examplebucket:

    ```
    // Set yourEndpoint to the endpoint of the region where the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com.
    String endpoint = "yourEndpoint";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "yourAccessKeyId";
    String accessKeySecret = "yourAccessKeySecret";
    // Specify the bucket name.
    String bucketName = "examplebucket";
    // Specify the absolute path of the directory. The absolute path of the directory cannot contain bucket names.
    String directoryName = "exampledir";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Use the recursive delete method to delete the directory.
    DeleteDirectoryRequest deleteDirectoryRequest = new DeleteDirectoryRequest(bucketName, directoryName);
    deleteDirectoryRequest.setDeleteRecursive(true);
    DeleteDirectoryResult deleteDirectoryResultossClient.deleteDirectory(deleteDirectoryRequest);
        
    // Display the deletion result.
    // The maximum total number of directories and objects that can be deleted at a time is 100. If only part of directories and objects are returned, the server returns nextDeleteToken. You can use nextDeleteToken to continue deleting remaining data.
    // nextDeleteToken helps the server find the object or directory after which the delete operation begins.
    String nextDeleteToken = deleteDirectoryResult.getNextDeleteToken();
    System.out.println("delete next token:" + nextDeleteToken);
    // Display the absolute path of the deleted directory.
    System.out.println("delete dir name :" + deleteDirectoryResult.getDirectoryName());
    // Display the total number of deleted objects and directories.
    System.out.println("delete number:" + deleteDirectoryResult.getDeleteNumber());
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
    ```


For more information about how to delete a directory, see [DeleteDirectory](/intl.en-US/API Reference/Object operations/Directory management/DeleteDirectory.md).

