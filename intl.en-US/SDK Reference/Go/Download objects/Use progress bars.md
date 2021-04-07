# Use progress bars

OSS allows you to use progress bars to indicate the progress of an upload or download task. The Bucket.GetObjectToFile method is used in the example to describe how to display the progress bar of the task initiated to download an object.

The following code provides an example on how to display the progress bar of the task initiated to download the exampleobject.txt object from examplebucket:

```
package main

import (
    "fmt"
    "os"
    "github.com/aliyun/aliyun-oss-go-sdk/oss"
)

// Define the progress bar listener.
type OssProgressListener struct {
}

// Define the function used to process progress change events.
func (listener *OssProgressListener) ProgressChanged(event *oss.ProgressEvent) {
    switch event.EventType {
    case oss.TransferStartedEvent:
        fmt.Printf("Transfer Started, ConsumedBytes: %d, TotalBytes %d.\n",
            event.ConsumedBytes, event.TotalBytes)
    case oss.TransferDataEvent:
        if event.TotalBytes != 0 {
            fmt.Printf("\rTransfer Data, ConsumedBytes: %d, TotalBytes %d, %d%%.",
                event.ConsumedBytes, event.TotalBytes, event.ConsumedBytes*100/event.TotalBytes)
        }
    case oss.TransferCompletedEvent:
        fmt.Printf("\nTransfer Completed, ConsumedBytes: %d, TotalBytes %d.\n",
            event.ConsumedBytes, event.TotalBytes)
    case oss.TransferFailedEvent:
        fmt.Printf("\nTransfer Failed, ConsumedBytes: %d, TotalBytes %d.\n",
            event.ConsumedBytes, event.TotalBytes)
    default:
    }
}

func main() {
    // Create an OSSClient instance.
    // Set yourEndpoint to the endpoint of the region where the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com.
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    client, err := oss.New("yourEndpoint","yourAccessKeyId","yourAccessKeySecret")
    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    // Specify the bucket name.
    bucketName := "examplebucket"
    // Specify the full path of the object. The full path of the object cannot contain bucket names.
    objectName := "exampleobject.txt"
    // Specify the full path of the local file. If the specified local file exists, the downloaded object replaces the local file. Otherwise, the local file is created.
    // If the path of the local file is not specified, the downloaded object is saved to the path of the project to which the sample program belongs.
    localFile := "D:\\localpath\\examplefile.txt"

    // Obtain the bucket.
    bucket, err := client.Bucket(bucketName)
    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // Display the download progress while you download the object.
    err = bucket.GetObjectToFile(objectName, localFile, oss.Progress(&OssProgressListener{}))
    if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
    }
    fmt.Println("Transfer Completed.")
}
        
```

