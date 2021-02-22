# Bucket inventory

This topic describes how to add, view, list, and delete the inventory configurations of a bucket.

**Note:** Before you can perform these operations, you must have the appropriate permissions on the bucket. By default, these permissions are available only to the bucket owner. If you do not have the permissions to perform these operations, apply for the permissions from the bucket owner.

## Add inventory configurations

**Note:**

-   You can have up to 1,000 inventory configurations per bucket.
-   The inventory list must be stored in a bucket that is in the same region as the bucket that you want to inventory.

The following code provides an example on how to add an inventory configuration for a bucket:

```
// This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String objectName = "<yourObjectName>";
String destBucketName ="<yourDestinationBucketName>";
String accountId ="<yourDestinationBucketAccountId>";
String roleArn ="<yourDestinationBucketRoleArn>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Create an inventory configuration.
InventoryConfiguration inventoryConfiguration = new InventoryConfiguration();

// Specify the inventory ID.
String inventoryId = "testid";
inventoryConfiguration.setInventoryId(id);

// Specify the object attributes to include in the inventory list.
List<String> fields = new ArrayList<String>();
fields.add(InventoryOptionalFields.Size);
fields.add(InventoryOptionalFields.LastModifiedDate);
fields.add(InventoryOptionalFields.IsMultipartUploaded);
fields.add(InventoryOptionalFields.StorageClass);
fields.add(InventoryOptionalFields.ETag);
fields.add(InventoryOptionalFields.EncryptionStatus);
inventoryConfiguration.setOptionalFields(fields);

// Specify whether to generate the inventory on a daily or weekly basis. The following code provides an example on how to generate an inventory list on a weekly basis. Weekly indicates once a week. Daily indicates once a day.
inventoryConfiguration.setSchedule(new InventorySchedule().withFrequency(InventoryFrequency.Weekly));

// Set the inventory list to include only the current version. If you set the InventoryIncludedObjectVersions parameter to All, all versions are inventoried. Versioning must be enabled for this configuration to take effect.
inventoryConfiguration.setIncludedObjectVersions(InventoryIncludedObjectVersions.Current);

// Specify whether the inventory is enabled. True indicates that the inventory list is generated, while false indicates that no inventory list is generated.
inventoryConfiguration.setEnabled(true);

// Specify the rule used to filter the objects to include in the inventory list. The following code provides an example on how to filter the objects by prefix.
InventoryFilter inventoryFilter = new InventoryFilter().withPrefix("obj-prefix");
inventoryConfiguration.setInventoryFilter(inventoryFilter);

// Configure the destination bucket to contain the inventory results.
InventoryOSSBucketDestination ossInvDest = new InventoryOSSBucketDestination();
// Specify the prefix of the inventory list object.
ossInvDest.setPrefix("destination-prefix");
// Specify the output format of the inventory results.
ossInvDest.setFormat(InventoryFormat.CSV);
// Specify the ID of the account that owns the destination bucket.
ossInvDest.setAccountId(accountId);
// Specify the role ARN of the destination bucket.
request.setRoleArn(roleArn);
// Specify the name of the destination bucket.
ossInvDest.setBucket(destBucketName);

// The following code provides an example on how to encrypt the inventory list object by using CMKs hosted in KMS.
// InventoryEncryption inventoryEncryption = new InventoryEncryption();
// InventoryServerSideEncryptionKMS serverSideKmsEncryption = new InventoryServerSideEncryptionKMS().withKeyId("test-kms-id");
// inventoryEncryption.setServerSideKmsEncryption(serverSideKmsEncryption);
// ossInvDest.setEncryption(inventoryEncryption);

// The following code provides an example on how to encrypt the inventory list object on the OSS server.
// InventoryEncryption inventoryEncryption = new InventoryEncryption();
// inventoryEncryption.setServerSideOssEncryption(new InventoryServerSideEncryptionOSS());
// ossInvDest.setEncryption(inventoryEncryption);

// Specify the destination for the inventory results.
InventoryDestination destination = new InventoryDestination();
destination.setOssBucketDestination(ossInvDest);
inventoryConfiguration.setDestination(destination);

// Upload the inventory configuration.
ossClient.setBucketInventoryConfiguration(bucketName, inventoryConfiguration);

// Shut down the OSSClient instance.
ossClient.shutdown();
```

For more information about how to add an inventory configuration for a bucket, see [PutBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/PutBucketInventory.md).

## View inventory configurations

The following code provides an example on how to view the inventory configurations of a bucket:

```
// This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String inventoryId = "<yourInventoryConfigurationId>";

// Specify the inventory ID to obtain the inventory configuration.
GetBucketInventoryConfigurationRequest request = new GetBucketInventoryConfigurationRequest(bucketName, inventoryId);
GetBucketInventoryConfigurationResult getResult = ossClient.getBucketInventoryConfiguration(request);

# Obtain the information about the inventory configuration.
InventoryConfiguration config = getResult.getInventoryConfiguration();
System.out.println("=====Inventory configuration=====");
System.out.println("inventoryId:" + config.getInventoryId());
System.out.println("isenabled:" + config.isEnabled());
System.out.println("includedVersions:" + config.getIncludedObjectVersions());
System.out.println("schdule:" + config.getSchedule().getFrequency());
if (config.getInventoryFilter().getPrefix()! = null) {
    System.out.println("filter, prefix:" + config.getInventoryFilter().getPrefix());
}

List<String> fields = config.getOptionalFields();
for (String field : fields) {
    System.out.println("field:" + field);
}

System.out.println("===bucket destination config===");
InventoryOSSBucketDestination destin = config.getDestination().getOssBucketDestination();
System.out.println("format:" + destin.getFormat());
System.out.println("bucket:" + destin.getBucket());
System.out.println("prefix:" + destin.getPrefix());
System.out.println("accountId:" + destin.getAccountId());
System.out.println("roleArn:" + destin.getRoleArn());
if (destin.getEncryption() ! = null) {
    if (destin.getEncryption().getServerSideKmsEncryption() ! = null) {
        System.out.println("server-side kms encryption, key id:" + destin.getEncryption().getServerSideKmsEncryption().getKeyId());
    } else if (destin.getEncryption().getServerSideOssEncryption() ! = null) {
        System.out.println("server-side oss encryption.") ;
    }
}

// Shut down the OSSClient instance.
ossClient.shutdown();
```

For more information about how to view inventory configurations, see [GetBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/GetBucketInventory.md).

## List multiple inventory configurations at a time

**Note:** You can obtain up to 100 inventory configurations per request. To obtain more than 100 inventory configurations, you must send multiple requests. Each subsequent request must use the token returned in the previous request.

The following code provides an example on how to list all inventory configurations of a bucket at a time:

```
// This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// List the inventory configurations. By default, a maximum of 100 results are listed. If the results are more than 100, the results are returned in pages. You can pass in the token to list the results on the subsequent page.
String continuationToken = null;
while (true) {
    ListBucketInventoryConfigurationsRequest listRequest = new ListBucketInventoryConfigurationsRequest(bucketName, continuationToken);
    ListBucketInventoryConfigurationsResult result = ossClient.listBucketInventoryConfigurations(listRequest);
    System.out.println("=======List bucket inventory configuration=======");
    System.out.println("istruncated:" + result.isTruncated());
    System.out.println("continuationToken:" + result.getContinuationToken());
    System.out.println("nextContinuationToken:" + result.getNextContinuationToken());
    System.out.println("list size :" + result.getInventoryConfigurationList
    if (result.getInventoryConfigurationList() ! = null && ! result.getInventoryConfigurationList().isEmpty()) {
        for (InventoryConfiguration config: result.getInventoryConfigurationList()) {
            System.out.println("===Inventory configuration===");
            System.out.println("inventoryId:" + config.getInventoryId());
            System.out.println("isenabled:" + config.isEnabled());
            System.out.println("includedVersions:" + config.getIncludedObjectVersions());
            System.out.println("schdule:" + config.getSchedule().getFrequency());
            if (config.getInventoryFilter().getPrefix()! = null) {
                System.out.println("filter, prefix:" + config.getInventoryFilter().getPrefix());
            }

            List<String> fields = config.getOptionalFields();
            for (String field: fields) {
                System.out.println("field:" + field);
            }

            System.out.println("===bucket destination config===");
            InventoryOSSBucketDestination destin = config.getDestination().getOssBucketDestination();
            System.out.println("format:" + destin.getFormat());
            System.out.println("bucket:" + destin.getBucket());
            System.out.println("prefix:" + destin.getPrefix());
            System.out.println("accountId:" + destin.getAccountId());
            System.out.println("roleArn:" + destin.getRoleArn());
            if (destin.getEncryption() ! = null) {
                if (destin.getEncryption().getServerSideKmsEncryption() ! = null) {
                    System.out.println("server-side kms encryption key id:" + destin.getEncryption().getServerSideKmsEncryption().getKeyId());
                } else if (destin.getEncryption().getServerSideOssEncryption() ! = null) {
                    System.out.println("server-side oss encryption.");
                }
            }
        }

    if (result.isTruncated()) {
        continuationToken = result.getNextContinuationToken();
    } else {
       break;
    }
}

// Shut down the OSSClient instance.
ossClient.shutdown(); 
```

For more information about how to list multiple inventory configurations at a time, see [ListBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/ListBucketInventory.md).

## Delete inventory configurations

The following code provides an example on how to delete the inventory configurations of a bucket:

```
// This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String inventoryId = "<yourInventoryConfigurationId>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Delete the inventory configuration specified by the inventory ID.
ossClient.deleteBucketInventoryConfiguration(bucketName, inventoryId);

// Shut down the OSSClient instance.
ossClient.shutdown(); 
```

For more information about how to delete the inventory configurations of a bucket, see [DeleteBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/DeleteBucketInventory.md).

