# Multipart upload

By using the multipart upload feature provided by OSS, you can split a large object into multiple parts and upload them separately. After all parts are uploaded, call the CompleteMultipartUpload operation to combine these parts into a single object to implement resumable upload.

## Multipart upload process

To implement multipart upload, perform the following operations:

1.  Initiate a multipart upload task.

    You can call the Bucket.InitiateMultipartUpload method to obtain a unique upload ID in OSS.

2.  Upload parts.

    You can call the Bucket.UploadPart method to upload parts.

    **Note:**

    -   Part numbers identify the relative positions of parts in an object that share the same upload ID. If you have uploaded a part and used its part number again to upload another part, the latter part overwrites the former part.
    -   OSS includes the MD5 hash of part data in the ETag header and returns the MD5 hash to the user.
    -   OSS calculates the MD5 hash of uploaded data and compares the MD5 hash with the MD5 hash calculated by the SDK. If the two hashes are different, the InvalidDigest error code is returned.
3.  Complete the multipart upload task.

    After all parts are uploaded, call the Bucket.CompleteMultipartUpload method to combine the parts into a complete object.


## Complete sample code

The following code provides a complete example that describes the process of multipart upload:

```
package main

import (
    "fmt"
    "os"

    "github.com/aliyun/aliyun-oss-go-sdk/oss"
)

func main() {
    // Create an OSSClient instance. 
    // Set yourEndpoint to the endpoint of the region where the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use a Resource Access Management (RAM) user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
    client, err := oss.New("yourEndpoint", "yourAccessKeyId", "yourAccessKeySecret")
    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    // Specify the bucket name. 
    bucketName := "examplebucket"
    // Specify the full path of the object. The full path of the object cannot contain bucket names. 
    objectName := "exampleobject.txt"
    // Specify the full path of the local file. If the path of the local file is not specified, the local file is uploaded from the path of the project to which the sample program belongs. 
    locaFilename := "D:\\localpath\\examplefile.txt"

    // Obtain the bucket. 
    bucket, err := client.Bucket(bucketName)
    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    // Split the local file into three parts. 
    chunks, err := oss.SplitFileByPartNum(locaFilename, 3)
    fd, err := os.Open(locaFilename)
    defer fd.Close()

    // Set the storage class to Standard. 
    storageType := oss.ObjectStorageClass(oss.StorageStandard)

    // Step 1: Initiate a multipart upload task. Set the storage class to Standard. 
    imur, err := bucket.InitiateMultipartUpload(objectName, storageType)
    // Step 2: Upload parts. 
    var parts []oss.UploadPart
    for _, chunk := range chunks {
        fd.Seek(chunk.Offset, os.SEEK_SET)
        // Call the UploadPart method to upload each part. 
        part, err := bucket.UploadPart(imur, fd, chunk.Size, chunk.Number)
        if err != nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
        parts = append(parts, part)
    }

    // Set the access control list (ACL) of the object to public read. By default, the object inherits the ACL of the bucket. 
    objectAcl := oss.ObjectACL(oss.ACLPublicRead)

    // Step 3: Complete the multipart upload task. Set the ACL to public read. 
    cmur, err := bucket.CompleteMultipartUpload(imur, parts, objectAcl)
    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    fmt.Println("cmur:", cmur)
}
```

## Cancel a multipart upload task

You can call the bucket.AbortMultipartUpload method to cancel a multipart upload task. If you cancel a multipart upload task, you cannot use the upload ID to manage parts. The uploaded parts are deleted.

The following code provides an example on how to cancel a multipart upload task:

```
package main
import (
    "fmt"
    "os"
    "github.com/aliyun/aliyun-oss-go-sdk/oss"
)
func main() {
    // Create an OSSClient instance. 
    // Set yourEndpoint to the endpoint of the region where the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
    client, err := oss.New("yourEndpoint", "yourAccessKeyId", "yourAccessKeySecret")

    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    // Obtain the bucket. 
    // Specify the bucket name. 
    bucket, err := client.Bucket("examplebucket")
    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    // Initiate a multipart upload task. 
    // Specify the full path of the object. The full path of the object cannot contain bucket names. 
    imur, err := bucket.InitiateMultipartUpload("exampleobject.txt")
    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    // Cancel the multipart upload task. 
    err = bucket.AbortMultipartUpload(imur)
    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
}
```

## List uploaded parts

The following code provides an example on how to list the parts uploaded in a specified multipart upload task:

```
package main
import (
    "fmt"
    "os"
    "github.com/aliyun/aliyun-oss-go-sdk/oss"
)
func main() {
    // Create an OSSClient instance. 
    // Set yourEndpoint to the endpoint of the region where the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
    client, err := oss.New("yourEndpoint", "yourAccessKeyId", "yourAccessKeySecret")
    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
     // Specify the bucket name. 
    bucketName := "examplebucket"
    // Specify the full path of the object. The full path of the object cannot contain bucket names. 
    objectName := "exampleobject.txt"
    // Specify the full path of the local file. If the path of the local file is not specified, the local file is uploaded from the path of the project to which the sample program belongs. 
    locaFilename := "D:\\localpath\\examplefile.txt"
    // Specify the upload ID. 
    uploadID := "EBF2CAC46F1748B99376D795********"

    // Obtain the bucket. 
    bucket, err := client.Bucket(bucketName)
    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    // Split the local file into three parts. 
    chunks, err := oss.SplitFileByPartNum(locaFilename, 3)
    fd, err := os.Open(locaFilename)
    defer fd.Close()
    // Initiate a multipart upload task. 
    imur, err := bucket.InitiateMultipartUpload(objectName)
    uploadID = imur.UploadID
    fmt.Println("InitiateMultipartUpload UploadID: ", uploadID)
    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    // Upload parts. 
    var parts []oss.UploadPart
    for _, chunk := range chunks {
        fd.Seek(chunk.Offset, os.SEEK_SET)
        // Call the UploadPart method to upload each part. 
        part, err := bucket.UploadPart(imur, fd, chunk.Size, chunk.Number)
        if err != nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
        fmt.Println("UploadPartNumber: ", part.PartNumber, ", ETag: ", part.ETag)
        parts = append(parts, part)
    }
    // List uploaded parts based on InitiateMultipartUploadResult. 
    lsRes, err := bucket.ListUploadedParts(imur)
    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    // Display the uploaded parts.  
    fmt.Println("\nParts:", lsRes.UploadedParts)
    for _, upload := range lsRes.UploadedParts {
        fmt.Println("List PartNumber:  ", upload.PartNumber, ", ETag: " ,upload.ETag, ", LastModified: ", upload.LastModified)
    }
    // Specify objectName and uploadID to obtain the result by using the InitiateMultipartUploadResult operation. List all uploaded parts based on the result. 
    var imur_with_uploadid oss.InitiateMultipartUploadResult
    imur_with_uploadid.Key = objectName
    imur_with_uploadid.UploadID = uploadID
    // List uploaded parts. 
    lsRes, err = bucket.ListUploadedParts(imur_with_uploadid)
    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    // Display the uploaded parts. 
    fmt.Println("\nListUploadedParts by UploadID: ", uploadID)
    for _, upload := range lsRes.UploadedParts {
        fmt.Println("List PartNumber:  ", upload.PartNumber, ", ETag: " ,upload.ETag, ", LastModified: ", upload.LastModified)
    }
    // Complete the multipart upload task. 
    cmur, err := bucket.CompleteMultipartUpload(imur, parts)
    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    fmt.Println("cmur:", cmur)
}
```

## List multipart upload tasks

You can call the `Bucket.ListMultipartUploads` method to list all ongoing multipart upload tasks that are initiated but not completed or that are canceled. The following table describes the parameters that you can configure to list these tasks.

|Parameter|Description|
|:--------|:----------|
|Delimiter|The character used to group objects by name. Objects whose names contain the same string from the prefix and the next occurrence of the delimiter are grouped as a single result element in CommonPrefixes.|
|MaxUploads|The maximum number of multipart upload tasks to list in this query. The default value is 1000. The maximum value is 1000.|
|KeyMarker|The name of the object after which the list of multipart upload tasks begins. All multipart upload tasks with objects whose names are alphabetically after the KeyMarker parameter value are included in the list. You can use this parameter with the UploadIDMarker parameter to specify the start position to list the returned results.|
|Prefix|The prefix that returned object names must contain. Note that if you use a prefix for a query, the returned object name contains the prefix.|
|UploadIDMarker|The start position from which to list the returned results. This parameter is used together with the KeyMarker parameter. -   This parameter is ignored if the KeyMarker parameter is not set.
-   If the KeyMarker parameter is set, the response includes the following tasks:
    -   Multipart upload tasks whose object names are alphabetically after the KeyMarker parameter value.
    -   All multipart upload tasks in which the object names are alphabetically after the KeyMarker value and the upload IDs are greater than the UploadIDMarker value. |

-   List multipart upload tasks by using default parameter values

    ```
    package main
    import (
        "fmt"
        "os"
        "github.com/aliyun/aliyun-oss-go-sdk/oss"
    )
    func main() {
        // Create an OSSClient instance. 
        // Set yourEndpoint to the endpoint of the region where the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
        // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to Object Storage Service (OSS) because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
        client, err := oss.New("yourEndpoint", "yourAccessKeyId", "yourAccessKeySecret")
        if err != nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
        // Obtain the bucket. 
        // Specify the bucket name. 
        bucketName := "examplebucket"
        bucket, err := client.Bucket(bucketName)
        if err != nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
        // List all multipart upload tasks. 
        keyMarker := ""
        uploadIdMarker := ""
        for {
            // By default, up to 1,000 records can be listed each query.  
            lsRes, err := bucket.ListMultipartUploads(oss.KeyMarker(keyMarker), oss.UploadIDMarker(uploadIdMarker))
            if err != nil {
                fmt.Println("Error:", err)
                os.Exit(-1)
            }
            // Display multipart upload tasks.  
            for _, upload := range lsRes.Uploads {
                fmt.Println("Upload: ", upload.Key, ", UploadID: ",upload.UploadID)
            }
            if lsRes.IsTruncated {
                keyMarker = lsRes.NextKeyMarker
                uploadIdMarker = lsRes.NextUploadIDMarker
            } else {
                break
            }
        }
    }
    ```

-   Specify file as the prefix

    ```
        lsRes, err := bucket.ListMultipartUploads(oss.Prefix("file"))
        if err != nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
        fmt.Println("Uploads:", lsRes.Uploads)
    ```

-   List a maximum of 100 multipart upload tasks

    ```
        lsRes, err := bucket.ListMultipartUploads(oss.MaxUploads(100))
        if err != nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
        fmt.Println("Uploads:", lsRes.Uploads)
    ```

-   Specify file as the prefix and list a maximum of 100 multipart upload tasks

    ```
        lsRes, err := bucket.ListMultipartUploads(oss.Prefix("file"), oss.MaxUploads(100))
        if err != nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
        fmt.Println("Uploads:", lsRes.Uploads)
    ```


