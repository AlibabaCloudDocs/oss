# List objects

This topic describes how to list all objects, objects whose names contain a specified prefix, and objects and subfolders within a specified folder of a bucket.

## Background information

You can call the GetBucket \(ListObjects\) or GetBucketV2 \(ListObjectsV2\) operation to list up to 1,000 objects in a bucket. You can configure the parameters to use different methods to list objects. For example, you can configure the parameters to list objects from a specified start position, list objects and subfolders in specified folders, and list objects by page. The GetBucket \(ListObjects\) and GetBucketV2 \(ListObjectsV2\) operations are different in the following aspects:

-   By default, when you use the GetBucket \(ListObjects\) operation to list objects, the bucket owner information is included in the response.
-   When you use the GetBucketV2 \(ListObjectsV2\) operation to list objects, you can set the fetchOwner parameter to specify whether to include the information about the bucket owner in the response.

    **Note:** To list objects in a versioned bucket, we recommend that you use the GetBucketV2 \(ListObjectsV2\) operation.


The following tables describe the parameters that you must configure when you use the GetBucket \(ListObjects\) or GetBucketV2 \(ListObjectsV2\) operation to list objects.

-   Use GetBucket \(ListObjects\) to list objects

    The following table describes the parameter you can configure when you use GetBucket \(ListObjects\) to list objects.

    |Parameter|Description|
    |:--------|:----------|
    |delimiter|The character used to group objects by name. Objects whose names contain the same string ranged from the prefix to the next occurrence of the delimiter are grouped as a single result element in commonPrefixes.|
    |prefix|The prefix that the names of returned objects must contain.|
    |maxKeys|The maximum number of objects that can be listed at a time. Default value: 100. Maximum value: 1000.|
    |marker|The position from which the list operation starts. All objects whose names are alphabetically greater than the value of this parameter are returned.|

    For more information, see [GetBucket \(ListObjects\)](/intl.en-US/API Reference/Bucket operations/Basic operations/GetBucket (ListObjects).md).

-   Use GetBucketV2 \(ListObjectsV2\) to list objects

    The following table describes the parameters you can configure when you use GetBucketV2 \(ListObjectsV2\) to list objects.

    |Parameter|Description|
    |:--------|:----------|
    |prefix|The prefix that the names of returned objects must contain.|
    |delimiter|The character used to group objects by name. Objects whose names contain the same string ranged from the prefix to the next occurrence of the delimiter are grouped as a single result element in commonPrefixes.|
    |startAfter|The position from which the list operation starts. All objects whose names are alphabetically greater than the value of this parameter are returned.|
    |fetchOwner|Specifies whether to include the owner information in the response.     -   true: The response includes the owner information.
    -   false: The response excludes the owner information. |

    For more information, see [GetBucketV2 \(ListObjectsV2\)](/intl.en-US/API Reference/Bucket operations/Basic operations/GetBucketV2 (ListObjectsV2).md).


## Simple list

-   Use GetBucket \(ListObjects\) to list objects

    The following code provides an example on how to use GetBucket \(ListObjects\) to list 100 objects in a specified bucket:

    ```
    package main
    
    import (
        "fmt"
        "os"
        "github.com/aliyun/aliyun-oss-go-sdk/oss"
    )
    
    func HandleError(err error) {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    
    func main() {
        // Create an OSSClient instance.
        client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
        if err ! = nil {
            HandleError(err)
        }
    
        // Obtain the bucket.
        bucketName := "<yourBucketName>"
        bucket, err := client.Bucket(bucketName)
        if err ! = nil {
            HandleError(err)
        }
    
        // List all objects.
        marker := ""
        for {
            lsRes, err := bucket.ListObjects(oss.Marker(marker))
            if err ! = nil {
                HandleError(err)
            }
    
            // Display the listed objects. // By default, up to 100 objects are listed each time. 
            for _, object := range lsRes.Objects {
                fmt.Println("Bucket: ", object.Key)
            }
    
            if lsRes.IsTruncated {
                marker = lsRes.NextMarker
            } else {
                break
            }
        }
    }
                
    ```

-   Use GetBucketV2 \(ListObjectsV2\) to list objects

    The following code provides an example on how to use GetBucketV2 \(ListObjectsV2\) to list 100 objects in a specified bucket:

    ```
    package main
    
    import (
        "fmt"
        "os"
        "github.com/aliyun/aliyun-oss-go-sdk/oss"
    )
    
    func HandleError(err error) {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    
    func main() {
        // Create an OSSClient instance.
        client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
        if err ! = nil {
            HandleError(err)
        }
    
        // Obtain the bucket.
        bucketName := "<yourBucketName>"
        bucket, err := client.Bucket(bucketName)
        if err ! = nil {
            HandleError(err)
        }
    
        continueToken := ""
        for {
            lsRes, err := bucket.ListObjectsV2(oss.ContinuationToken(continueToken))
            if err ! = nil {
                HandleError(err)
            }
    
            // Display the listed objects. // By default, up to 100 objects are listed each time.
            for _, object := range lsRes.Objects {
                fmt.Println(object.Key, object.Type, object.Size, object.ETag, object.LastModified, object.StorageClass)
            }
    
            if lsRes.IsTruncated {
                continueToken = lsRes.NextContinuationToken
            } else {
                break
            }
        }
    }
    ```


## List a specified number of objects

-   Use GetBucket \(ListObjects\) to list a specified number of objects

    The following code provides an example on how to use GetBucket \(ListObjects\) to list a specified number of objects:

    ```
    package main
    
    import (
        "fmt"
        "os"
        "github.com/aliyun/aliyun-oss-go-sdk/oss"
    )
    
    func main() {
        // Create an OSSClient instance.
        client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
    
        bucketName := "<yourBucketName>"
    
        // Obtain the bucket.
        bucket, err := client.Bucket(bucketName)
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
    
        // Set the maximum number of objects that can be listed each time and list the objects.
        lsRes, err := bucket.ListObjects(oss.MaxKeys(200))
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
    
        // Display the listed objects. // By default, up to 100 objects are listed each time.
        fmt.Println("Objects:", lsRes.Objects)
        for _, object := range lsRes.Objects {
            fmt.Println("Object:", object.Key)
        }
    }
                
    ```

-   Use GetBucketV2 \(ListObjectsV2\) to list a specified number of objects

    The following code provides an example on how to use GetBucketV2 \(ListObjectsV2\) to list a specified number of objects:

    ```
    package main
    
    import (
        "fmt"
        "os"
        "github.com/aliyun/aliyun-oss-go-sdk/oss"
    )
    
    func HandleError(err error) {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    
    func main() {
        // Create an OSSClient instance.
        client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
        if err ! = nil {
            HandleError(err)
        }
    
        // Obtain the bucket.
        bucketName := "<yourBucketName>"
        bucket, err := client.Bucket(bucketName)
        if err ! = nil {
            HandleError(err)
        }
        
        // Set the MaxKeys parameter to configure the maximum number of objects that can be listed each time and list the objects.
        lsRes, err := bucket.ListObjectsV2(oss.MaxKeys(200))
        if err ! = nil {
            HandleError(err)
        }
    
        // Display the listed objects. // By default, up to 100 objects are listed each time.
        for _, object := range lsRes.Objects {
            fmt.Println(object.Key, object.Type, object.Size, object.ETag, object.LastModified, object.StorageClass, object.Owner.ID, object.Owner.DisplayName)
        }
    }
    ```


## List objects whose names contain a specified prefix

-   Use GetBucket \(ListObjects\) to list the sizes of objects whose names contain a specified prefix

    The following code provides an example on how to use GetBucket \(ListObjects\) to list objects whose names contain a specified prefix:

    ```
    package main
    
    import (
        "fmt"
        "os"
        "github.com/aliyun/aliyun-oss-go-sdk/oss"
    )
    
    func main() {
        // Create an OSSClient instance.
        client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
    
        bucketName := "<yourBucketName>"
    
        // Obtain the bucket.
        bucket, err := client.Bucket(bucketName)
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
    
        // List objects whose names contain the specified prefix. By default, up to 100 objects are listed.
        lsRes, err := bucket.ListObjects(oss.Prefix("my-object-"))
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
    
        // Display the listed objects. // By default, up to 100 objects are listed each time.
        fmt.Println("Objects:", lsRes.Objects)
        for _, object := range lsRes.Objects {
            fmt.Println("Object:", object.Key)
        }
    }
                
    ```

-   Use GetBucketV2 \(ListObjectsV2\) to list objects whose names contain a specified prefix

    The following code provides an example on how to use GetBucketV2 \(ListObjectsV2\) to list objects whose names contain a specified prefix:

    ```
    package main
    
    import (
        "fmt"
        "os"
        "github.com/aliyun/aliyun-oss-go-sdk/oss"
    )
    
    func HandleError(err error) {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    
    func main() {
        // Create an OSSClient instance.
        client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
        if err ! = nil {
            HandleError(err)
        }
    
        // Obtain the bucket.
        bucketName := "<yourBucketName>"
        bucket, err := client.Bucket(bucketName)
        if err ! = nil {
            HandleError(err)
        }
        
        // Set the Prefix parameter to my-object-.
        lsRes, err := bucket.ListObjectsV2(oss.Prefix("my-object-"))
        if err ! = nil {
            HandleError(err)
        }
    
        // Display the listed objects. // By default, up to 100 objects are listed each time.
        for _, object := range lsRes.Objects {
            fmt.Println(object.Key, object.Type, object.Size, object.ETag, object.LastModified, object.StorageClass, object.Owner.ID, object.Owner.DisplayName)
        }
    }
    ```


## List all objects from a specified start position

-   Use GetBucket \(ListObjects\) to list objects from a specified start position.

    You can configure the Marker parameter to specify the start position from which the list operation starts. All objects whose names are alphabetically greater than the value of Marker are returned.

    ```
    package main
    
    import (
        "fmt"
        "os"
        "github.com/aliyun/aliyun-oss-go-sdk/oss"
    )
    
    func main() {
        // Create an OSSClient instance.
        client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
    
        bucketName := "<yourBucketName>"
    
        // Obtain the bucket.
        bucket, err := client.Bucket(bucketName)
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
    
        // Specify the Marker parameter to list objects whose names are alphabetically greater than the value of Marker. By default, up to 100 objects are listed.
        lsRes, err := bucket.ListObjects(oss.Marker("my-object-xx"))
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
    
        // Display the listed objects. // By default, up to 100 objects are listed each time.
        fmt.Println("Objects:", lsRes.Objects)
        for _, object := range lsRes.Objects {
            fmt.Println("Object:", object.Key)
        }
    }
                
    ```

-   Use GetBucketV2 \(ListObjectsV2\) to list objects from a specified start position

    You can configure the StartAfter parameter to specify the start position from which the list operation starts. All objects whose names are alphabetically greater than the value of StartAfter are returned.

    ```
    package main
    
    import (
        "fmt"
        "os"
        "github.com/aliyun/aliyun-oss-go-sdk/oss"
    )
    
    func HandleError(err error) {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    
    func main() {
        // Create an OSSClient instance.
        client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
        if err ! = nil {
            HandleError(err)
        }
    
        // Obtain the bucket.
        bucketName := "<yourBucketName>"
        bucket, err := client.Bucket(bucketName)
        if err ! = nil {
            HandleError(err)
        }
        
        // Specify the StartAfter parameter to list objects whose names are alphabetically greater than the value of StartAfter. By default, up to 100 objects are listed.
        lsRes, err := bucket.ListObjectsV2(oss.StartAfter("my-object-"))
        if err ! = nil {
            HandleError(err)
        }
    
        // Display the listed objects. // By default, up to 100 objects are listed each time.
        for _, object := range lsRes.Objects {
            fmt.Println(object.Key, object.Type, object.Size, object.ETag, object.LastModified, object.StorageClass, object.Owner.ID, object.Owner.DisplayName)
        }
    }
    ```


## List all objects by page

-   Use GetBucket \(ListObjects\) to list all objects by page

    The following code provides an example on how to use GetBucket \(ListObjects\) to list all objects within a specified bucket by page: You can configure the MaxKeys parameter to specify the maximum number of objects that can be listed on each page.

    ```
    package main
    
    import (
        "fmt"
        "os"
        "github.com/aliyun/aliyun-oss-go-sdk/oss"
    )
    
    func main() {
        // Create an OSSClient instance.
        client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
    
        bucketName := "<yourBucketName>"
    
        // Obtain the bucket.
        bucket, err := client.Bucket(bucketName)
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
    
        // List all objects by page, with up to 100 objects on each page.
        marker := oss.Marker("")
        for {
            lsRes, err := bucket.ListObjects(oss.MaxKeys(100), marker)
            if err ! = nil {
                fmt.Println("Error:", err)
                os.Exit(-1)
            }
            marker = oss.Marker(lsRes.NextMarker)
            // Display the listed objects. // By default, up to 100 objects are listed each time.
            fmt.Println("Objects:", lsRes.Objects)
    
            if ! lsRes.IsTruncated {
                break
            }
        }
    }
                
    ```

-   Use GetBucketV2 \(ListObjectsV2\) to list objects and subfolders in a specified folder

    The following code provides an example on how to use GetBucketV2 \(ListObjectsV2\) to list all objects in a specified bucket by page: You can configure the MaxKeys parameter to specify the maximum number of objects that can be listed on each page.

    ```
    package main
    
    import (
        "fmt"
        "os"
        "github.com/aliyun/aliyun-oss-go-sdk/oss"
    )
    
    func HandleError(err error) {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    
    func main() {
        // Create an OSSClient instance.
        client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
        if err ! = nil {
            HandleError(err)
        }
    
        // Obtain the bucket.
        bucketName := "<yourBucketName>"
        bucket, err := client.Bucket(bucketName)
        if err ! = nil {
            HandleError(err)
        }
    
        // List all objects by page, with up to 100 objects on each page.
        continueToken := ""
        for {
            lsRes, err := bucket.ListObjectsV2(oss.MaxKeys(100), oss.ContinuationToken(continueToken))
            if err ! = nil {
                HandleError(err)
            }
    
            // Display the listed objects. // By default, up to 100 objects are listed each time.
            for _, object := range lsRes.Objects {
                fmt.Println(object.Key, object.Type, object.Size, object.ETag, object.LastModified, object.StorageClass)
            }
    
            if lsRes.IsTruncated {
                continueToken = lsRes.NextContinuationToken
            } else {
                break
            }
        }
    }
    ```


## List objects whose names contain a specified prefix by page

-   Use GetBucket \(ListObjects\) to list objects whose names contain a specified prefix by page

    The following code provides an example on how to use GetBucket \(ListObjects\) to list objects whose names contain a specified prefix by page: You can configure the MaxKeys parameter to specify the maximum number of objects that can be listed on each page.

    ```
    package main
    
    import (
        "fmt"
        "os"
        "github.com/aliyun/aliyun-oss-go-sdk/oss"
    )
    
    func main() {
        // Create an OSSClient instance.
        client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
    
        bucketName := "<yourBucketName>"
    
        // Obtain the bucket.
        bucket, err := client.Bucket(bucketName)
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
    
        // List the objects whose names contain the specified prefix by page, with up to 80 objects on each page.
        prefix := oss.Prefix("my-object-")
        marker := oss.Marker("")
        for {
            lsRes, err := bucket.ListObjects(oss.MaxKeys(80), marker, prefix)
            if err ! = nil {
                fmt.Println("Error:", err)
                os.Exit(-1)
            }
    
            prefix = oss.Prefix(lsRes.Prefix)
            marker = oss.Marker(lsRes.NextMarker)
            // Display the listed objects. // By default, up to 100 objects are listed each time.
            fmt.Println("Objects:", lsRes.Objects)
    
            if ! lsRes.IsTruncated {
                break
            }
        }
    }
                
    ```

-   Use GetBucketV2 \(ListObjectsV2\) to list objects whose names contain a specified prefix by page

    The following code provides an example on how to use GetBucketV2 \(ListObjectsV2\) to list objects whose names contain a specified prefix by page: You can configure the MaxKeys parameter to specify the maximum number of objects that can be listed on each page.

    ```
    package main
    
    import (
        "fmt"
        "os"
        "github.com/aliyun/aliyun-oss-go-sdk/oss"
    )
    
    func HandleError(err error) {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    
    func main() {
        // Create an OSSClient instance.
        client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
        if err ! = nil {
            HandleError(err)
        }
    
        // Obtain the bucket.
        bucketName := "<yourBucketName>"
        bucket, err := client.Bucket(bucketName)
        if err ! = nil {
            HandleError(err)
        }
    
        // List the objects whose names contain the specified prefix by page, with up to 80 objects on each page.
        prefix := "my-object-"
        continueToken := ""
        for {
            lsRes, err := bucket.ListObjectsV2(oss.Prefix(prefix), oss.MaxKeys(80), oss.ContinuationToken(continueToken))
            if err ! = nil {
                HandleError(err)
            }
    
            // Display the listed objects. // By default, up to 100 objects are listed each time.
            for _, object := range lsRes.Objects {
                fmt.Println(object.Key, object.Type, object.Size, object.ETag, object.LastModified, object.StorageClass)
            }
    
            if lsRes.IsTruncated {
                continueToken = lsRes.NextContinuationToken
            } else {
                break
            }
        }
    }
    ```


## List the information about all objects within a specified folder

-   Use GetBucket \(ListObjects\) to list the information about all objects within a specified folder

    The following code provides an example on how to use GetBucket \(ListObjects\) to list the information about all objects within a specified folder, including the size, last modified time, and name of the objects:

    ```
    package main
    
    import (
        "fmt"
        "os"
        "github.com/aliyun/aliyun-oss-go-sdk/oss"
    )
    
    func main() {
        // Create an OSSClient instance.
        client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
    
        // Obtain the bucket.
        bucket, err := client.Bucket("<yourBucketName>")
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
    
        // Traverse the objects to list them.
        marker := oss.Marker("")
        prefix := oss.Prefix("<yourObjectPrefix>")
        for {
            lor, err := bucket.ListObjects(marker, prefix)
            if err ! = nil {
                fmt.Println("Error:", err)
                os.Exit(-1)
            }
            
            for _, object := range lor.Objects {
                fmt.Printf("%s %d %s\n", object.LastModified, object.Size, object.Key)
            }
    
            prefix = oss.Prefix(lor.Prefix)
            marker = oss.Marker(lor.NextMarker)
            if ! lor.IsTruncated {
                break
            }
        }
    }
    ```

-   Use GetBucketV2 \(ListObjectsV2\) to list the information about all objects within a specified folder

    The following code provides an example on how to use GetBucketV2 \(ListObjectsV2\) to list the information about all objects within a specified folder:

    ```
    package main
    
    import (
        "fmt"
        "os"
        "github.com/aliyun/aliyun-oss-go-sdk/oss"
    )
    
    func HandleError(err error) {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    
    func main() {
        // Create an OSSClient instance.
        client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
        if err ! = nil {
            HandleError(err)
        }
    
        // Obtain the bucket.
        bucketName := "<yourBucketName>"
        bucket, err := client.Bucket(bucketName)
        if err ! = nil {
            HandleError(err)
        }
    
        continueToken := ""
        prefix := "my-object-"
        for {
            lsRes, err := bucket.ListObjectsV2(oss.Prefix(prefix), oss.ContinuationToken(continueToken))
            if err ! = nil {
                HandleError(err)
            }
    
            // Display the listed objects. By default, a maximum of 100 objects are returned each time.
            for _, object := range lsRes.Objects {
                fmt.Println(object.Key, object.Type, object.Size, object.ETag, object.LastModified, object.StorageClass)
            }
    
            if lsRes.IsTruncated {
                continueToken = lsRes.NextContinuationToken
            } else {
                break
            }
        }
    }
    ```


## List the information about all subfolders within a specified folder

-   Use GetBucket \(ListObjects\) to list the information about all subfolders within a specified folder

    The following code provides an example on how to use GetBucket \(ListObjects\) to list the information about all subfolders within a specified folder:

    ```
    import (
        "fmt"
        "os"
        "github.com/aliyun/aliyun-oss-go-sdk/oss"
    )
    
    func main() {
        // Create an OSSClient instance.
        client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
    
        // Obtain the bucket.
        bucket, err := client.Bucket("<yourBucketName>")
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
    
        marker := oss.Marker("")
        prefix := oss.Prefix("<yourDirPrefix>")
        for {
            lor, err := bucket.ListObjects(marker, prefix, oss.Delimiter("/"))
            if err ! = nil {
                fmt.Println("Error:", err)
                os.Exit(-1)
            }
    
            for _, dirName := range lor.CommonPrefixes {
                fmt.Println(dirName)
            }
    
            prefix = oss.Prefix(lor.Prefix)
            marker = oss.Marker(lor.NextMarker)
            if ! lor.IsTruncated {
                break
            }
        }
    }
    ```

-   Use GetBucketV2 \(ListObjectsV2\) to list the information about all subfolders within a specified folder

    The following code provides an example on how to use GetBucketV2 \(ListObjectsV2\) to list the information about all subfolders within a specified folder:

    ```
    package main
    
    import (
        "fmt"
        "os"
        "github.com/aliyun/aliyun-oss-go-sdk/oss"
    )
    
    func HandleError(err error) {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    
    func main() {
        // Create an OSSClient instance.
        client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
        if err ! = nil {
            HandleError(err)
        }
    
        // Obtain the bucket.
        bucketName := "<yourBucketName>"
        bucket, err := client.Bucket(bucketName)
        if err ! = nil {
            HandleError(err)
        }
    
        continueToken := ""
        prefix := "<yourDirPrefix>"
        for {
            lsRes, err := bucket.ListObjectsV2(oss.Prefix(prefix), oss.ContinuationToken(continueToken), oss.Delimiter("/"))
            if err ! = nil {
                HandleError(err)
            }
    
            for _, dirName := range lsRes.CommonPrefixes {
                fmt.Println(dirName)
            }
    
            if lsRes.IsTruncated {
                continueToken = lsRes.NextContinuationToken
            } else {
                break
            }
        }
    }
    ```


## List objects and their owner information

The following code provides an example on how to use GetBucketV2 \(ListObjectsV2\) to list objects and their owner information:

```
package main

import (
    "fmt"
    "os"
    "github.com/aliyun/aliyun-oss-go-sdk/oss"
)

func HandleError(err error) {
    fmt.Println("Error:", err)
    os.Exit(-1)
}

func main() {
    // Create an OSSClient instance.
    client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
    if err ! = nil {
        HandleError(err)
    }

    // Obtain the bucket.
    bucketName := "<yourBucketName>"
    bucket, err := client.Bucket(bucketName)
    if err ! = nil {
        HandleError(err)
    }
    
    // Obtain the owner information.
    lsRes, err := bucket.ListObjectsV2(oss.FetchOwner(true))
    if err ! = nil {
        HandleError(err)
    }

    // Display the listed objects. By default, 100 objects are returned at a time.
    for _, object := range lsRes.Objects {
        fmt.Println(object.Key, object.Owner.ID, object.Owner.DisplayName)
    }
}
```

