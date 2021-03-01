# PutBucketLifecycle

You can call this operation to configure lifecycle rules for a bucket. After you configure a lifecycle rule for a bucket, OSS automatically deletes the objects that match the rule or convert the storage class of the objects at the time specified in the lifecycle rule.

## Usage notes

When you call the PutBucketLifecycle operation, take note of the following items:

-   Only the bucket owner or RAM users that have the PutBucketLifecycle permission can initiate a PutBucketLifecycle request.
-   If no lifecycle rules are configured for a bucket, a lifecycle rule is created after you call PutBucketLifecycle. If a lifecycle rule is configured for the bucket, a new lifecycle rule is created and the existing lifecycle rule is overwritten.
-   PutBucketLifecycle uses the overwriting semantics. New lifecycle rules overwrite the existing rules. To add lifecycle rules for a bucket, call GetBucketLifecycle to obtain the existing lifecycle configurations of the bucket, add new lifecycle configurations, and call PutBucketLifecycle to update the lifecycle configurations for the bucket.
-   When you call the PutBucketLifecycle operation, you can set a validity period or expiration date for objects or parts that are generated in incomplete multipart upload tasks.

## Request structure

```
PUT /? lifecycle HTTP/1.1
Date: GMT Date
Content-Length: ContentLength
Content-Type: application/xml
Authorization: SignatureValue 
Host: BucketName.oss.aliyuncs.com
<? xml version="1.0" encoding="UTF-8"? >
<LifecycleConfiguration>
  <Rule>
    <ID>RuleID</ID>
    <Prefix>Prefix</Prefix>
    <Status>Status</Status>
    <Expiration>
      <Days>Days</Days>
    </Expiration>
    <Transition>
      <Days>Days</Days>
      <StorageClass>StorageClass</StorageClass>
    </Transition>
    <AbortMultipartUpload>
      <Days>Days</Days>
    </AbortMultipartUpload>
  </Rule>
</LifecycleConfiguration>
```

## Request headers

A PutBucketLifecycle request contains only common request headers. For more information, see [Common request headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Request elements

|Element|Type|Required|Example|Description|
|-------|----|--------|-------|-----------|
|LifecycleConfiguration|Container|Yes|N/A|The container that stores lifecycle configurations. The container can contain up to 1,000 lifecycle rules.Chile nodes: Rule

Parent nodes: none |
|Rule|Container|Yes|N/A|The container that stores lifecycle rules.-   A lifecycle rule cannot be configured to convert the storage class of objects in an Archive bucket.
-   The validity period specified for Expiration must be longer than that specified for Transition. Likewise, the expiration time specified for Expiration must be later than that specified for Transition.

Child nodes: ID, Prefix, Status, and Expiration

Parent nodes: LifecycleConfiguration |
|ID|String|No|rule1|The unique ID of a lifecycle rule. The ID can be up to 255 bytes in length. If the value of this parameter is null or not specified, OSS automatically generates a unique ID for the lifecycle rule.Child nodes: none

Parent nodes: Rule |
|Prefix|String|Yes|tmp/|The prefix of the objects to which the rule applies. The prefixes specified by different rules cannot overlap.-   If Prefix is specified, this rule applies only to objects whose names contain the specified prefix in the bucket.
-   If Prefix is not specified, this rule applies to all objects in the bucket.

Child nodes: none

Parent nodes: Rule |
|Status|String|Yes|Enabled|Specifies whether to run the rule. A value of Enabled indicates that OSS runs the rule. A value of Disabled indicates that OSS ignores the rule.Parent nodes: Rule

Valid values: Enabled and Disabled |
|Expiration|Container|No|N/A|The delete operation to perform on objects based on the lifecycle rule. For an object in a versioned bucket, the delete operation specified by this element is performed on only the current version of the object.Child nodes: Days, CreatedBeforeDate, or ExpiredObjectDeleteMarker

Parent nodes: Rule |
|Days|Positive integer|Either Days or CreatedBeforeDate is required|1|The number of days within which objects can be retained after they are last modified.Parent nodes: Expiration or AbortMultipartUpload |
|CreatedBeforeDate|String|Either Days or CreatedBeforeDate is required|2002-10-11T00:00:00.000Z|The date based on which the lifecycle rules are implemented. OSS performs the specified operation on data whose last modified date is before this date. Specify the time in the ISO 8601 standard. The time must be at UTC 00:00:00.Parent nodes: Expiration or AbortMultipartUpload |
|ExpiredObjectDeleteMarker|String|No|true|Specifies whether to automatically remove expired delete markers. Valid values:

-   true: Expired delete markers are automatically removed. When the value of this element is true, the Days or CreatedBeforeDate elements cannot be specified.
-   false: Expired delete markers are not automatically removed. When the value of this element is false, the Days or CreatedBeforeDate elements must be specified.

Parent nodes: Expiration |
|Transition|Container|No|N/A|The validity period within which objects can be retained and the conversion operation on the objects. After the validity period or expiration date, the storage class of the objects is converted to IA, Archive, or Cold Archive.The storage class of Standard objects can be converted to IA, Archive, or Cold Archive. The validity period for conversion to Archive must be longer than that for conversion to IA. For example, if the validity period is set to 30 for objects whose storage class is converted to IA after the validity period, the validity period must be set to a value greater than 30 for objects whose storage class is converted to Archive.

Parent nodes: Rule

Child nodes: Days, CreatedBeforeDate, and StorageClass

**Note:** Either Days or CreatedBeforeDate is required |
|StorageClass|String|Yes if Transition or NoncurrentVersionTransition is configured|IA|The storage class of objects after conversion.**Note:** You can convert the storage class of objects in an IA bucket to Archive or Cold Archive but not Standard.

Valid values: IA, Archive, and Cold Archive

Parent nodes: Transition |
|AbortMultipartUpload|Container|No|N/A|The operation to perform on incomplete multipart upload tasks.Child nodes: Days or CreatedBeforeDate

Parent nodes: Rule |
|Tag|Container|No|N/A|The object tags. Parent nodes: Rule

Child nodes: Key and Value |
|Key|String|Yes if Tag is configured|TagKey1|The key of the tag. Parent nodes: Tag |
|Value|String|Yes if Tag is configured|TagValue1|The value of the tag. Parent nodes: Tag |
|NoncurrentVersionExpiration|Container|No|N/A|The delete operation to perform on the previous versions of the object. Child nodes: NoncurrentDays |
|NoncurrentVersionTransition|Container|No|N/A|The validity period within which previous versions can be retained and the conversion operation on the previous versions. After the validity period, the storage class of the previous versions can be converted to IA or Archive. The validity period for conversion from Standard to Archive must be longer than that for conversion from Standard to IA.

Child nodes: NoncurrentDays and StorageClass |
|NoncurrentDays|String|Yes if NoncurrentVersionTransition or NoncurrentVersionExpiration is configured|5|The number of days within which the previous versions can be retained. When the current versions expire, they become the previous versions. Parent nodes: NoncurrentVersionTransition and NoncurrentVersionExpiration |

## Response headers

The response to a PutBucketLifecycle request contains only common response headers. For more information, see [Common response headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Examples

-   Sample requests for an unversioned bucket

    ```
    PUT /? lifecycle HTTP/1.1
    Host: oss-example.oss.aliyuncs.com
    Content-Length: 443
    Date: Thu , 8 Jun 2017 13:08:38 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:PYbzsdWAIWAlMW8luk****
    <? xml version="1.0" encoding="UTF-8"? >
    <LifecycleConfiguration>
      <Rule>
        <ID>delete objects and parts after one day</ID>
        <Prefix>logs/</Prefix>
        <Status>Enabled</Status>
        <Expiration>
          <Days>1</Days>
        </Expiration>
        <AbortMultipartUpload>
          <Days>1</Days>
        </AbortMultipartUpload>
      </Rule>
      <Rule>
        <ID>transit objects to IA after 30, to Archive 60, expire after 10 years</ID>
        <Prefix>data/</Prefix>
        <Status>Enabled</Status>
        <Transition>
          <Days>30</Days>
          <StorageClass>IA</StorageClass>
        </Transition>
        <Transition>
          <Days>60</Days>
          <StorageClass>Archive</StorageClass>
        </Transition>
        <Expiration>
          <Days>3600</Days>
        </Expiration>
      </Rule>
      <Rule>
        <ID>transit objects to Archive after 60 days</ID>
        <Prefix>important/</Prefix>
        <Status>Enabled</Status>
        <Transition>
          <Days>6</Days>
          <StorageClass>Archive</StorageClass>
        </Transition>
      </Rule>
      <Rule>
        <ID>delete created before date</ID>
        <Prefix>backup/</Prefix>
        <Status>Enabled</Status>
        <Expiration>
          <CreatedBeforeDate>2017-01-01T00:00:00.000Z</CreatedBeforeDate>
        </Expiration>
        <AbortMultipartUpload>
          <CreatedBeforeDate>2017-01-01T00:00:00.000Z</CreatedBeforeDate>
        </AbortMultipartUpload>
      </Rule>
      <Rule>
        <ID>r1</ID>
        <Prefix>rule1</Prefix>
        <Tag><Key>xx</Key><Value>1</Value></Tag>
        <Tag><Key>yy</Key><Value>2</Value></Tag>
        <Status>Enabled</Status>
        <Expiration>
          <Days>30</Days>
        </Expiration>
      </Rule>
      <Rule>
        <ID>r2</ID>
        <Prefix>rule2</Prefix>
        <Tag><Key>xx</Key><Value>1</Value></Tag>
        <Status>Enabled</Status>
        <Transition>
          <Days>60</Days>
          <StorageClass>Archive</StorageClass>
        </Transition>
      </Rule>
    </LifecycleConfiguration>            
    ```

    Sample responses

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674A4D890****
    Date: Thu , 8 Jun 2017 13:08:38 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   Sample requests for a versioned bucket

    ```
    PUT /? lifecycle HTTP/1.1
    Host: oss-example.oss.aliyuncs.com
    Content-Length: 336
    Date: Mon , 6 May 2019 15:23:20 GMT
    Authorization: OSSWnjl3fg9fdv8fg4b****:Phuu8bBhS8dsff2a****
    <? xml version="1.0" encoding="UTF-8"? >
    <LifecycleConfiguration>
      <Rule>
        <ID>delete example</ID>
        <Prefix>logs/</Prefix>
        <Status>Enabled</Status>
        <Expiration>
          <ExpiredObjectDeleteMarker>true</ExpiredObjectDeleteMarker>     
        </Expiration>
        <NoncurrentVersionExpiration>
          <NoncurrentDays>5</NoncurrentDays>
        </NoncurrentVersionExpiration>
        <AbortMultipartUpload>
          <Days>1</Days>
        </AbortMultipartUpload>
      </Rule>
      <Rule>
        <ID>transit example</ID>
        <Prefix>data/</Prefix>
        <Status>Enabled</Status>
        <Transition>
          <Days>30</Days>
          <StorageClass>IA</StorageClass>
        </Transition>
        <NoncurrentVersionTransition>
          <NoncurrentDays>10</NoncurrentDays>
          <StorageClass>IA</StorageClass>
        </NoncurrentVersionTransition>
      </Rule>
    </LifecycleConfiguration>
    ```

    Sample responses

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 7D3435J59A9812B*****
    Date: Mon , 6 May 2019 15:23:20 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```


## SDKs

You can use OSS SDKs for the following programming languages to call the PutBucketLifecycle operation:

-   [Java](/intl.en-US/SDK Reference/Java/Buckets/Manage lifecycle rules.md)
-   [Python](/intl.en-US/SDK Reference/Python/Buckets/Manage lifecycle rules.md)
-   [Go](/intl.en-US/SDK Reference/Go/Buckets/Manage lifecycle rules.md)
-   [C++](/intl.en-US/SDK Reference/C++/Buckets/Manage lifecycle rules.md)
-   [PHP](/intl.en-US/SDK Reference/PHP/Buckets/Manage lifecycle rules.md)
-   [C](/intl.en-US/SDK Reference/C/Buckets/Manage lifecycle rules.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Buckets/Manage lifecycle rules.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Buckets/Manage lifecycle rules.md)
-   [Ruby](/intl.en-US/SDK Reference/Ruby/Buckets/Manage lifecycle rules.md)

## Error codes

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|AccessDenied|403|The error message returned because you are not authorized to perform the PutBucketLifecycle operation. Only users that have the PutBucketLifecycle permission can configure lifecycle rules for a bucket.|
|InvalidArgument|400|-   The storage class of objects in a Standard bucket can be converted from Standard to IA or Archive. When you configure lifecycle rules, you can configure conversion to Standard and conversion to IA at the same time. The validity period for conversion to Archive must be longer than that for conversion to IA.
-   The validity period specified for Expiration must be longer than that specified for Transition. Likewise, the expiration time specified for Expiration must be later than that specified for Transition. |

