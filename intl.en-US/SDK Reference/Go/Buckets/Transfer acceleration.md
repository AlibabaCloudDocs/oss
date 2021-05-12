# Transfer acceleration

The transfer acceleration feature allows users worldwide to access objects stored in Object Storage Service \(OSS\) more quickly. This feature is applicable to scenarios where data needs to be transferred over long geographical distances. This feature can also be used to download or upload large objects that are gigabytes or terabytes in size.

For more information about transfer acceleration, see [Transfer acceleration](https://icms.alibaba-inc.com/content/oss/f3b55d?l=1&m=151&n=1436619) in OSS Developer Guide.

## Enable transfer acceleration

The following code provides an example on how to enable transfer acceleration for a bucket named examplebucket:

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
  // Set Endpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set Endpoint to https://oss-cn-hangzhou.aliyuncs.com. 
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
  client, err := oss.New("yourEndpoint", "yourAccessKeyId", "yourAccessKeySecret")
  if err != nil {
    HandleError(err)
  }

  // Specify the bucket name. 
  bucketName := "examplebucket"

  // Enable transfer acceleration for the bucket. 
  // The Enabled parameter specifies whether to enable transfer acceleration. A value of true specifies that transfer acceleration is enabled. A value of false specifies that transfer acceleration is disabled. 
  accConfig := oss.TransferAccConfiguration{}
  accConfig.Enabled = true

  err = client.SetBucketTransferAcc(bucketName, accConfig)
  if err != nil {
    HandleError(err)
  }
  fmt.Printf("set bucket transfer accelerate success\n")  
}
```

For more information about the operation used to enable transfer acceleration, see [PutBucketTransferAcceleration](https://icms.alibaba-inc.com/content/oss/f6a517?l=1&m=154&n=2926996).

## Query the status of transfer acceleration

The following code provides an example on how to query the transfer acceleration status of a bucket named examplebucket:

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
  client, err := oss.New("yourEndpoint", "yourAccessKeyId", "yourAccessKeySecret")
  if err != nil {
    HandleError(err)
  }

  // Specify the bucket name. 
  bucketName := "examplebucket"

  // Query the transfer acceleration status of the bucket. 
  accConfig, err := client.GetBucketTransferAcc(bucketName)
  if err != nil {
    HandleError(err)
  }

  fmt.Printf("accConfigRes.Enabled:%T\n", accConfig.Enabled)
}
```

For more information about the operation used to query the transfer acceleration status of a bucket, see [GetBucketTransferAcceleration](https://icms.alibaba-inc.com/content/oss/f6a517?l=1&m=154&n=2926998).

