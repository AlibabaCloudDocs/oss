DeleteBucketReplication 
============================================

You can call this operation to disable cross-region replication (CRR) for a bucket and delete the CRR rule configured for the bucket. After the CRR rule is deleted, all operations performed in the source bucket are not synchronized to the destination bucket.

Usage notes 
--------------------------------

When you call the DeleteBucketReplication operation, take note of the following items:

* 200 OK is returned if no CRR rules are configured.

  

* DeleteBucketReplication does not immediately delete a CRR rule. OSS takes a period of time to clear the CRR task executed based on the CRR rule. During the process, the CRR task is in the closing state.After the CRR task is cleared, the CRR rule is deleted.

  

* If you call DeleteBucketReplication to delete a CRR rule that corresponds to a CRR task in the closing state, OSS returns 204 NoContent.

  




Request structure 
--------------------------------------

    POST /?replication&comp=delete HTTP/1.1 Host: BucketName.oss-cn-hangzhou.aliyuncs.com Date:GMT Date Content-Length: ContentLength Content-Type: application/xml Authorization: SignatureValue <?xml version="1.0" encoding="UTF-8"?> <ReplicationRules> <ID>rule id</ID> </ReplicationRules>



Request elements 
-------------------------------------



|     Element      |   Type    | Required |                                                                             Description                                                                             |
|------------------|-----------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ReplicationRules | Container | Yes      | The container that is used to store the CRR rule to delete. Parent nodes: none Child nodes: ID                                      |
| ID               | String    | Yes      | The ID of CRR rule to delete.You can call DeleteBucketReplication to obtain the ID. Parent nodes: ReplictionRules Child nodes: none |



Examples 
-----------------------------

* Sample requests

  




    POST /?replication&comp=delete HTTP/1.1 Host: oss-example.oss-cn-hangzhou.aliyuncs.com Date:Thu, 24 Sep 2015 15:39:18 GMT Content-Length: 46 Content-Type: application/xml Authorization: OSS qn6qrrqxo2oawuk53otf****:CTkuxpLAi4XZ+WwIfNm0Fmgb**** <?xml version="1.0" encoding="UTF-8"?> <ReplicationRules> <ID>test_replication_1</ID> </ReplicationRules> 



* Sample responses

  




    HTTP/1.1 200 OK x-oss-request-id: 534B371674E88A4D8906**** Date: Thu, 24 Sep 2015 15:39:18 GMT Connection: close Content-Length: 0 Server: AliyunOSS 



Error codes 
--------------------------------



|       Error code        | HTTP status code |                                                                                                                                                                                                                                                                                                                                      Description                                                                                                                                                                                                                                                                                                                                      |
|-------------------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| NoSuchBucket            | 404 NotFound     | The error message returned because the specified bucket does not exist.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| TooManyReplicationRules | 400 BadRequest   | The error message returned because more than one CRR rule is configured in the request. You can configure only one CRR rule in a single request.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| TransferAccAlreadyInUse | 409Conflict      | The error message returned because transfer acceleration is disabled for the destination bucket specified in the CRR rule. In this case, the following information about the source bucket and destination bucket is contained in the XML body of the response. <?xml version="1.0" encoding="UTF-8"?> <Error> <Code>TransferAccAlreadyInUse</Code> <Message>The transfer acceleration is aleady used by cross-region replication.</Message> <SourceBucket>srcBucket</SourceBucket> <DestinationBucket>destBucket</DestinationBucket> <RequestId>5F1E76142A535D373683****</RequestId> <HostId>oss-cn-hangzhou.aliyuncs.com</HostId> </Error>  |





