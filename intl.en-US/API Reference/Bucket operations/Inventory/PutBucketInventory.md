# PutBucketInventory

Configures inventories for a bucket.

## Usage notes

You can use the bucket inventory feature to export the information about specific objects in a bucket, such as the number, sizes, storage classes, and encryption status of the objects. When you configure inventories for a bucket, take note of the following items:

-   Only the bucket owner or users that have the PutBucketInventory permission on the bucket can initiate a PutBucketInventory request.
-   Before you configure inventories, you must create a Resource Access Management \(RAM\) role. The RAM role must have permissions to read all objects from the source bucket and write objects to the destination bucket. If you use the inventory feature for the first time, we recommend that you configure this feature in the Object Storage Service \(OSS\) console. After you configure an inventory, you can obtain the RAM role that has permissions to read objects from the source bucket and write objects to the destination bucket. For more information about permissions of the RAM role for the inventory feature, see [Bucket inventory](/intl.en-US/Developer Guide/Buckets/Bucket inventory.md).
-   You can configure up to 1,000 inventories for a bucket.
-   Inventory lists must be stored in a bucket in the same region as the bucket for which the inventory feature is configured.

## Request structure

```
  <InventoryConfiguration>
     <Id>report1</Id>
     <IsEnabled>true</IsEnabled>
     <Filter>
        <Prefix>filterPrefix</Prefix>
     </Filter>
     <Destination>
        <OSSBucketDestination>
           <Format>CSV</Format>
           <AccountId>1000000000000000</AccountId>
           <RoleArn>acs:ram::1000000000000000:role/AliyunOSSRole</RoleArn>
           <Bucket>acs:oss:::destination-bucket</Bucket>
           <Prefix>prefix1</Prefix>
           <Encryption>
              <SSE-KMS>
                 <KeyId>keyId</KeyId>
              </SSE-KMS>
           </Encryption>
        </OSSBucketDestination>
     </Destination>
     <Schedule>
        <Frequency>Daily</Frequency>
     </Schedule>
     <IncludedObjectVersions>All</IncludedObjectVersions>
     <OptionalFields>
        <Field>Size</Field>
        <Field>LastModifiedDate</Field>
        <Field>ETag</Field>
        <Field>StorageClass</Field>
        <Field>IsMultipartUploaded</Field>
        <Field>EncryptionStatus</Field>
     </OptionalFields>
  </InventoryConfiguration>
```

## Request elements

|Element|Type|Required|Example|Description|
|-------|----|--------|-------|-----------|
|Id|String|Yes|report1|The name of the inventory. The name must be globally unique in the bucket.|
|IsEnabled|Boolean|Yes|true|Specifies whether to enable the bucket inventory feature. Valid values:

-   true: The inventory feature is enabled.
-   false: The inventory feature is disabled. |
|Filter|Container|No|N/A|The container that stores the prefix used to filter objects. Only objects whose names contain the specified prefix are included in the inventory list.|
|Prefix|String|No|Pics|The prefix specified in the inventory. Parent nodes: Filter |
|Destination|Container|Yes|N/A|The container that stores the information about exported inventory lists.|
|OSSBucketDestination|Container|Yes|N/A|The container that stores the information about the bucket in which exported inventory lists are stored. Parent nodes: Destination |
|Format|String|Yes|CSV|The format of exported inventory lists. The exported inventory lists are comma-separated values \(CSV\) objects compressed by using GZIP. Set the value to CSV. 

Parent nodes: OSSBucketDestination |
|AccountId|String|Yes|100000000000000|The account ID granted by the bucket owner to perform the PutBucketInventory operation. Parent nodes: OSSBucketDestination |
|RoleArn|String|Yes|acs:ram::100000000000000:role/AliyunOSSRole|The Alibaba Cloud Resource Name \(ARN\) of the role that has permissions to read all objects from the source bucket and write objects to the destination bucket. Format: `acs:ram::uid:role/rolename`. Parent nodes: OSSBucketDestination |
|Bucket|String|Yes|acs:oss:::bucket\_0001|The name of the bucket in which exported inventory lists are stored. Parent nodes: OSSBucketDestination |
|Prefix|String|No|prefix1|The prefix of the exported inventory lists. Parent nodes: OSSBucketDestination |
|Encryption|Container|No|N/A|The container that stores the encryption method of inventory lists. Valid values:

-   SSE-OSS: The SSE-OSS method is used to encrypt and decrypt inventory lists.
-   SSE-KMS: The default customer master key \(CMK\) or a specified CMK is used to encrypt and decrypt inventory lists.

Parent nodes: OSSBucketDestination

For more information about server-side encryption, see [Server-side encryption](/intl.en-US/Developer Guide/Data security/Data encryption/Server-side encryption.md). |
|SSE-OSS|Container|No|N/A|The container that stores the information about the SSE-OSS encryption method. Parent nodes: Encryption |
|SSE-KMS|Container|No|N/A|The container that stores the CMK used for SSE-KMS encryption. Parent nodes: Encryption |
|KeyId|String|No|keyId|The ID of the CMK used for SSE-KMS encryption. Parent nodes: SSE-KMS |
|Schedule|Container|Yes|N/A|The container that stores the information about the frequency at which inventory lists are exported.|
|Frequency|String|Yes|Daily|The frequency at which inventory lists are exported. Valid values:

-   Daily: Inventory lists are exported on a daily basis.
-   Weekly: Inventory lists are exported on a weekly basis.

Parent nodes: Schedule |
|IncludedObjectVersions|String|Yes|All|Specifies whether to include the versioning information of objects in inventory lists. Valid values:

-   All: The information about all versions of the objects is exported.
-   Current: The information only about the current versions of the objects is exported. |
|OptionalFields|Container|No|N/A|The container that stores the configuration fields included in inventory lists.|
|Field|String|No|Size|The configuration fields included in inventory lists. -   Size: the size of the object.
-   LastModifiedDate: the last modified time of the object.
-   ETag: the ETag value of the object, which is used to identify the content of the object.
-   StorageClass: the storage class of the object.
-   IsMultipartUploaded: indicates whether the object is uploaded by using multipart upload.
-   EncryptionStatus: the encryption status of the object. |

## Response elements

The response headers involved in this API operation contain only common response headers such as Content-Length and Date. For more information, see [Common response headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Examples

-   Sample requests

    ```
      PUT /?inventory&inventoryId=report1 HTTP/1.1
      Host: BucketName.oss.aliyuncs.com
      Date: Mon, 31 Oct 2016 12:00:00 GMT
      Authorization: authorization string
      Content-Length: length
    
      <?xml version="1.0" encoding="UTF-8"?>
      <InventoryConfiguration>
         <Id>report1</Id>
         <IsEnabled>true</IsEnabled>
         <Filter>
            <Prefix>Pics</Prefix>
         </Filter>
         <Destination>
            <OSSBucketDestination>
               <Format>CSV</Format>
               <AccountId>100000000000000</AccountId>
               <RoleArn>acs:ram::100000000000000:role/AliyunOSSRole</RoleArn>
               <Bucket>acs:oss:::bucket_0001</Bucket>
               <Prefix>prefix1</Prefix>
               <Encryption>
                  <SSE-KMS>
                     <KeyId>keyId</KeyId>
                  </SSE-KMS>
               </Encryption>
            </OSSBucketDestination>
         </Destination>
         <Schedule>
            <Frequency>Daily</Frequency>
         </Schedule>
         <IncludedObjectVersions>All</IncludedObjectVersions>
         <OptionalFields>
            <Field>Size</Field>
            <Field>LastModifiedDate</Field>
            <Field>ETag</Field>
            <Field>StorageClass</Field>
            <Field>IsMultipartUploaded</Field>
            <Field>EncryptionStatus</Field>
         </OptionalFields>
      </InventoryConfiguration>
    ```

-   Sample responses

    ```
      HTTP/1.1 200 OK
      x-oss-request-id: 56594298207FB3044385****
      Date: Mon, 31 Oct 2016 12:00:00 GMT
      Content-Length: 0
      Server: AliyunOSS
    ```


## Error codes

|Error code|HTTP status code|Description|
|----------|----------------|-----------|
|InvalidArgument|400|The error message returned because the input parameter is invalid.|
|InventoryExceedLimit|400|The error message returned because the number of configured inventories has been reached.|
|AccessDenied|403|Possible causes:-   The authentication information is not included in the PutBucketInventory request.
-   You are not authorized to perform this operation. |
|InventoryAlreadyExist|409|The error message returned because the inventory that you want to configure already exists.|

