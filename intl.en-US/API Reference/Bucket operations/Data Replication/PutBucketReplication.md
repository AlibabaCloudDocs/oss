PutBucketReplication 
=========================================

You can call this operation to configure data replication rules for a bucket. OSS supports two data replication features: cross-region replication (CRR) and same-region replication (SRR). 

Usage notes 
--------------------------------

You can use data replication features to automatically synchronize operations performed on objects in a source bucket to objects in a destination bucket. These operations include the creating, overwriting, and deletion of objects. When you use data replication features, take note of the following items:

* Data replication is an asynchronous process. Based on the size of the data to replicate, it takes several minutes to several hours to replicate data from the source bucket to the destination bucket.

  

* The source bucket and destination bucket cannot have the same name.

  

* When you use CRR, the source bucket and destination bucket must be located in different regions. When you use SRR, the source bucket and destination bucket must be located in the same region.

  




For more information, see [Cross-region replication](/intl.en-US/Developer Guide/Data security/Disaster recovery/Cross-region replication.md) and [Same-region replication](/intl.en-US/Developer Guide/Data security/Disaster recovery/Same-region replication.md).

Request structure 
--------------------------------------

    POST /?replication&comp=add HTTP/1.1
    Date: GMT Date
    Content-Length: ContentLength
    Content-Type: application/xml
    Authorization: SignatureValue
    Host: BucketName.oss-cn-hangzhou.aliyuncs.com
    
    <?xml version="1.0" encoding="UTF-8"?>
    <ReplicationConfiguration>
       <Rule>     
            <PrefixSet>
                <Prefix>prefix_1</Prefix>
                <Prefix>prefix_2</Prefix>
            </PrefixSet>
            <Action>ALL,PUT</Action>
            <Destination>
                <Bucket>Target Bucket Name</Bucket>
                <Location>cn-hangzhou</Location>
                <TransferType>oss_acc</TransferType>
            </Destination>
            <HistoricalObjectReplication>enabled or disabled</HistoricalObjectReplication>
       </Rule>
    </ReplicationConfiguration>



Request elements 
-------------------------------------



|           Element           |   Type    | Required |                                                                                                                                                                                                                                                                                                                                                                                                            Description                                                                                                                                                                                                                                                                                                                                                                                                             |
|-----------------------------|-----------|----------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ReplicationConfiguration    | Container | Yes      | The container that is used to store data replication rules.  Parent nodes: none Child nodes: Rule                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Rule                        | Container | Yes      | The container that is used to store the data replication rule.  Parent nodes: ReplicationConfiguration Child nodes: Destination, HistoricalObjectReplication, and ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ID                          | String    | No       | The ID of the data replication rule corresponding to the data replication task.  The ID of a data replication rule can be up to 255 characters in length and is unique in a bucket.  If the ID of a data replication rule is null or is not specified, OSS generates a unique ID for the rule.  Parent nodes: Rule Child nodes: none                                                                                                                                                                                                                                                                                                                                                                                                                               |
| PrefixSet                   | Container | No       | The container that is used to store prefixes. You can specify up to 10 prefixes in each data replication rule.  Parent nodes: Rule Child nodes: Prefix                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| Prefix                      | String    | No       | The prefix used to specify the object to replicate. Only objects that match the prefix are replicated to the destination bucket.  * The value of Prefix can be up to 1,023 characters in length.   * If you specify Prefix in a data replication rule, OSS synchronizes new data and historical data based on the specified Prefix value.    Parent nodes: PrefixSet Child nodes: none                                                                                                                                                                                                                                                                                                          |
| Action                      | String    | No       | The operations that can be synchronized to the destination bucket. If you configure Action in a data replication rule, OSS synchronizes new data and historical data based on the specified Action value.  You can set Action to one or more of the following operation types. Default value: ALL.  Valid values: * ALL: PUT, DELETE, and ABORT operations are synchronized to the destination bucket.   * PUT: Write operations are synchronized to the destination bucket, including PutObject, PostObject, AppendObject, CopyObject, PutObjectACL, InitiateMultipartUpload, UploadPart, UploadPartCopy, and CompleteMultipartUpload.    Parent nodes: Rule Child nodes: none |
| Destination                 | Container | Yes      | The container that is used to store the information about the destination bucket.  Parent nodes: Rule Child nodes: Bucket and Location                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| Bucket                      | String    | Yes      | The destination bucket to which the data is replicated.  Parent nodes: Destination Child nodes: none                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Location                    | String    | Yes      | The region in which the destination bucket is located.  Parent nodes: Destination Child nodes: none                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| TransferType                | String    | Yes      | The link used to transfer data in data replication. Default value: internal.  Valid values: * internal: the default link.   * oss_acc: the link in which data transmission is accelerated. You can set TransferType to oss_acc only when you create CRR rules.    Parent nodes: Destination Child nodes: none                                                                                                                                                                                                                                                                                                                                                                                   |
| HistoricalObjectReplication | String    | No       | Specifies whether to replicate historical data from the source bucket to the destination bucket before data replication is enabled. Default value: enabled.  Valid values: * enabled: indicates that historical data is replicated to the destination bucket.   * disabled: indicates that historical data is not replicated to the destination bucket. Only data uploaded to the source bucket after data replication is enabled for the source bucket is replicated.    Parent nodes: Rule Child nodes: none                                                                                                                                                                                  |
| SyncRole                    | String    | No       | The role that you authorize OSS to use to replicate data. If SSE-KMS is specified to encrypt the objects replicated to the destination bucket, SyncRole must be specified.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| SourceSelectionCriteria     | Container | No       | The container that specifies other conditions used to filter the source objects to replicate. Filter conditions can be specified for only source objects encrypted by using SSE-KMS.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| SseKmsEncryptedObjects      | Container | No       | The container used to filter source objects encrypted by using SSE-KMS. This element must be specified if SourceSelectionCriteria is specified in the data replication rule.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| Status                      | String    | No       | Specifies whether to replicate objects encrypted by using SSE-KMS.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| EncryptionConfiguration     | Container | No       | The encryption configuration of the objects replicated to the destination bucket.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ReplicaKmsKeyID             | String    | No       | The CMK ID used in SSE-KMS.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |



Examples 
-----------------------------

* Sample requests

  




    POST /?replication&comp=add HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com 
    Content-Type: application/xml
    Content-Length: 186
    Date: Thu, 24 Sep 2015 15:39:12 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:KU5h8YMUC78M30dXqf3JxrTZ****
    
    
    <?xml version="1.0" encoding="UTF-8"?>
    <ReplicationConfiguration>
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
         <HistoricalObjectReplication>enabled</HistoricalObjectReplication>
          <SyncRole>acs:ram::174649585760****:role/aliyunramrole</SyncRole>
          <SourceSelectionCriteria>
             <SseKmsEncryptedObjects>
               <Status>Enabled</Status>
             </SseKmsEncryptedObjects>
          </SourceSelectionCriteria>
          <EncryptionConfiguration>
               <ReplicaKmsKeyID>c4d49f85-ee30-426b-a5ed-95e9139d****</ReplicaKmsKeyID>
          </EncryptionConfiguration>     
      </Rule>
    </ReplicationConfiguration>



* Sample responses

  




    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Thu, 24 Sep 2015 15:39:12 GMT
    Content-Length: 0
    Connection: close
    Server: AliyunOSS



Error codes 
--------------------------------



|          Error code           | HTTP status code |                                                                                                                                                                                                                                 Description                                                                                                                                                                                                                                  |
|-------------------------------|------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| InvalidTargetBucket           | 400 BadRequest   | Possible causes: * The error message returned because the name of the source bucket is the same as that of the destination bucket.   * The error message returned because the specified bucket does not exist.   * The error message returned because the source bucket and destination bucket are owned by different users.             |
| InvalidTargetLocation         | 400 BadRequest   | The error message returned because the region of the destination bucket is different from the region specified in the XML body of the request.                                                                                                                                                                                                                                                                                                                               |
| BucketReplicationAlreadyExist | 400 BadRequest   | The error message returned because a data replication rule already exists between the source bucket and destination bucket.  To configure a data replication rule between the source bucket and destination bucket, delete the existing rule.                                                                                                                                                                                                                |
| BadReplicationLocation        | 400 BadRequest   | The error message returned because the region of the destination bucket is invalid.  You can call the GetBucketReplicationLocation operation to query valid regions in which the destination bucket can be located.                                                                                                                                                                                                                                          |
| NoReplicationLocation         | 400 BadRequest   | The error message returned because the region of the source bucket does not have a matching region for which CRR can be configured.  For more information about the matching regions for which CRR can be configured, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).                                                                                                                            |
| TooManyReplicationRules       | 400 BadRequest   | The error message returned because more than one data replication rule is configured in the request.  You can configure only one data replication rule in a single request.                                                                                                                                                                                                                                                                                  |
| TooManyIncomingReplication    | 400 BadRequest   | The error message returned because 100 data replication rules are configured for the bucket. Delete the rules that you no longer use and try again.  You can configure up to 100 data replication rules for a bucket. If your business requires more than 100 data replication rules for a bucket, [submit a ticket](https://workorder-intl.console.aliyun.com/?spm=5176.2020520001.nav-right.ditem-sub.433c12d26HHFbc#/ticket/createIndex). |
| TooManyOutgoingReplication    | 400 BadRequest   | The error message returned because 100 data replication rules are configured for the bucket. Delete the rules that you no longer use and try again.  You can configure up to 100 data replication rules for a bucket. If your business requires more than 100 data replication rules for a bucket, [submit a ticket](https://workorder-intl.console.aliyun.com/?spm=5176.2020520001.nav-right.ditem-sub.433c12d26HHFbc#/ticket/createIndex). |
| MissingArgument               | 400 BadRequest   | The error message returned because the transmission link is not specified.                                                                                                                                                                                                                                                                                                                                                                                                   |
| InvalidArgument               | 400 BadRequest   | The error message returned because the specified transmission link is not supported.                                                                                                                                                                                                                                                                                                                                                                                         |



