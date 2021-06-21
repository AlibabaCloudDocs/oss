# List objects

You can list objects in a versioned bucket, including all objects, objects whose names contain a specified prefix, and objects and subfolders within a specified folder.

## Scenarios

The following shows the structure of the objects and folders in a bucket named examplebucket.

```
examplebucket
  └── fun
       └── exampleobject.jpg
       └── examplefile.txt
       └── destfolder
               └── image1.jpg
               └── image2.png
  └── srcfile.txt
  └── oss.jpg
```

The following sections describe how to list different objects by specifying conditions.

For more information about the parameters that you can configure when you list objects, see [GetBucketVersions\(ListObjectVersions\)](/intl.en-US/API Reference/Bucket operations/Versioning/GetBucketVersions(ListObjectVersions).md).

## List the versions of all objects in a bucket

The following code provides an example on how to list the versions of all objects including delete markers within a specified bucket:

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
    // Set yourEndpoint to the endpoint of the region where the bucket is located. For example, if your bucket is in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com.
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    client, err := oss.New("yourEndpoint","yourAccessKeyId","yourAccessKeySecret")
    if err ! = nil {
        HandleError(err)
    }

    // Specify the bucket name.
    bucketName := "examplebucket"
    bucket,err := client.Bucket(bucketName)
    if err ! = nil {
        HandleError(err)
    }

    // List the versions of all objects including delete markers within the bucket.
    keyMarker := oss.KeyMarker("")
    // The VersionIdMarker parameter is configured together with the KeyMarker parameter to specify the position from which the list operation starts.
    versionIdMarker := oss.VersionIdMarker("")
    for {
        lor, err := bucket.ListObjectVersions(keyMarker,versionIdMarker)
        if err ! = nil {
            HandleError(err)
        }

        // Display the information about the versions of returned objects.
        for _, dirName := range lor.ObjectVersions{
            fmt.Println("Versionid:",dirName.VersionId)
            fmt.Println("Key:",dirName.Key)
            fmt.Println("Is Latest",dirName.IsLatest)
        }
        // Display the version information of the listed delete markers.
         for _, marker  := range lor.ObjectDeleteMarkers {
            fmt.Println(marker.VersionId)
            fmt.Println(marker.Key)
        }
        // Check whether the versions of all objects and delete markers are listed. If the versions of only part of objects are listed, the list operation continues. If the versions of all objects are completely listed, the list operation ends.
        keyMarker = oss.KeyMarker(lor.NextKeyMarker)
        versionIdMarker = oss.VersionIdMarker(lor.NextVersionIdMarker)
        if ! lor.IsTruncated {
            break
        }
    }
}
```

For more information about endpoints, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md). For more information about the naming conventions for buckets, see [bucket](/intl.en-US/Developer Guide/Terms.md).

Returned results

```
Versionid: CAEQChiBgIDHzNPEthciIDYzZGQ5M2VjOTU2ODRmMzA4ZWM4ODk0NjVmMWEx****
Key: fun/
Is Latest true
Versionid: CAEQChiBgID61tnEthciIDNmOGM2MTQ3ZjU1NTQ2MGFhYWJjNjQxMTQxZWZh****
Key: fun/destfolder/
Is Latest true
Versionid: CAEQChiBgMDczt3EthciIDEwYjliZmQ4MDNiMDQ2Njk4YWMxM2NhM2E5NzQ3****
Key: fun/destfolder/image1.jpg
Is Latest true
Versionid: CAEQChiBgIDszt3EthciIGU5OWU0ZTllMGY3NTRmMmU5NzVjZmJkYmE3ZWYy****
Key: fun/destfolder/image2.png
Is Latest true
Versionid: CAEQChiBgICditzEthciIDBiNTg1ZTZkMDRlMzRjNDdiMjRjMTBlOGUzMTM0****
Key: fun/examplefile.txt
Is Latest true
Versionid: CAEQChiBgMC.itzEthciIDEzNWVlYjNhYzNmMjQ4NWM5Nzc2NzllY2FiYmQ3****
Key: fun/exampleobject.jpg
Is Latest true
Versionid: CAEQChiBgIDMgdbEthciIDUyMGI0NmZlNThkODQwY2ZhNmZhNTQ1Njk4ZTdj****
Key: oss.jpg
Is Latest true
Versionid: CAEQChiBgICIgdbEthciIDdlM2Q1YjYxZDIyZDQyMzI4MTRkNzVmYzdiMTBh****
Key: srcfile.txt
Is Latest true
```

## List the versions of objects whose names contain a specified prefix

The following code provides an example on how to list all versions of objects whose names contain a specified prefix:

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
    client, err := oss.New("yourEndpoint","yourAccessKeyId","yourAccessKeySecret")
    if err ! = nil {
        HandleError(err)
    }

    // Specify the bucket name.
    bucketName := "examplebucket"
    bucket,err := client.Bucket(bucketName)
    if err ! = nil {
        HandleError(err)
    }

    // Specify the Prefix parameter to list the versions of objects whose names contain the prefix fun.
    prefix := oss.Prefix("fun")
    keyMarker := oss.KeyMarker("")
    versionIdMarker := oss.VersionIdMarker("")
    for {
        lor, err := bucket.ListObjectVersions(prefix,keyMarker,versionIdMarker)
        if err ! = nil {
            HandleError(err)
        }

        // Display the information about the versions of returned objects.
        for _, dirName := range lor.ObjectVersions{
            fmt.Println("Versionid:",dirName.VersionId)
            fmt.Println("Key:",dirName.Key)
            fmt.Println("Is Latest",dirName.IsLatest)
        }
        // Check whether the versions of objects whose names contain the specified prefix are returned. If the versions of only part of objects whose names contain the specified prefix are listed, the list operation continues. If the versions of objects whose names contain the specified prefix are completely listed, the list operation ends.
        keyMarker = oss.KeyMarker(lor.NextKeyMarker)
        versionIdMarker = oss.VersionIdMarker(lor.NextVersionIdMarker)
        if ! lor.IsTruncated {
            break
        }
    }
}
```

Returned results

```
Versionid: CAEQChiBgIDHzNPEthciIDYzZGQ5M2VjOTU2ODRmMzA4ZWM4ODk0NjVmMWEx****
Key: fun/
Is Latest true
Versionid: CAEQChiBgID61tnEthciIDNmOGM2MTQ3ZjU1NTQ2MGFhYWJjNjQxMTQxZWZh****
Key: fun/destfolder/
Is Latest true
Versionid: CAEQChiBgMDczt3EthciIDEwYjliZmQ4MDNiMDQ2Njk4YWMxM2NhM2E5NzQ3****
Key: fun/destfolder/image1.jpg
Is Latest true
Versionid: CAEQChiBgIDszt3EthciIGU5OWU0ZTllMGY3NTRmMmU5NzVjZmJkYmE3ZWYy****
Key: fun/destfolder/image2.png
Is Latest true
Versionid: CAEQChiBgICditzEthciIDBiNTg1ZTZkMDRlMzRjNDdiMjRjMTBlOGUzMTM0****
Key: fun/examplefile.txt
Is Latest true
Versionid: CAEQChiBgMC.itzEthciIDEzNWVlYjNhYzNmMjQ4NWM5Nzc2NzllY2FiYmQ3****
Key: fun/exampleobject.jpg
Is Latest true
```

## List a specified number of object versions

The following code provides an example on how to list a specified number of object versions:

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
    client, err := oss.New("yourEndpoint","yourAccessKeyId","yourAccessKeySecret")
    if err ! = nil {
        HandleError(err)
    }

    // Specify the bucket name.
    bucketName := "examplebucket"
    bucket,err := client.Bucket(bucketName)
    if err ! = nil {
        HandleError(err)
    }

    // Specify the MaxKeys parameter to list up to four object versions in alphabetical order of the object names.
    maxkey := oss.MaxKeys(4)
    keyMarker := oss.KeyMarker("")
    versionIdMarker := oss.VersionIdMarker("")
    for {
        lor, err := bucket.ListObjectVersions(maxkey,keyMarker,versionIdMarker)
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }

        // Display the information about the versions of returned objects.
        for _, dirName := range lor.ObjectVersions{
            fmt.Println("Versionid:",dirName.VersionId)
            fmt.Println("Key:",dirName.Key)
            fmt.Println("Is Latest",dirName.IsLatest)
        }
        // Check whether the specified number of object versions are listed. If the specified number of object versions are not completely listed, the list operation continues. If the specified number of object versions are completely listed, the list operation ends.
        keyMarker = oss.KeyMarker(lor.NextKeyMarker)
        versionIdMarker = oss.VersionIdMarker(lor.NextVersionIdMarker)
        if ! lor.IsTruncated {
            break
        }
    }
}
```

Returned results

```
Versionid: CAEQChiBgIDHzNPEthciIDYzZGQ5M2VjOTU2ODRmMzA4ZWM4ODk0NjVmMWEx****
Key: fun/
Is Latest true
Versionid: CAEQChiBgID61tnEthciIDNmOGM2MTQ3ZjU1NTQ2MGFhYWJjNjQxMTQxZWZh****
Key: fun/destfolder/
Is Latest true
Versionid: CAEQChiBgMDczt3EthciIDEwYjliZmQ4MDNiMDQ2Njk4YWMxM2NhM2E5NzQ3****
Key: fun/destfolder/image1.jpg
Is Latest true
Versionid: CAEQChiBgIDszt3EthciIGU5OWU0ZTllMGY3NTRmMmU5NzVjZmJkYmE3ZWYy****
Key: fun/destfolder/image2.png
Is Latest true
```

## List the versions of all objects by page

The following code provides an example on how to list the versions of all objects by page:

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
    client, err := oss.New("yourEndpoint","yourAccessKeyId","yourAccessKeySecret")
    if err ! = nil {
        HandleError(err)
    }

    // Specify the bucket name.
    bucketName := "examplebucket"
    bucket,err := client.Bucket(bucketName)
    if err ! = nil {
        HandleError(err)
    }

    // List the versions of all objects in the bucket by page.
    keyMarker := oss.KeyMarker("")
    // Specify the MaxKeys parameter to list up to four object versions in alphabetical order of the object names.
    maxkey := oss.MaxKeys(4)
    versionIdMarker := oss.VersionIdMarker("")
    for {
        lor, err := bucket.ListObjectVersions(maxkey,keyMarker,versionIdMarker)
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }

        // Display the information about the versions of returned objects.
        for _, dirName := range lor.ObjectVersions{
            fmt.Println("Versionid:",dirName.VersionId)
            fmt.Println("Key:",dirName.Key)
            fmt.Println("Is Latest",dirName.IsLatest)
        }

        fmt.Println("---------------")

        // Check whether the specified number of object versions are listed. If the versions of only part of objects are listed, the list operation continues. If the versions of all objects are completely listed, the list operation ends.
        if lor.IsTruncated {
            keyMarker = oss.KeyMarker(lor.NextKeyMarker)
            versionIdMarker = oss.VersionIdMarker(lor.NextVersionIdMarker)
        }else{
            break
        }
    }
}
```

Returned results

```
Versionid: CAEQChiBgIDHzNPEthciIDYzZGQ5M2VjOTU2ODRmMzA4ZWM4ODk0NjVmMWEx****
Key: fun/
Is Latest true
Versionid: CAEQChiBgID61tnEthciIDNmOGM2MTQ3ZjU1NTQ2MGFhYWJjNjQxMTQxZWZh****
Key: fun/destfolder/
Is Latest true
Versionid: CAEQChiBgMDczt3EthciIDEwYjliZmQ4MDNiMDQ2Njk4YWMxM2NhM2E5NzQ3****
Key: fun/destfolder/image1.jpg
Is Latest true
Versionid: CAEQChiBgIDszt3EthciIGU5OWU0ZTllMGY3NTRmMmU5NzVjZmJkYmE3ZWYy****
Key: fun/destfolder/image2.png
Is Latest true
---------------
Versionid: CAEQChiBgICditzEthciIDBiNTg1ZTZkMDRlMzRjNDdiMjRjMTBlOGUzMTM0****
Key: fun/examplefile.txt
Is Latest true
Versionid: CAEQChiBgMC.itzEthciIDEzNWVlYjNhYzNmMjQ4NWM5Nzc2NzllY2FiYmQ3****
Key: fun/exampleobject.jpg
Is Latest true
Versionid: CAEQChiBgIDMgdbEthciIDUyMGI0NmZlNThkODQwY2ZhNmZhNTQ1Njk4ZTdj****
Key: oss.jpg
Is Latest true
Versionid: CAEQChiBgICIgdbEthciIDdlM2Q1YjYxZDIyZDQyMzI4MTRkNzVmYzdiMTBh****
Key: srcfile.txt
Is Latest true
---------------
```

## List objects by folder

OSS does not use a hierarchical structure for objects, but instead uses a flat structure. All elements are stored as objects in buckets. A folder is an object 0 KB in size whose name ends with a forward slash \(/\). You can upload and download this object. By default, objects whose names end with a forward slash \(/\) are displayed as folders in the OSS console.

You can use the delimiter and prefix parameters to list objects by folder.

-   If you set prefix to a folder name in the request, objects and subfolders whose names contain the prefix are listed.
-   If you also set delimiter to a forward slash \(/\) in the request, the objects and subfolders whose names start with the specified prefix in the folder are listed. Each subfolder is listed as a single result element in commonPrefixes. The objects and folders in these subfolders are not listed.

The following examples describe how to list objects by simulating folders.

-   List the versions of objects within the root folder of a bucket

    The following code provides an example on how to list the versions of objects within the root folder of a bucket:

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
        client, err := oss.New("yourEndpoint","yourAccessKeyId","yourAccessKeySecret")
        if err ! = nil {
            HandleError(err)
        }
    
        // Specify the bucket name.
        bucketName := "examplebucket"
        bucket,err := client.Bucket(bucketName)
        if err ! = nil {
            HandleError(err)
        }
    
        // Set the delimiter parameter to a forward slash (/) to list the versions of objects and the names of subfolders within the root folder.
        delimiter := oss.Delimiter("/")
        keyMarker := oss.KeyMarker("")
        versionIdMarker := oss.VersionIdMarker("")
        for {
            lor, err := bucket.ListObjectVersions(keyMarker,delimiter,versionIdMarker)
            if err ! = nil {
                HandleError(err)
            }
    
            // Display the information about the versions of returned objects.
            for _, dirName := range lor.ObjectVersions{
                fmt.Println("Versionid:",dirName.VersionId)
                fmt.Println("Key:",dirName.Key)
                fmt.Println("Is Latest",dirName.IsLatest)
            }
    
            // Display the folders whose names end with a forward slash (/).
            for _,common_prefix := range lor.CommonPrefixes{
                fmt.Println("common_prefix:",common_prefix)
            }
            // Check whether the versions of objects within the root folder of the bucket are completely listed. If the versions of only part of objects within the root folder of the bucket are listed, the list operation continues. If the versions of objects within the root folder of the bucket are completely listed, the list operation ends.
            if lor.IsTruncated {
                keyMarker = oss.KeyMarker(lor.NextKeyMarker)
                versionIdMarker = oss.VersionIdMarker(lor.NextVersionIdMarker)
            }else{
                break
            }
        }
    }
    ```

    Returned results

    ```
    Versionid: CAEQChiBgIDMgdbEthciIDUyMGI0NmZlNThkODQwY2ZhNmZhNTQ1Njk4ZTdj****
    Key: oss.jpg
    Is Latest true
    Versionid: CAEQChiBgICIgdbEthciIDdlM2Q1YjYxZDIyZDQyMzI4MTRkNzVmYzdiMTBh****
    Key: srcfile.txt
    Is Latest true
    common_prefix: fun/
    ```

-   List objects and subfolders within a specified folder

    The following code provides an example on how to list the objects and subfolders within a specified folder:

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
        client, err := oss.New("yourEndpoint","yourAccessKeyId","yourAccessKeySecret")
        if err ! = nil {
            HandleError(err)
        }
    
        // Specify the bucket name.
        bucketName := "examplebucket"
        bucket,err := client.Bucket(bucketName)
        if err ! = nil {
            HandleError(err)
        }
    
        // Configure the Prefix parameter to list all objects and subfolders within the fun folder. Set the delimiter parameter to a forward slash (/).
        prefix := oss.Prefix("fun/")
        delimiter := oss.Delimiter("/")
        keyMarker := oss.KeyMarker("")
        versionIdMarker := oss.VersionIdMarker("")
        for {
            lor, err := bucket.ListObjectVersions(prefix,delimiter,keyMarker,versionIdMarker)
            if err ! = nil {
                HandleError(err)
            }
    
            // Display the information about the versions of returned objects.
            for _, dirName := range lor.ObjectVersions{
                fmt.Println("Versionid:",dirName.VersionId)
                fmt.Println("Key:",dirName.Key)
                fmt.Println("Is Latest:",dirName.IsLatest)
            }
    
            // Display the folders whose names end with a forward slash (/).
            for _,common_prefix := range lor.CommonPrefixes{
                fmt.Println("common_prefix:",common_prefix)
            }
            // Check whether all objects and subfolders within the specified folder are listed. If the objects and subfolders within the specified folder are not completely listed, the list operation continues. If the objects and subfolders within the specified folder are completely listed, the list operation ends.
            if lor.IsTruncated {
                keyMarker = oss.KeyMarker(lor.NextKeyMarker)
                versionIdMarker = oss.VersionIdMarker(lor.NextVersionIdMarker)
            }else{
                break
            }
        }
    }
    ```

    Returned results

    ```
    Versionid: CAEQChiBgIDHzNPEthciIDYzZGQ5M2VjOTU2ODRmMzA4ZWM4ODk0NjVmMWEx****
    Key: fun/
    Is Latest: true
    Versionid: CAEQChiBgICditzEthciIDBiNTg1ZTZkMDRlMzRjNDdiMjRjMTBlOGUzMTM0****
    Key: fun/examplefile.txt
    Is Latest: true
    Versionid: CAEQChiBgMC.itzEthciIDEzNWVlYjNhYzNmMjQ4NWM5Nzc2NzllY2FiYmQ3****
    Key: fun/exampleobject.jpg
    Is Latest: true
    common_prefix: fun/destfolder/
    ```


