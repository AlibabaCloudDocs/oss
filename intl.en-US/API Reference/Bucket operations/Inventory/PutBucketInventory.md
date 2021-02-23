# PutBucketInventory

You can call this operation to configure inventories for a bucket.

**Note:** For more information about the bucket inventory feature, see [Bucket inventory](/intl.en-US/Developer Guide/Buckets/Bucket inventory.md)

## Usage notes

You can use the bucket inventory feature to export the information about specific objects in a bucket, such as the number, sizes, storage classes, and encryption status of the objects. Take note of the following items when you configure inventories for a bucket:

-   Only the bucket owner or users that have the PutBucketInventory permission can initiate a PutBucketInventory request.
-   You can configure up to 1,000 inventories for a bucket.
-   The inventory list must be stored in a bucket within the same region as the bucket for which the inventory is configured.

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
|Id|String|Yes|report1|The name of the inventory list. The name must be globally unique within the bucket.|
|IsEnabled|Boolean|Yes|true|Specifies whether the bucket inventory feature is enabled. Valid values:

-   true: enables bucket inventory.
-   false: disables bucket inventory. |
|Filter|Container|No|N/A|The container that stores the prefix used to filter the objects included in the inventory list. Only objects whose names contain the specified prefix are included in the inventory list.|
|Prefix|String|No|Pics|The prefix specified in the inventory. Parent nodes: Filter |
|Destination|Container|Yes|N/A|The container that stores the information about the exported inventory list.|
|OSSBucketDestination|Container|Yes|N/A|The container that stores the information about the bucket in which the exported inventory list is stored. Parent nodes: Destination |
|Format|String|Yes|CSV|The format of the exported inventory list. Valid value: CSV

Parent nodes: OSSBucketDestination |
|AccountId|String|Yes|100000000000000|The account ID granted by the bucket owner to perform the PutBucketInventory operation. Parent nodes: OSSBucketDestination |
|RoleArn|String|Yes|acs:ram::100000000000000:role/AliyunOSSRole|The name of the role to which the bucket owner grants permissions to perform the PutBucketInventory operation. Format: `acs:ram::uid:role/rolename`.Parent nodes: OSSBucketDestination |
|Bucket|String|Yes|acs:oss:::bucket\_0001|The name of the bucket in which the exported inventory list is stored. Parent nodes: OSSBucketDestination |
|Prefix|String|No|prefix1|The path of the exported inventory list. Parent nodes: OSSBucketDestination |
|Encryption|Container|No|N/A|The container that stores the encryption method of the inventory list. Valid values:

-   SSE-OSS: The SSE-OSS method is used to encrypt and decrypt the inventory list.
-   SSE-KMS: The default customer master key \(CMK\) or a specified CMK to encrypt and decrypt the inventory list.

Parent nodes: OSSBucketDestination

For more information about server-side encryption, see [Server-side encryption](/intl.en-US/Developer Guide/Data security/Data encryption/Server-side encryption.md). |
|SSE-OSS|Container|No|N/A|The container that stores the information about the SSE-OSS encryption method. Parent nodes: Encryption |
|SSE-KMS|Container|No|N/A|The container that stores the CMK used in the SSE-KMS encryption method. Parent nodes: Encryption |
|KeyId|String|No|keyId|The ID of the CMK used in the SSE-KMS encryption method.Parent nodes: SSE-KMS |
|Schedule|Container|Yes|N/A|The container that stores the frequency at which inventory lists are exported.|
|Frequency|String|Yes|Daily|The frequency at which inventory lists are exported. Valid values:

-   Daily: Inventory lists are exported on a daily basis.
-   Weekly: Inventory lists are exported on a weekly basis.

Parent nodes: Schedule |
|IncludedObjectVersions|String|Yes|All|Specifies whether versioning information about the objects is included in the inventory list. Valid values:

-   All: All versions of the objects are exported.
-   Current: Only the current versions of the objects are exported. |
|OptionalFields|Container|No|N/A|The container that stores the configuration fields included in the inventory list.|
|Field|String|No|Size|The configuration fields included in the inventory list. -   Size: The size of the object.
-   LastModifiedDate: The last modified time of the object.
-   ETag: The ETag value of the object, which is used to identify the content of the object.
-   StorageClass: The storage class of the object.
-   IsMultipartUploaded: Indicates whether the object is uploaded by using multipart upload.
-   EncryptionStatus: The encryption status of the object. |

## Response headers

Only common response headers are included in the response to a PutBucketInventory request, such as Content-Length and Data. For more information, see [Common response headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Examples

-   Sample requests

    ```
      PUT /? inventory&inventoryId=report1 HTTP/1.1
      Host: BucketName.oss.aliyuncs.com
      Date: Mon, 31 Oct 2016 12:00:00 GMT
      Authorization: authorization string
      Content-Length: length
    
      <? xml version="1.0" encoding="UTF-8"? >
      <InventoryConfiguration>
         <Id>56594298207FB304438516F9</Id>
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
|InventoryExceedLimit|400|The error message returned because the number of inventories to configure exceeds the limit.|
|AccessDenied|403|-   The error message returned because the authentication information is not included in the PutBucketInventory request.
-   The error message returned because you are not authorized to perform this operation. |
|InventoryAlreadyExist|409|The error message returned because the inventory that you want to configure already exists.|

