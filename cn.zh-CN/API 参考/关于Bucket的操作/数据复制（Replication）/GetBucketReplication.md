GetBucketReplication 
=========================================

GetBucketReplication接口用来获取某个存储空间（Bucket）已设置的数据复制规则。

请求语法 
-------------------------

    GET /?replication HTTP/1.1
    Host: BucketName.oss-cn-hangzhou.aliyuncs.com 
    Date: GMT Date
    Authorization: SignatureValue



响应元素 
-------------------------




|             名称              | 类型  |                                                                                                                                                                                                          描述                                                                                                                                                                                                          |
|-----------------------------|-----|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ReplicationConfiguration    | 容器  | Bucket复制规则的容器。 父节点：无 子节点：Rule                                                                                                                                                                                                                                                                                                                                                        |
| Rule                        | 容器  | 保存复制规则的容器。 父节点：ReplicationConfiguration 子节点：Destination、HistoricalObjectReplication、Status和ID                                                                                                                                                                                                                                                                                        |
| ID                          | 字符串 | 复制规则对应的ID。 父节点：Rule 子节点：无                                                                                                                                                                                                                                                                                                                                                            |
| PrefixSet                   | 容器  | 保存前缀（Prefix）的容器。每条复制规则中，最多可指定10个Prefix。 父节点：Rule 子节点：Prefix                                                                                                                                                                                                                                                                                                                          |
| Prefix                      | 字符串 | 被复制到目标Bucket中Object的前缀（Prefix）。 父节点：PrefixSet 子节点：无                                                                                                                                                                                                                                                                                                                                  |
| Action                      | 字符串 | 表示被同步到目标Bucket的操作。 Action允许以下操作类型，您可以指定一项或多项。 * ALL（默认值）：表示PUT、DELETE、ABORT操作均会被同步到目标Bucket。   * PUT：表示被同步到目标Bucket的写入操作，包括PutObject、PostObject、AppendObject、CopyObject、PutObjectACL、InitiateMultipartUpload、UploadPart、UploadPartCopy、CompleteMultipartUpload。    父节点：Rule 子节点：无 |
| Status                      | 字符串 | 表示复制状态。 取值： * starting：设置数据复制规则后，OSS会为Bucket准备复制任务，此时的复制状态为starting。   * doing：当数据复制规则生效后，即数据处于同步状态时，此时的复制状态为doing。   * closing：删除数据复制规则后，OSS会自动完成清理工作，此时的复制状态为closing。    父节点：Rule 子节点：无                                                        |
| Destination                 | 容器  | 保存目标Bucket信息的容器。 父节点：Rule 子节点：Bucket和Location                                                                                                                                                                                                                                                                                                                                        |
| Bucket                      | 字符串 | 数据要复制到的目标Bucket。 父节点：Destination 子节点：无                                                                                                                                                                                                                                                                                                                                               |
| Location                    | 字符串 | 目标Bucket所处的Location。 父节点：Destination 子节点：无                                                                                                                                                                                                                                                                                                                                           |
| TransferType                | 字符串 | 数据复制时使用的数据传输类型。 取值： * internal（默认值）：OSS默认传输链路。   * oss_acc：传输加速链路。只有创建跨区域复制规则时才能使用传输加速链路。                                                                                                                                                                                                         |
| HistoricalObjectReplication | 字符串 | 是否复制历史数据。即开启数据复制前，是否将源Bucket中的已有的数据复制到目标Bucket。 取值： * enabled（默认）：表示复制历史数据。   * disabled：表示不复制历史数据，即仅复制数据复制规则生效后新写入的数据。    父节点：Rule 子节点：无                                                                                                                                         |





示例 
-----------------------

* 请求示例

  




    GET /?replication HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com 
    Date: Thu, 24 Sep 2015 15:39:15 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:CTkuxpLAi4XZ+WwIfNm0Fmgb****



* 返回示例

  **说明**

  仅当传输类型为oss_acc时，返回的XML示例中才会包含\<TransferType\>元素。
  




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



错误码 
------------------------



|              错误码               |     状态码      |          说明          |
|--------------------------------|--------------|----------------------|
| NoSuchBucket                   | 404 NotFound | 请求的Bucket不存在。        |
| NoSuchReplicationConfiguration | 404 NotFound | 请求的Bucket没有配置数据复制规则。 |





