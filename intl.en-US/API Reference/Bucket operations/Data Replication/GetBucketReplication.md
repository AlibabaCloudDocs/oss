GetBucketReplication 
=========================================

You can call this operation to query the data replication rule configured for a bucket. 

Request structure 
--------------------------------------

    GET /?replication HTTP/1.1
    Host: BucketName.oss-cn-hangzhou.aliyuncs.com 
    Date: GMT Date
    Authorization: SignatureValue



Response elements 
--------------------------------------




|           Element           |   Type    |                                                                                                                                                                                                                                                                                                                                                    Description                                                                                                                                                                                                                                                                                                                                                     |
|-----------------------------|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ReplicationConfiguration    | Container | The container that stores data replication rules.  Parent nodes: none Child nodes: Rule                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| Rule                        | Container | The container that stores the data replication rule.  Parent nodes: ReplicationConfiguration Child nodes: Destination, HistoricalObjectReplication, Status, and ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ID                          | String    | The ID of the data replication rule.  Parent nodes: Rule Child nodes: none                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| PrefixSet                   | Container | The container that is used to store prefixes. You can specify up to 10 prefixes in each data replication rule.  Parent nodes: Rule Child nodes: Prefix                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| Prefix                      | String    | The prefix used to specify the object to replicate.  Parent nodes: PrefixSet Child nodes: none                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Action                      | String    | The operations that can be synchronized to the destination bucket.  You can set Action to one or more of the following operation types. Default value: ALL.  * ALL: PUT, DELETE, and ABORT operations are synchronized to the destination bucket.   * PUT: write operations are synchronized to the destination bucket, including PutObject, PostObject, AppendObject, CopyObject, PutObjectACL, InitiateMultipartUpload, UploadPart, UploadPartCopy, and CompleteMultipartUpload.    Parent nodes: Rule Child nodes: none                                                      |
| Status                      | String    | The status of the data replication task.  Valid values: * starting: OSS creates a data replication task after a data replication rule is configured. In this case, the state of the task is starting.   * doing: the state of a data replication task is doing when the data replication rule takes effect and the data is being replicated.   * closing: OSS clears a data replication task after the corresponding data replication rule is deleted. In this case, the state of the task is closing.    Parent nodes: Rule Child nodes: none |
| Destination                 | Container | The container that is used to store the information about the destination bucket.  Parent nodes: Rule Child nodes: Bucket and Location                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| Bucket                      | String    | The destination bucket to which the data is replicated.  Parent nodes: Destination Child nodes: none                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Location                    | String    | The region in which the destination bucket is located.  Parent nodes: Destination Child nodes: none                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| TransferType                | String    | The link used to transfer data in data replication. Default value: internal.  Valid values: * internal: the default link.   * oss_acc: the link in which data transmission is accelerated. TransferType can be set to oss_acc only for cross-region replication (CRR) rules.                                                                                                                                                                                                                                                                                                                    |
| HistoricalObjectReplication | String    | Indicates whether historical data from the source bucket is replicated to the destination bucket before data replication is enabled. Default value: enabled.  Valid values: * enabled: indicates that historical data is replicated to the destination bucket.   * disabled: indicates that historical data is not replicated to the destination bucket. Only data uploaded to the source bucket after data replication is enabled for the source bucket is replicated.    Parent nodes: Rule Child nodes: none                                                                 |





Examples 
-----------------------------

* Sample requests

  




    GET /?replication HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com 
    Date: Thu, 24 Sep 2015 15:39:15 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:CTkuxpLAi4XZ+WwIfNm0Fmgb****



* Sample responses

  **Note**

  The TransferType element is contained in the XML body of the response only when the value of TransferType is set to oss_acc in the request.
  




    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906**** 
    Date: Thu, 24 Sep 2015 15:39:15 GMT
    Content-Length: 186
    Content-Type: application/xml 
    Connection: close
    Server: AliyunOSS
    
    
    <?xml version="1.0" ?>
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
        <Status>doing</Status>
        <HistoricalObjectReplication>enabled</HistoricalObjectReplication>
        <SyncRole>acs:ram::174649585760****:role/aliyunramrole</SyncRole>
      </Rule>
    </ReplicationConfiguration>



Error codes 
--------------------------------



|           Error code           | HTTP status code |                                              Description                                              |
|--------------------------------|------------------|-------------------------------------------------------------------------------------------------------|
| NoSuchBucket                   | 404 NotFound     | The error message returned because the specified bucket does not exist.                               |
| NoSuchReplicationConfiguration | 404 NotFound     | The error message returned because no data replication rules are configured for the specified bucket. |





