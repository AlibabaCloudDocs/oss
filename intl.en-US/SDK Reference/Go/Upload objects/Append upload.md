# Append upload

You can use AppendObject to append content to appendable objects that are uploaded.

## Usage notes

When you call AppendObject to upload an object, take note of the following items:

-   If the object to append does not exist, an append object is created.
-   If the object to append is an existing append object and the specified position from which the append operation starts is not equal to the current object size, the PositionNotEqualToLength error is returned. If the object to append is a non-append object, the ObjectNotAppendable error is returned.
-   The CopyObject operation cannot be performed on append objects.

For more information about how to perform append upload, see [AppendObject](/intl.en-US/API Reference/Object operations/Basic operations/AppendObject.md).

## Sample codes

The following code provides an example on how to perform append upload:

```
package main

import (
    "fmt"
    "os"
    "strings"
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

    var nextPos int64 = 0
    // If the object is appended for the first time, the append position is 0. The position for the next append operation is included in the response. The position from which the next append operation starts is the current length of the object.
    // <yourObjectName> indicates the complete path of the object you want to append, and must include the extension of the object. Example: example/test.txt
    nextPos, err = bucket.AppendObject("<yourObjectName>", strings.NewReader("YourObjectAppendValue1"), nextPos)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    
    // If you have appended content to the object, you can obtain the position from which the current append operation starts from the next_position field in the response returned by the last operation or by using the bucket.head_object method.
    //props, err := bucket.GetObjectDetailedMeta("<yourObjectName>")
    //if err ! = nil {
    //    fmt.Println("Error:", err)
    //    os.Exit(-1)
    //}
    //nextPos, err = strconv.ParseInt(props.Get("X-Oss-Next-Append-Position"), 10, 64)
    //if err ! = nil {
    //    fmt.Println("Error:", err)
    //    os.Exit(-1)
    //}    

    // Perform the second append operation.
    nextPos, err = bucket.AppendObject("<yourObjectName>", strings.NewReader("YourObjectAppendValue2"), nextPos)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // You can perform append operations on an object for multiple times.
}
```

For more information about the naming conventions for buckets, see [bucket](/intl.en-US/Developer Guide/Terms.md). For more information about the naming conventions for objects, see [object](/intl.en-US/Developer Guide/Terms.md).

