# GetBucketInventory

You can call this operation to query the specified inventory task configured for a bucket.

**Note:** To call this operation, ensure that you have permissions to perform operations on the inventory tasks of the bucket. By default, the bucket owner has the permission to perform this operation. If you do not have the permission, apply for the permission from the bucket owner.

## Request syntax

```
GET /? inventory&inventoryId=inventoryId HTTP/1.1
```

## Request elements

|Element|Type|Required|Description|
|-------|----|--------|-----------|
|inventoryId|String|Yes|The ID of the inventory rule to query.|

## Response elements

|Element|Type|Required|Description|
|-------|----|--------|-----------|
|Id|String|Yes|The specified inventory list name, which must be globally unique in the bucket.|
|IsEnabled|Boolean|Yes|Indicates whether the inventory function is enabled. Valid values: true and false

-   The value of true indicates that the inventory function is enabled.
-   The value of false indicates that no inventory list is generated. |
|Filter|Container|No|The container that stores the prefix used to filter the objects in the inventory list. Only objects with the specified prefix are included in the inventory list.|
|Prefix|String|No|The prefix specified in the inventory rule. Parent node: Filter |
|Destination|Container|Yes|The container used to store the information about the bucket that stores the exported inventory list.|
|OSSBucketDestination|Container|Yes|The information about the bucket that stores the exported inventory list. Parent node: Destination |
|Format|String|Yes|The format of the exported inventory list. Valid value: CSV

Parent node: OSSBucketDestination |
|AccountId|String|Yes|The account ID granted by the bucket owner. Parent node: OSSBucketDestination |
|RoleArn|String|Yes|The name of the role to which the bucket owner grants permissions. Format: acs:ram::uid:role/rolename

Parent node: OSSBucketDestination |
|Bucket|String|Yes|The bucket that stores the exported inventory list. Parent node: OSSBucketDestination |
|Prefix|String|No|The path of the exported inventory list. Parent node: OSSBucketDestination |
|Encryption|Container|No|The container that stores the encryption method of the inventory list. Valid values: SSE-OSS, SSE-KMS, and Null

Parent node: OSSBucketDestination |
|SSE-OSS|Container|No|The container that stores the information about the SSE-OSS encryption method. Parent node: Encryption |
|SSE-KMS|Container|No|The container that stores the CMK used in the SSE-KMS encryption method. Parent node: Encryption |
|KeyId|String|No|The CMK used in the SSE-KMS encryption method. Parent node: SSE-KMS |
|Schedule|Container|Yes|The container that stores the frequency that inventory lists are exported.|
|Frequency|String|Yes|The frequency that inventory lists are exported. Valid values: Daily and Weekly

Parent node: Schedule |
|IncludedObjectVersions|String|Yes|Specifies whether versioning information about the objects is included in the inventory list. Valid values: All and Current

-   The value of All indicates that all versions of the objects are exported.
-   The value of Current indicates that only the current versions of the objects are exported. |
|OptionalFields|Container|No|The container that stores the configuration fields included in the inventory list.|
|Field|Container|No|The configuration fields included in the inventory list. Valid values: Size, LastModifiedDate, ETag, StorageClass, IsMultipartUploaded, and EncryptionStatus

Parent node: OptionalFields |

## Examples

-   Sample request

    ```
    GET /? inventory&inventoryId=list1 HTTP/1.1
    ```

-   Sample response

    ```
      HTTP/1.1 200 OK
      x-oss-request-id: 56594298207FB304438516F9
      Date: Mon, 31 Oct 2016 12:00:00 GMT
      Server: AliyunOSS
      Content-Length: length
    
      <? xml version="1.0" encoding="UTF-8"? >
      <InventoryConfiguration">
         <Id>report1</Id>
         <IsEnabled>true</IsEnabled>
         <Destination>
            <OSSBucketDestination>
               <Format>CSV</Format>
               <AccountId>1000000000000000</AccountId>
               <RoleArn>acs:ram::1000000000000000:role/AliyunOSSRole</RoleArn>
               <Bucket>acs:oss:::bucket_0001</Bucket>
               <Prefix>prefix1</Prefix>
               <Encryption>
                  <SSE-OSS/>
               </Encryption>
            </OSSBucketDestination>
         </Destination>
         <Schedule>
            <Frequency>Daily</Frequency>
         </Schedule>
         <Filter>
           <Prefix>myprefix/</Prefix>
         </Filter>
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


