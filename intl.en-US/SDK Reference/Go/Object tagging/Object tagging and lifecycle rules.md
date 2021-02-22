# Object tagging and lifecycle rules

Lifecycle rules can take effect on objects with specified prefixes or tags. You can also specify prefixes and tags as the condition of a lifecycle rule at the same time.

**Note:** If you set a specified tag as the condition of a lifecycle rule, the rule applies to an object only when the key and value in the tag of the object both match the specified tag. If you specify a prefix and multiple tags as the condition of a lifecycle rule, the rule applies to an object only when the prefix and tags of the object match the prefix and all tags specified in the rule.

## Set tags as the matching condition of a lifecycle rule

Run the following code to set tags as the matching condition of a lifecycle rule:

```
package main

import (
  "fmt"
  "os"

  "github.com/aliyun/aliyun-oss-go-sdk/oss"
)

func main() {
  // Creates an OSSClient instance.
  client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // Sets the lifecycle rule ID to "rule2" and applies it to objects prefixed with "one".
  // The storage class of Objects prefixed with "one" is converted to IA after 3 days and is converted to Archive after 30 days.
    Days: 3,
    StorageClass: oss.StorageIA,
  }
  transitionArch := oss.LifecycleTransition{
    Days: 30,
    StorageClass: oss.StorageArchive,
  }

  // Configures the tags used as the matching condition. 
  tag1 := oss.Tag{
    Key:   "key1",
    Value: "value1",
  }
  tag2 := oss.Tag{
    Key:   "key2",
    Value: "value2",
  }

  // Sets the bucket lifecycle rule, which includes the tags set as the matching condition.
  rule1 := oss.LifecycleRule{
    ID: "rule1",
    Prefix: "one",
    Status: "Enabled",
    Transitions: []oss.LifecycleTransition{transitionIA, transitionArch},
    Tags:[]oss.Tag{tag1,tag2},
  }
  rules := []oss.LifecycleRule{rule1}
  err = client.SetBucketLifecycle("<yourBucketName>", rules)
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }
}
```

## View the tags set as the matching condition of a lifecycle rule

Run the following code to view the tags set as the matching condition of a lifecycle rule:

```
package main

import (
  "fmt"
  "os"

  "github.com/aliyun/aliyun-oss-go-sdk/oss"
)

func main() {
  // Creates an OSSClient instance.
  client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // Obtains lifecycle rule for the bucket that stores the object.
  lc, err := client.GetBucketLifecycle("<yourBucketName>")
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // Prints the lifecycle rule that includes the tags set as the matching condition.
  for i, v := range lc.Rules {
    fmt.Printf("Bucket %d Lifecycle: %v", i, v)
    if  v.Expiration != nil {
      fmt.Printf(", Expiration:%v", *v.Expiration)
    }
    fmt.Printf("\n")
  }
}
```

