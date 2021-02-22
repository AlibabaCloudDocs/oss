# Manage inventory rules

This topic describes how to add, view, list, and delete the inventory configurations of a bucket.

**Note:** Before you can perform these operations, you must have the appropriate permissions on the bucket. By default, these permissions are available only to the bucket owner. If you do not have the permissions to perform these operations, apply for the permissions from the bucket owner.

## Add inventory rules

**Note:**

-   You can have up to 1,000 inventory configurations per bucket.
-   The inventory list must be stored in a bucket that is in the same region as the bucket that you want to inventory.

The following code provides an example on how to add an inventory configuration for a bucket:

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

    bEnabled := true // Check whether the inventory feature is enabled.

    // To encrypt inventory lists, refer to the following code. 
    //var invEncryption oss.InvEncryption
    //var invSseOss oss.InvSseOss
    //var invSseKms oss.InvSseKms
    //invSseKms.KmsId = "<yourKmsId>"
    //invEncryption.SseOss = &invSseOss // Use SSE-OSS to encrypt the inventory list.
    //invEncryption.SseKms = &invSseKms // Use SSE-KMS to encrypt the inventory list.

    invConfig := oss.InventoryConfiguration{
        // Specify the name of the inventory rule. The name must be globally unique in the current bucket.
        Id: "<yourInventoryId2>",

        // Check whether the inventory feature is enabled.
        IsEnabled: &bEnabled,

        // Specify the prefix used to filter the inventory rule. 
        Prefix: "<yourFilterPrefix>",
        OSSBucketDestination: oss.OSSBucketDestination{
            // Specify the format of the exported inventory list.
            Format: "CSV",

             // Specify the ID of the account granted by the bucket owner. Example: 109885487000****.            
             AccountId: "<yourGrantAccountId>",
             // Specify the role that the bucket owner grants permissions. Example: acs:ram::109885487000****:role/ram-test.
            RoleArn: "<yourRoleArn>",

            // Specify the bucket used to store the exported inventory list.
            Bucket: "acs:oss:::" + "<yourDestBucketName>",

            // Specify the path of the exported inventory list.            
            Prefix: "<yourDestPrefix>",

            // To encrypt the inventory list, refer to the following code.
           //Encryption:     &invEncryption,
        },

        // Specify the frequency that inventory lists are exported. 
        Frequency: "Daily",

        // Specify whether versioning information about the objects is included in the inventory list. 
        IncludedObjectVersions: "All",

        OptionalFields: oss.OptionalFields{
            // Specify the configuration fields included in the inventory list.            
            Field: []string{
                "Size", "LastModifiedDate", "ETag", "StorageClass", "IsMultipartUploaded", "EncryptionStatus",
            },
        },
    }

    err = client.SetBucketInventory("<yourBucketName>", invConfig)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
}
```

For more information about how to add an inventory configuration for a bucket, see [PutBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/PutBucketInventory.md).

## View an inventory rule

The following code provides an example on how to view the inventory configurations of a bucket:

```
package main

import (
    "encoding/xml"
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

    result, err := client.GetBucketInventory("<yourBucketName>", "<yourInventoryId>")
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // Display the inventory rule.
    bs, err := xml.MarshalIndent(result, "  ", "    ")
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    fmt.Println(string(bs))
}
```

For more information about how to view inventory configurations, see [GetBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/GetBucketInventory.md).

## List inventory rules

**Note:** You can obtain up to 100 inventory configurations per request. To obtain more than 100 inventory configurations, you must send multiple requests. Each subsequent request must use the token returned in the previous request.

The following code provides an example on how to list all inventory configurations of a bucket at a time:

```
package main

import (
    "encoding/xml"
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

    var sumResult oss.ListInventoryConfigurationsResult
    vmarker := ""
    for {
        listResult, err := client.ListBucketInventory("<yourBucketName>", vmarker)
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
        sumResult.InventoryConfiguration = append(sumResult.InventoryConfiguration, listResult.InventoryConfiguration...)
        if listResult.IsTruncated ! = nil && *listResult.IsTruncated {
            vmarker = listResult.NextContinuationToken
        } else {
            break
        }
    }

    // Display the inventory rules configured for the bucket.
    bs, err := xml.MarshalIndent(sumResult, "  ", "    ")
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    fmt.Println(string(bs))
}
```

For more information about how to list multiple inventory configurations at a time, see [ListBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/ListBucketInventory.md).

## Delete an inventory rule

The following code provides an example on how to delete the inventory configurations of a bucket:

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
    // Delete the inventory rule.
    err = client.DeleteBucketInventory("<yourBucketName>", "<yourInventoryId>")
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
}
```

For more information about how to delete the inventory configurations of a bucket, see [DeleteBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/DeleteBucketInventory.md).

