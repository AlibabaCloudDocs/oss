DeleteBucketReplication 
============================================

You can call this operation to disable data replication for a bucket and delete the data replication rule configured for the bucket. After the data replication rule is deleted, all operations performed in the source bucket are not synchronized to the destination bucket. 

Usage notes 
--------------------------------

* 200 OK is returned if no data replication rules are configured.

  

* DeleteBucketReplication does not immediately delete a data replication rule. OSS takes a period of time to clear the data replication task executed based on the rule. During the process, the data replication task is in the closing state. After the data replication task is cleared, the data replication rule is deleted.

  

* If you call DeleteBucketReplication to delete a data replication rule that corresponds to a data replication task in the closing state, 204 NoContent
  is returned.

  




Request structure 
--------------------------------------

    POST /?replication&comp=delete HTTP/1.1 Host: BucketName.oss-cn-hangzhou.aliyuncs.com 
    Date: GMT Date
    Content-Length: ContentLength
    Content-Type: application/xml 
    Authorization: SignatureValue
    
    <?xml version="1.0" encoding="UTF-8"?>
    <ReplicationRules>
       <ID>rule id</ID>
    </ReplicationRules>



Request elements 
-------------------------------------



|     Element      |   Type    | Required |                                                                                    Description                                                                                     |
|------------------|-----------|----------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ReplicationRules | Container | Yes      | The container that is used to store the data replication rule to delete.  Parent nodes: none Child nodes: ID                                       |
| ID               | String    | Yes      | The ID of data replication rule to delete. You can call DeleteBucketReplication to obtain the ID.  Parent nodes: ReplictionRules Child nodes: none |



Examples 
-----------------------------

* Sample requests

  




    POST /?replication&comp=delete HTTP/1.1 
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com 
    Date: Thu, 24 Sep 2015 15:39:18 GMT
    Content-Length: 46
    Content-Type: application/xml
    Authorization: OSS qn6qrrqxo2oawuk53otf****:CTkuxpLAi4XZ+WwIfNm0Fmgb****
    
    
    <?xml version="1.0" encoding="UTF-8"?>
    <ReplicationRules>
      <ID>test_replication_1</ID>
    </ReplicationRules>



* Sample responses

  




    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906**** 
    Date: Thu, 24 Sep 2015 15:39:18 GMT
    Connection: close 
    Content-Length: 0
    Server: AliyunOSS



Error codes 
--------------------------------



|       Error code        | HTTP status code |                                                                                                                                                                                                                                                                                                                                      Description                                                                                                                                                                                                                                                                                                                                      |
|-------------------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| NoSuchBucket            | 404 NotFound     | The error message returned because the specified bucket does not exist.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| TooManyReplicationRules | 400 BadRequest   | The error message returned because more than one data replication rule is configured in the request.  You can configure only one data replication rule in a single request.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| TransferAccAlreadyInUse | 409Conflict      | The error message returned because transfer acceleration is disabled for the destination bucket specified in the CRR rule. In this case, the following information about the source bucket and destination bucket is contained in the XML body of the response. <?xml version="1.0" encoding="UTF-8"?> <Error> <Code>TransferAccAlreadyInUse</Code> <Message>The transfer acceleration is aleady used by cross-region replication.</Message> <SourceBucket>srcBucket</SourceBucket> <DestinationBucket>destBucket</DestinationBucket> <RequestId>5F1E76142A535D373683****</RequestId> <HostId>oss-cn-hangzhou.aliyuncs.com</HostId> </Error>  |





