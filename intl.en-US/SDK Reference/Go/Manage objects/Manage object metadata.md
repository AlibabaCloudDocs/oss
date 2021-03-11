# Manage object metadata

Object metadata includes HTTP headers and user metadata. This topic describes how to configure, modify, and query the metadata of an object.

For more information about object metadata, see [Manage object metadata](/intl.en-US/Developer Guide/Objects/Manage files/Manage object metadata.md). For the complete code of append upload, visit [GitHub](https://github.com/aliyun/aliyun-oss-go-sdk/blob/master/sample/object_meta.go).

## Configure object metadata

You can configure the metadata of an object when you upload the object. The following table describes the metadata that you can configure.

|Parameter|Description|
|:--------|:----------|
|CacheControl|Specifies the caching behavior of a web page when the object is downloaded.|
|ContentDisposition|Specifies the name of the object when it is downloaded.|
|ContentEncoding|Specifies the encoding format that is used when the object is downloaded.|
|Expires|Specifies the expiration time of the cache in the GMT format.|
|ServerSideEncryption|Specifies the server-side encryption algorithm to use when OSS creates the object. Valid value: AES256.|
|ObjectACL|Specifies the ACL of the created object.|
|Meta|Specifies the user metadata of the object. The parameters of user metadata are prefixed with `X-Oss-Meta-`.|

The following code provides an example on how to configure object metadata:

```
package main

import (
    "fmt"
    "os"
    "time"
    "strings"
    "github.com/aliyun/aliyun-oss-go-sdk/oss"
)

func main() {
    // Create an OSSClient instance.
    client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // Obtain the name of the bucket.
    bucket, err := client.Bucket("<yourBucketName>")
    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // Configure object metadata: Set the expiration time to 23:00:00 Janualry 10, 2049 in GMT, ACL to public read, and MyProp to MyPropVal as user metadata.
    expires := time.Date(2049, time.January, 10, 23, 0, 0, 0, time.UTC)
    options := []oss.Option{
        oss.Expires(expires),
        oss.ObjectACL(oss.ACLPublicRead),
        oss.Meta("MyProp", "MyPropVal"),
    }

    // Use a data stream to upload the object.
    err = bucket.PutObject("<yourObjectName>", strings.NewReader("MyObjectValue"), options...)
    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // Obtain the object metadata.
    props, err := bucket.GetObjectDetailedMeta("<yourObjectName>")
    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    fmt.Println("Object Meta:", props)
}
            
```

## Modify object metadata

The following code provides an example on how to modify one or multiple object metadata parameters at a time:

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
    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    bucketName := "<yourBucketName>"
    objectName := "<yourObjectName>"

    // Obtain the name of the bucket.
    bucket, err := client.Bucket(bucketName)
    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // Modify one object metadata parameter at a time.
    err = bucket.SetObjectMeta(objectName, oss.Meta("MyMeta", "MyMetaValue1"))
    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // Modify multiple object metadata parameters at a time.
    options := []oss.Option{
        oss.Meta("MyMeta", "MyMetaValue2"),
        oss.Meta("MyObjectLocation", "HangZhou"),
    }
    err = bucket.SetObjectMeta(objectName, options...)
    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // Obtain the object metadata.
    props, err := bucket.GetObjectDetailedMeta(objectName)
    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    fmt.Println("Object Meta:", props)
}
            
```

## Query object metadata

The following code provides an example on how to obtain Object Meta.

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
    objectName := "<yourObjectName>"

    // Obtain the name of the bucket.
    bucket, err := client.Bucket(bucketName)
    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // Obtain the object metadata.
    props, err := bucket.GetObjectDetailedMeta(objectName)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    fmt.Println("Object Meta:", props)
}
            
```

