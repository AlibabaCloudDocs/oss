# ListBucketInventory

You can call this operation to query all inventory tasks configured for a bucket.

**Note:**

-   You can query up to 100 inventory configurations by sending a request. To query more than 100 inventory configurations, you must send multiple requests and keep the token of each request to use it as the parameter for the next request.
-   To call this operation, ensure that you have permissions to perform operations on the inventory tasks of the bucket. By default, the bucket owner has the permission to perform this operation. If you do not have the permission, apply for the permission from the bucket owner.

## Request syntax

-   Requests that contain continuation-token

    ```
    GET /? inventory&continuation-token=xxx HTTP/1.1
    ```

-   Requests that do not contain continuation-token

    ```
    GET /? inventory HTTP/1.1
    ```


## Response elements

|Element|Type|Description|
|-------|----|-----------|
|InventoryConfiguration|Container|The container that stores inventory configurations.|
|IsTruncated|Boolean|Specifies whether to list all inventory tasks configured for the bucket. Valid values: true and false

-   The value of false indicates that all inventory tasks configured for the bucket are listed.
-   The value of true indicates that not all inventory tasks configured for the bucket are listed. To list the next page of inventory configurations, set the continuation-token parameter in the next request to the value of the NextContinuationToken header in the response to the current request. |
|NextContinuationToken|String|If the value of IsTruncated in the response is true and value of this header is not null, set the continuation-token parameter in the next request to the value of this header.|
|Id|String|The specified inventory list name, which must be globally unique in the bucket.|
|IsEnabled|Boolean|Indicates whether the inventory function is enabled. Valid values: true and false

-   The value of true indicates that the inventory function is enabled.
-   The value of false indicates that no inventory list is generated. |
|Filter|Container|The container that stores the prefix used to filter the objects in the inventory list. Only objects with the specified prefix are included in the inventory list.|
|Prefix|String|The prefix specified in the inventory rule. Parent node: Filter |
|Destination|Container|The container used to store the information about the bucket that stores the exported inventory list.|
|OSSBucketDestination|Container|The information about the bucket that stores the exported inventory list. Parent node: Destination |
|Format|String|The format of the exported inventory list. Valid value: CSV

Parent node: OSSBucketDestination |
|AccountId|String|The account ID granted by the bucket owner. Parent node: OSSBucketDestination |
|RoleArn|String|The name of the role to which the bucket owner grants permissions. Format：acs:ram::uid:role/rolename

Parent node: OSSBucketDestination |
|Bucket|String|The bucket that stores the exported inventory list. Parent node: OSSBucketDestination |
|Prefix|String|The path of the exported inventory list. Parent node: OSSBucketDestination |
|Encryption|Container|The container that stores the encryption method of the inventory list. Valid values: SSE-OSS, SSE-KMS, and Null

Parent node: OSSBucketDestination |
|SSE-OSS|Container|The container that stores the information about the SSE-OSS encryption method. Parent node: Encryption |
|SSE-KMS|Container|The container that stores the CMK used in the SSE-KMS encryption method. Parent node: Encryption |
|KeyId|String|The CMK used in the SSE-KMS encryption method. Parent node: SSE-KMS |
|Schedule|Container|The container that stores the frequency that inventory lists are exported.|
|Frequency|String|The frequency that inventory lists are exported. Valid values: Daily and Weekly

Parent node: Schedule |
|IncludedObjectVersions|String|Specifies whether versioning information about the objects is included in the inventory list. Valid values: All and Current

-   The value of All indicates that all versions of the objects are exported.
-   The value of Current indicates that only the current versions of the objects are exported. |
|OptionalFields|Container|The container that stores the configuration fields included in the inventory list.|
|Field|Container|The configuration fields included in the inventory list. Valid values: Size, LastModifiedDate, ETag, StorageClass, IsMultipartUploaded, and EncryptionStatus

Parent node: OptionalFields |

## Examples

-   Sample request

    ```
      GET /? inventory HTTP/1.1
      Host: BucketName.oss.aliyuncs.com
      Date: Fri, 24 Feb 2012 03:55:00 GMT
      Authorization: authorization string
      Content-Type: text/plain
    ```

-   Sample response

    ```
      HTTP/1.1 200 OK
      x-oss-request-id: 56594298207FB304438516F9
      Date: Sat, 30 Apr 2016 23:29:37 GMT
      Content-Type: application/xml
      Content-Length: length
      Connection: close
      Server: AliyunOSS
    
      <? xml version="1.0" encoding="UTF-8"? >
      <ListInventoryConfigurationsResult>
         <InventoryConfiguration>
            <Id>report1</Id>
            <IsEnabled>true</IsEnabled>
            <Destination>
               <OSSBucketDestination>
                  <Format>CSV</Format>
                  <AccountId>1000000000000000</AccountId>
                  <RoleArn>acs:ram::1000000000000000:role/AliyunOSSRole</RoleArn>
                  <Bucket>acs:oss:::destination-bucket</Bucket>
                  <Prefix>prefix1</Prefix>
               </OSSBucketDestination>
            </Destination>
            <Schedule>
               <Frequency>Daily</Frequency>
            </Schedule>
            <Filter>
               <Prefix>prefix/One</Prefix>
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
         <InventoryConfiguration>
            <Id>report2</Id>
            <IsEnabled>true</IsEnabled>
            <Destination>
               <OSSBucketDestination>
                  <Format>CSV</Format>
                  <AccountId>1000000000000000</AccountId>
                  <RoleArn>acs:ram::1000000000000000:role/AliyunOSSRole</RoleArn>
                  <Bucket>acs:oss:::destination-bucket</Bucket>
                  <Prefix>prefix2</Prefix>
               </OSSBucketDestination>
            </Destination>
            <Schedule>
               <Frequency>Daily</Frequency>
            </Schedule>
            <Filter>
               <Prefix>prefix/Two</Prefix>
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
         <InventoryConfiguration>
            <Id>report3</Id>
            <IsEnabled>true</IsEnabled>
            <Destination>
               <OSSBucketDestination>
                  <Format>CSV</Format>
                  <AccountId>1000000000000000</AccountId>
                  <RoleArn>acs:ram::1000000000000000:role/AliyunOSSRole</RoleArn>
                  <Bucket>acs:oss:::destination-bucket</Bucket>
                  <Prefix>prefix3</Prefix>
               </OSSBucketDestination>
            </Destination>
            <Schedule>
               <Frequency>Daily</Frequency>
            </Schedule>
            <Filter>
               <Prefix>prefix/Three</Prefix>
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
          ...
         <IsTruncated>true</IsTruncated>
         <NextContinuationToken>... </NextContinuationToken> 
      </ListInventoryConfigurationsResult>
    ```


