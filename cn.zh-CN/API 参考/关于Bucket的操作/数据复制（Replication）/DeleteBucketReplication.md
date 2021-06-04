DeleteBucketReplication 
============================================

DeleteBucketReplication接口用来停止某个存储空间（Bucket）的数据复制并删除Bucket的复制配置，此时源Bucket中的任何操作都不会被同步到目标Bucket。

注意事项 
-------------------------

* 当请求的Bucket没有配置数据复制规则时，调用此接口将返回200 HTTP OK。

  

* 调用此接口删除某个数据复制规则时，该复制规则不会立刻被删除。OSS需要一定的时间来执行清理操作，此时复制规则的状态为closing。当清理工作完成后，该复制规则才被删除。

  

* 当请求的Bucket的数据复制规则处于closing状态时，调用此接口将返回204
  NoContent。

  




请求语法 
-------------------------

    POST /?replication&comp=delete HTTP/1.1 Host: BucketName.oss-cn-hangzhou.aliyuncs.com 
    Date: GMT Date
    Content-Length：ContentLength
    Content-Type: application/xml 
    Authorization: SignatureValue
    
    <?xml version="1.0" encoding="UTF-8"?>
    <ReplicationRules>
       <ID>rule id</ID>
    </ReplicationRules>



请求元素 
-------------------------



|        名称        | 类型  | 是否必选 |                                                    描述                                                    |
|------------------|-----|------|----------------------------------------------------------------------------------------------------------|
| ReplicationRules | 容器  | 是    | 保存需要删除的数据复制规则的容器。 父节点：无 子节点：ID                                           |
| ID               | 字符串 | 是    | 需要删除的复制规则对应的ID。规则ID可从GetBucketReplication中获取。 父节点：ReplicationRules 子节点：无 |



示例 
-----------------------

* 请求示例

  




    POST /?replication&comp=delete HTTP/1.1 
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com 
    Date: Thu, 24 Sep 2015 15:39:18 GMT
    Content-Length：46
    Content-Type: application/xml
    Authorization: OSS qn6qrrqxo2oawuk53otf****:CTkuxpLAi4XZ+WwIfNm0Fmgb****
    
    
    <?xml version="1.0" encoding="UTF-8"?>
    <ReplicationRules>
      <ID>test_replication_1</ID>
    </ReplicationRules>



* 返回示例

  




    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906**** 
    Date: Thu, 24 Sep 2015 15:39:18 GMT
    Connection: close 
    Content-Length: 0
    Server: AliyunOSS



错误码 
------------------------



|           错误码           |      状态码       |                                                                                                                                                                                                                                          描述                                                                                                                                                                                                                                          |
|-------------------------|----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| NoSuchBucket            | 404 NotFound   | 请求的Bucket不存在。                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| TooManyReplicationRules | 400 BadRequest | 请求中配置的数据复制规则超过一条。 单次请求中只能配置一条数据复制规则。                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| TransferAccAlreadyInUse | 409Conflict    | 对跨区域复制指定的目标Bucket关闭了传输加速，此时错误XML中返回跨区域复制的源Bucket和目标Bucket信息如下： <?xml version="1.0" encoding="UTF-8"?> <Error> <Code>TransferAccAlreadyInUse</Code> <Message>The transfer acceleration is aleady used by cross-region replication.</Message> <SourceBucket>srcBucket</SourceBucket> <DestinationBucket>destBucket</DestinationBucket> <RequestId>5F1E76142A535D373683****</RequestId> <HostId>oss-cn-hangzhou.aliyuncs.com</HostId> </Error>  |





