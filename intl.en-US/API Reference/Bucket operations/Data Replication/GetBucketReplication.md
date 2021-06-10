GetBucketReplication 
=========================================

You can call this operation to query the cross-region replication (CRR) rule configured for a bucket.

Request syntax 
-----------------------------------

    GET /? replication HTTP/1.1
    Host: BucketName.oss-cn-hangzhou.aliyuncs.com 
    Date: GMT Date
    Authorization: SignatureValue



Response elements 
--------------------------------------




|           Element           |   Type    |                                                                                                                                                                                                                                                                                                                                          Description                                                                                                                                                                                                                                                                                                                                           |
|-----------------------------|-----------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ReplicationConfiguration    | Container | The container that stores CRR rules. Parent node: none Chile node: Rule                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Rule                        | Container | The container that stores the CRR rule. Parent node: ReplicationConfiguration Child nodes: Destination, HistoricalObjectReplication, Status, and ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ID                          | String    | The ID of the CRR rule. Parent node: Rule Child node: none                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| PrefixSet                   | Container | The container that stores prefixes. You can specify up to 10 prefixes in each CRR rule. Parent node: Rule Child node: Prefix                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Prefix                      | String    | The prefix used to specify the object to replicate. Parent node: PrefixSet Child node: none                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| Action                      | String    | The operations that can be synchronized to the destination bucket. You can set Action to one or more of the following operation types. Valid values: * All: PUT, DELETE, and ABORT operations are synchronized to the destination bucket. This is the default value.   * PUT: write operations are synchronized to the destination bucket, including PutObject, PostObject, AppendObject, CopyObject, PutObjectACL, InitiateMultipartUpload, UploadPart, UploadPartCopy, and CompleteMultipartUpload.    Parent node: Rule Child node: none |
| Status                      | String    | The status of the CRR task. Valid values: * starting: OSS creates a CRR task after a CRR rule is configured. In this case, the state of the CRR task is starting.   * doing: the state of a CRR task is doing when the CRR rule takes effect and the data is being replicated.   * closing: OSS clears a CRR task after the corresponding CRR rule is deleted. In this case, the state of the CRR task is closing.    Parent node: Rule Child node: none                                                                   |
| Destination                 | Container | The container that stores the information about the destination bucket. Parent node: Rule Child nodes: Bucket and Location                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Bucket                      | String    | The destination bucket to which the data is replicated. Parent node: Destination Child node: none                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| Location                    | String    | The region in which the destination bucket is located. Parent node: Destination Child node: none                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| TransferType                | String    | The link used to transfer data in CRR. Valid values: * internal: the default link.   * oss_acc: the link in which data transmission is accelerated.                                                                                                                                                                                                                                                                                                                                                                                                                         |
| HistoricalObjectReplication | String    | Indicates whether historical data is replicated. Historical data indicates the data stored in the source bucket before CRR is enabled for the source bucket. Valid values: * Enabled: indicates that historical data is replicated to the destination bucket. This is the default value.   * Disabled: indicates that historical data is not replicated to the destination bucket. Only data uploaded to the source bucket after CRR is enabled for the source bucket is replicated.    Parent node: Rule Child node: none                                  |





Examples 
-----------------------------

* Sample request

  




    GET /? replication HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com 
    Date: Thu, 24 Sep 2015 15:39:15 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:CTkuxpLAi4XZ+WwIfNm0Fmgb****



* Sample response

  **Note**

  The TransferType element is contained in the XML body of the response only when the value of TransferType is set to oss_acc in the request.
  




    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906**** 
    Date: Thu, 24 Sep 2015 15:39:15 GMT
    Content-Length: 186
    Content-Type: application/xml 
    Connection: close
    Server: AliyunOSS
    
    
    <? xml version="1.0" ? >
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



|           Error code           | HTTP status code |                                          Limit                                           |
|--------------------------------|------------------|------------------------------------------------------------------------------------------|
| NoSuchBucket                   | 404 NotFound     | The error message returned because the specified bucket does not exist.                  |
| NoSuchReplicationConfiguration | 404 NotFound     | The error message returned because no CRR rules are configured for the specified bucket. |





