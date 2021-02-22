# Bucket inventory

This topic describes how to add, view, list, and delete the inventory configurations of a bucket.

**Note:** Before you can perform these operations, you must have the appropriate permissions on the bucket. By default, these permissions are available only to the bucket owner. If you do not have the permissions to perform these operations, apply for the permissions from the bucket owner.

## Add inventory configurations

**Note:**

-   You can have up to 1,000 inventory configurations per bucket.
-   The inventory list must be stored in a bucket that is in the same region as the bucket that you want to inventory.

The following code provides an example on how to add an inventory configuration for a bucket:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);

    InventoryConfiguration inventoryConf;
    /* Specify the inventory ID, which must be globally unique in the current bucket. */
    inventoryConf.setId("inventoryId");

    /* Specify whether the inventory is enabled. True indicates that the inventory list is generated, while false indicates that no inventory list is generated. */
    inventoryConf.setIsEnabled(true);

    /* Optional. Specify the prefix of object names to filter objects to include in the inventory list. After the prefix is specified, objects whose names contain the prefix are filtered to include in the inventory list. */
    inventoryConf.setFilter(InventoryFilter("objectPrefix"));

    InventoryOSSBucketDestination dest;
    /* Specify the output format of the inventory results. */
    dest.setFormat(InventoryFormat::CSV);
    /* Specify the account ID that is authorized by the bucket owner. */
    dest.setAccountId("10988548********");
    /* Specify the role ARN to which the bucket owner grants the operation permissions. */
    dest.setRoleArn("acs:ram::10988548********:role/inventory-test");
    /* Specify the bucket to contain the inventory results. */
    dest.setBucket("yourDstBucketName");
    /* Specify the prefix of the inventory list object. */
    dest.setPrefix("yourPrefix");
    /* Optional. Specify the method for encrypting the inventory list object. You can use SSEOSS or SSEKMS. */
    //dest.setEncryption(InventoryEncryption(InventorySSEOSS()));
    //dest.setEncryption(InventoryEncryption(InventorySSEKMS("yourKmskeyId")));
    inventoryConf.setDestination(dest);

    /* Specify the frequency to output the inventory results. You can set Daily or Weekly. */
    inventoryConf.setSchedule(InventoryFrequency::Daily);

    /* Set the object versions to include in the inventory list. Optional values: All and Current. */
    inventoryConf.setIncludedObjectVersions(InventoryIncludedObjectVersions::All);

    /* Optional. Specify the configurations to include in the inventory results based on your requirements. */
    InventoryOptionalFields field { 
        InventoryOptionalField::Size, InventoryOptionalField::LastModifiedDate, 
        InventoryOptionalField::ETag, InventoryOptionalField::StorageClass, 
        InventoryOptionalField::IsMultipartUploaded, InventoryOptionalField::EncryptionStatus
    };
    inventoryConf.setOptionalFields(field);

    /* Set the inventory configuration. */
    auto outcome = client.SetBucketInventoryConfiguration(
        SetBucketInventoryConfigurationRequest(BucketName, inventoryConf));

    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "Set Bucket Inventory fail" <<
        ",code:" << outcome.error().Code() <<
        ",message:" << outcome.error().Message() <<
        ",requestId:" << outcome.error().RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

For more information about how to add an inventory configuration for a bucket, see [PutBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/PutBucketInventory.md).

## View inventory configurations

The following code provides an example on how to view the inventory configurations of a bucket:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";

    /* Specify the inventory ID. */
    std::string InventoryId = "yourInventoryId";

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);

    /* Obtain the inventory configuration. */
    auto outcome = client.GetBucketInventoryConfiguration(
        GetBucketInventoryConfigurationRequest(BucketName, InventoryId));

    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "Get Bucket Inventory fail" <<
        ",code:" << outcome.error().Code() <<
        ",message:" << outcome.error().Message() <<
        ",requestId:" << outcome.error().RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }

    /* Obtain the information about the inventory configuration. */
    const auto& inventoryConf = outcome.result().InventoryConfiguration();
    std::cout << inventoryConf.Id() << std::endl;
    std::cout << inventoryConf.IsEnabled() << std::endl;
    std::cout << inventoryConf.Filter().Prefix() << std::endl;
    std::cout << inventoryConf.Destination().OSSBucketDestination().AccountId() << std::endl;
    std::cout << inventoryConf.Destination().OSSBucketDestination().RoleArn() << std::endl;
    std::cout << inventoryConf.Destination().OSSBucketDestination().Bucket() << std::endl;
    std::cout << inventoryConf.Destination().OSSBucketDestination().Prefix() << std::endl;
    if (inventoryConf.Destination().OSSBucketDestination().Encryption().hasSSEKMS()) {
        std::cout << inventoryConf.Destination().OSSBucketDestination().Encryption().SSEKMS().KeyId() << std::endl;
    }
    else if (inventoryConf.Destination().OSSBucketDestination().Encryption().hasSSEOSS()) {
        std::cout << "has sse-oss" << std::endl;
    }

    std::cout << static_cast<int>(inventoryConf.Schedule()) << std::endl;
    std::cout << static_cast<int>(inventoryConf.IncludedObjectVersions()) << std::endl;

    for (const auto& fiedl: inventoryConf.OptionalFields()) {
        std::cout << static_cast<int>(fiedl) << std::endl;
    }

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

For more information about how to view inventory configurations, see [GetBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/GetBucketInventory.md).

## List multiple inventory configurations at a time

**Note:** You can obtain up to 100 inventory configurations per request. To obtain more than 100 inventory configurations, you must send multiple requests. Each subsequent request must use the token returned in the previous request.

The following code provides an example on how to list all inventory configurations of a bucket at a time:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);

    /* Configure the request parameters. */
    std::string nextToken = "";
    bool isTruncated = false;
    do {
        /* List the inventory configurations. Up to 100 inventory configurations can be listed per request. */
        auto request = ListBucketInventoryConfigurationsRequest(BucketName);
        request.setContinuationToken(nextToken);
        auto outcome = client.ListBucketInventoryConfigurations(request);

        if (! outcome.isSuccess()) {    
            /*Handle exceptions.*/
            std::cout << "ListObjects fail" <<
                ",code:" << outcome.error().Code() <<
                ",message:" << outcome.error().Message() <<
                ",requestId:" << outcome.error().RequestId() << std::endl;
            break;
        }

        for (const auto& inventoryConf : outcome.result().InventoryConfigurationList()) {
            std::cout << inventoryConf.Id() << std::endl;
            std::cout << inventoryConf.IsEnabled() << std::endl;
            std::cout << inventoryConf.Filter().Prefix() << std::endl;
            std::cout << inventoryConf.Destination().OSSBucketDestination().AccountId() << std::endl;
            std::cout << inventoryConf.Destination().OSSBucketDestination().RoleArn() << std::endl;
            std::cout << inventoryConf.Destination().OSSBucketDestination().Bucket() << std::endl;
            std::cout << inventoryConf.Destination().OSSBucketDestination().Prefix() << std::endl;
            if (inventoryConf.Destination().OSSBucketDestination().Encryption().hasSSEKMS()) {
                std::cout << inventoryConf.Destination().OSSBucketDestination().Encryption().SSEKMS().KeyId() << std::endl;
            }
            else if (inventoryConf.Destination().OSSBucketDestination().Encryption().hasSSEOSS()) {
                std::cout << "has sse-oss" << std::endl;
            }

            std::cout << static_cast<int>(inventoryConf.Schedule()) << std::endl;
            std::cout << static_cast<int>(inventoryConf.IncludedObjectVersions()) << std::endl;

            for (const auto& fiedl: inventoryConf.OptionalFields()) {
                std::cout << static_cast<int>(fiedl) << std::endl;
            }
        }

        nextToken = outcome.result().NextContinuationToken();
        isTruncated = outcome.result().IsTruncated();
    } while (isTruncated);

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

For more information about how to list multiple inventory configurations at a time, see [ListBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/ListBucketInventory.md).

## Delete inventory configurations

The following code provides an example on how to delete the inventory configurations of a bucket:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";

    /* Specify the inventory ID. */
    std::string InventoryId = "yourInventoryId";

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);

    /* Delete the inventory configuration. */
    auto outcome = client.DeleteBucketInventoryConfiguration(
        DeleteBucketInventoryConfigurationRequest(BucketName, InventoryId));;

    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "Delete Bucket Inventory fail" <<
        ",code:" << outcome.error().Code() <<
        ",message:" << outcome.error().Message() <<
        ",requestId:" << outcome.error().RequestId() << std::endl;
    }

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

For more information about how to delete the inventory configurations of a bucket, see [DeleteBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/DeleteBucketInventory.md).

