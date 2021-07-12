# Set logging

You can enable access logs to record bucket access to log files, which are stored in a specified bucket.

The log file format is as follows:

`<TargetPrefix><SourceBucket>YYYY-mm-DD-HH-MM-SS-UniqueString`

For more information about access log files, see [Set access logging](/intl.en-US/Developer Guide/Manage logs/Log storage.md). For the complete code of set logging, see [GitHub](https://github.com/aliyun/aliyun-oss-go-sdk/blob/master/sample/bucket_logging.go).

## Enable access logging

Run the following code to enable bucket access logging:

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
    targetBucketName := "<yourTargetBucketName>"
    targetPrefix := "<yourTargetPrefix>"

    // Enable access logging for a bucket. targetBucketName is the bucket that logs are stored, targetPrefix is the prefix of the stored log files, that is, the path of the log files.
    err = client.SetBucketLogging(bucketName, targetBucketName, targetPrefix, true)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
}
            
```

## View access logging configurations

Run the following code to view the access logging configurations for a bucket:

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

    // View the access logging configurations for a bucket.
    logRes, err := client.GetBucketLogging(bucketName)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    fmt.Println("Target Bucket: ", logRes.LoggingEnabled.TargetBucket)
    fmt.Println("Target Prefix: ", logRes.LoggingEnabled.TargetPrefix)
}
            
```

## Disable access logging

Run the following code to disable access logging for a bucket:

```
package main

import (
    "fmt"
    "os"
    "github.com/aliyun/aliyun-oss-go-sdk/oss"
)

func main() {
    // Create an OSSClient instance.
    // client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    bucketName := "<yourBucketName>"

    // Disable access logging.
    err = client.DeleteBucketLogging(bucketName)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
}
            
```

