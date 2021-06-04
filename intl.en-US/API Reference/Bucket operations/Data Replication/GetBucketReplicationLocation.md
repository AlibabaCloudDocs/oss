GetBucketReplicationLocation 
=================================================

You can call this operation to query the region in which the destination bucket can be located. You can determine the region of the destination bucket based on the response returned for the operation.

Request syntax 
-----------------------------------

    GET /? replicationLocation HTTP/1.1
    Host: BucketName.oss-cn-hangzhou.aliyuncs.com 
    Date: GMT Date
    Authorization: SignatureValue



Response elements 
--------------------------------------



|            Element             |   Type    |                                                                                                                                                                                                     Description                                                                                                                                                                                                     |
|--------------------------------|-----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ReplicationLocation            | Container | The container that stores the region in which the destination bucket can be located.                                                                                                                                                                                                                                                                                                                                |
| Location                       | String    | The region in which the destination bucket can be located. Example: oss-cn-beijing. Parent node: ReplicationLocation Child node: none **Note** If the destination bucket can be located in multiple regions, multiple regions are contained in the response. If regions in which the destination bucket can be located do not exist, the value of Location is null. |
| LocationTransferTypeConstraint | Container | The container that stores regions in which the destination bucket can be located with TransferType specified.                                                                                                                                                                                                                                                                                                       |
| LocationTransferType           | Container | The container that stores regions in which the destination bucket can be located with the TransferType information.                                                                                                                                                                                                                                                                                                 |
| TransferTypes                  | Container | The container that stores the transmission type.                                                                                                                                                                                                                                                                                                                                                                    |
| Type                           | String    | The link used to transfer data in CRR. Valid values: * internal: the default link.   * oss_acc: the link in which data transmission is accelerated.                                                                                                                                              |



Examples 
-----------------------------

* Sample request

  




    GET /? replicationLocation HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com 
    Date: Thu, 24 Sep 2015 15:39:15 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:CTkuxpLAi4XZ+WwIfNm0Fmgb****



* Sample response

  




    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Thu, 24 Sep 2015 15:39:15 GMT
    Content-Length: 84
    Content-Type: application/xml Connection: close
    Server: AliyunOSS
    
    <? xml version="1.0" ? >
    <ReplicationLocation>
      <Location>oss-cn-beijing</Location>
      <Location>oss-cn-qingdao</Location>
      <Location>oss-cn-shenzhen</Location>
      <Location>oss-cn-hongkong</Location>
      <Location>oss-us-west-1</Location>
      <LocationTransferTypeConstraint>
        <LocationTransferType>
          <Location>oss-cn-hongkong</Location>
            <TransferTypes>
              <Type>oss_acc</Type>          
            </TransferTypes>
          </LocationTransferType>
          <LocationTransferType>
            <Location>oss-us-west-1</Location>
            <TransferTypes>
              <Type>oss_acc</Type>
            </TransferTypes>
          </LocationTransferType>
        </LocationTransferTypeConstraint>
      </ReplicationLocation>



Error codes 
--------------------------------



|  Error code  | HTTP status code |                               Description                               |
|--------------|------------------|-------------------------------------------------------------------------|
| NoSuchBucket | 404 NotFound     | The error message returned because the specified bucket does not exist. |





