# GetBucketReplicationProgress

You can call this operation to query the progress of a data replication task configured for a bucket.

## Request structure

```
GET /?replicationProgress&rule-id=RuleId HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Request parameters

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|rule-id|String|No|The ID of the data replication rule. You can call GetBucketReplication to obtain the ID.|

## Response elements

|Element|Type|Description|
|-------|----|-----------|
|ReplicationProgress|Container|The container that is used to store the progress of data replication tasks. Parent nodes: none

Child nodes: Rule |
|Rule|Container|The container that stores the progress of the data replication task corresponding to each data replication rule. Parent nodes: ReplicationConfiguration

Child nodes: ID, Destination, Status, and Progress |
|ID|String|The ID of the data replication rule. Parent nodes: Rule

Child nodes: none |
|PrefixSet|Container|The container that stores prefixes. You can specify up to 10 prefixes in each data replication rule. Parent nodes: Rule

Child nodes: Prefix |
|Prefix|String|The prefix used to specify the object to replicate. Only objects that match the prefix are replicated to the destination bucket. Parent nodes: PrefixSet

Child nodes: none |
|Action|String|The operations that can be synchronized to the destination bucket. You can set Action to one or more of the following operation types. Default value: ALL.

-   ALL: PUT, DELETE, and ABORT operations are synchronized to the destination bucket.
-   PUT: Write operations are synchronized to the destination bucket, including PutObject, PostObject, AppendObject, CopyObject, PutObjectACL, InitiateMultipartUpload, UploadPart, UploadPartCopy, and CompleteMultipartUpload.

Parent nodes: Rule

Child nodes: none |
|Destination|Container|The container that is used to store the information about the destination bucket. Parent nodes: Rule

Child nodes: Bucket and Location |
|Bucket|String|The destination bucket to which the data is replicated. Parent nodes: Destination

Child nodes: none |
|Location|String|The region in which the destination bucket is located. Parent nodes: Destination

Child nodes: none |
|TransferType|String|The link used to transfer data in data replication. Default value: internal. -   internal: the default link.
-   oss\_acc: the link in which data transmission is accelerated. TransferType can be set to oss\_acc only for cross-region replication \(CRR\) rules. |
|HistoricalObjectReplication|String|Indicates whether historical data from the source bucket is replicated to the destination bucket before data replication is enabled. Default value: enabled. Valid values:

-   enabled: indicates that historical data is replicated to the destination bucket.
-   disabled: indicates that historical data is not replicated to the destination bucket. Only data uploaded to the source bucket after data replication is enabled for the source bucket is replicated. |
|Progress|Container|The container that stores the progress of the data replication task. This element is returned only when the data replication task is in the doing state. Parent nodes: Rule

Child nodes: HistoricalObject and NewObject |
|HistoricalObject|String|The percentage of the replicated historical data. This element is valid only when HistoricalObjectReplication is set to enabled. Parent nodes: Progress

Child nodes: none |
|NewObject|String|The time used to distinguish new data from historical data. Data that is written to the source bucket before the time is replicated to the destination bucket as new data. The value of this element is in GMT. Example: Thu, 24 Sep 2015 15:39:18 GMT.

Parent nodes: Progress

Child nodes: none |

## Examples

-   Sample requests

    ```
    GET /?replicationProgress&rule-id=test_replication_1 HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Thu, 24 Sep 2015 15:39:15 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:CTkuxpLAi4XZ+WwIfNm0Fmgb****
    ```

-   Sample responses

    **Note:** The TransferType element is contained in the XML body of the response only when the value of TransferType is set to oss\_acc in the request.

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Thu, 24 Sep 2015 15:39:15 GMT
    Content-Length: 234
    Content-Type: application/xml
    Connection: close
    Server: AliyunOSS
    
    
    <?xml version="1.0" ?>
    <ReplicationProgress>
     <Rule>
       <ID>test_replication_1</ID>
       <PrefixSet>
        <Prefix>source_image</Prefix>
        <Prefix>video</Prefix>
       </PrefixSet>
       <Action>PUT</Action>
       <Destination>
        <Bucket>target-bucket</Bucket>
        <Location>oss-cn-beijing</Location>
        <TransferType>oss_acc</TransferType>
       </Destination>
       <Status>doing</Status>
       <HistoricalObjectReplication>enabled</HistoricalObjectReplication>
       <Progress>
        <HistoricalObject>0.85</HistoricalObject>
        <NewObject>2015-09-24T15:28:14.000Z </NewObject>
       </Progress>
     </Rule>
    </ReplicationProgress>
    ```


## Error codes

|Error code|HTTP status code|Description|
|----------|----------------|-----------|
|NoSuchBucket|404 NotFound|The error message returned because the specified bucket does not exist.|
|NoSuchReplicationRule|404 NotFound|The error message returned because the specified rule ID does not exist.|
|NoSuchReplicationConfiguration|404 NotFound|The error message returned because no data replication rules are configured for the specified bucket.|
|TooManyReplicationRules|400 BadRequest|The error message returned because more than one data replication rule is configured in the request. You can configure only one data replication rule in a single request. |

