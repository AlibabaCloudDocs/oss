PutBucketReplication 
=========================================

PutBucketReplication接口用于为存储空间（Bucket）指定跨区域复制规则。

注意事项 
-------------------------

跨区域复制（Cross-Region Replication）是跨不同OSS数据中心（地域）的存储空间（Bucket）自动、异步（近实时）复制文件（Object），它会将Object的创建、更新和删除等操作从源Bucket复制到不同区域的目标Bucket。使用跨区域复制时，有如下注意事项：

* 跨区域复制采用异步复制，数据复制到目标Bucket需要一定的时间，通常几分钟到几小时不等，具体取决于数据的大小。

  

* 源Bucket与目标Bucket必须处于不同的数据中心，且名称不能相同。

  有关跨区域复制的更多信息，请参见[跨区域复制介绍](/intl.zh-CN/开发指南/数据安全/数据容灾/跨区域复制介绍.md)。
  






请求语法 
-------------------------

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



请求元素 
-------------------------



|             名称              | 类型  | 是否必须 |                                                                                                                                                                                                                                           描述                                                                                                                                                                                                                                            |
|-----------------------------|-----|------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ReplicationConfiguration    | 容器  | 是    | 配置Bucket复制规则的容器。 父节点：无 子节点：Rule                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Rule                        | 容器  | 是    | 保存复制规则的容器，目前仅允许配置一条复制规则。 父节点：ReplicationConfiguration 子节点：Destination、HistoricalObjectReplication和ID                                                                                                                                                                                                                                                                                                                                                    |
| ID                          | 字符串 | 否    | 复制规则对应的ID。 此ID最大长度为255个字符，且在单个Bucket中全局唯一。 如果没有指定ID或者ID值为空时，OSS会为该复制规则生成唯一值。 父节点：Rule 子节点：无                                                                                                                                                                                                                                                                                                                             |
| PrefixSet                   | 容器  | 否    | 保存前缀（Prefix）的容器。每条复制规则中，最多可指定10条Prefix。 父节点：Rule 子节点：Prefix                                                                                                                                                                                                                                                                                                                                                                                             |
| Prefix                      | 字符串 | 否    | 设置待复制Object的Prefix。只有匹配该Prefix的Object才被复制到目标Bucket。 * Prefix最大长度为1023个字符。   * 如果配置了Prefix，则新写入的数据和历史数据的同步都会遵循Prefix指定的规则。    父节点：PrefixSet 子节点：无                                                                                                                                                                                                     |
| Action                      | 字符串 | 否    | 指定可以被复制到目标Bucket的操作。如果配置了Action，则新写入的数据和历史数据的同步都会遵循Action指定的复制操作。 Action允许以下操作类型，您可以指定一项或多项。 取值： * ALL（默认值）：表示PUT、DELETE、ABORT操作均会被同步到目标Bucket。   * PUT：表示被同步到目标Bucket的写入操作，包括PutObject、PostObject、AppendObject、CopyObject、PutObjectACL、InitiateMultipartUpload、UploadPart、UploadPartCopy、CompleteMultipartUpload。    父节点：Rule 子节点：无 |
| Destination                 | 容器  | 是    | 保存目标Bucket信息的容器。 父节点：Rule 子节点：Bucket和Location                                                                                                                                                                                                                                                                                                                                                                                                           |
| Bucket                      | 字符串 | 是    | 指定数据要复制到的目标Bucket。 父节点：Destination 子节点：无                                                                                                                                                                                                                                                                                                                                                                                                                |
| Location                    | 字符串 | 是    | 目标Bucket所处的地域。 父节点：Destination 子节点：无                                                                                                                                                                                                                                                                                                                                                                                                                    |
| TransferType                | 字符串 | 是    | 指定跨区域复制时使用的数据传输链路。 取值： * internal（默认值）：OSS默认传输链路。   * oss_acc：传输加速链路。 **说明** 如果创建跨区域复制规则时指定使用oss_acc的传输链路，并且OSS在校验跨区域复制规则创建成功后，会检查用户是否对目标Bucket开启OSS传输加速。若未开启，OSS将自动开启OSS传输加速。    父节点：Destination 子节点：无                                                                                                                             |
| HistoricalObjectReplication | 字符串 | 否    | 指定是否复制历史数据。即开启跨区域复制前，是否将源Bucket中的数据复制到目标Bucket。 取值： * Enabled（默认值）：表示复制历史数据   * Disabled：表示不复制历史数据。即仅复制跨区域复制规则生效后新写入的数据。    父节点：Rule 子节点：无                                                                                                                                                                                                           |
| SyncRole                    | 字符串 | 否    | 授权OSS使用哪个角色来进行数据复制。如果指定使用SSE-KMS加密目标对象，则必须指定SyncRole。                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| SourceSelectionCriteria     | 容器  | 否    | 用于标识要复制的源对象的其他筛选条件的容器。当前OSS仅支持针对SSE-KMS加密的源对象指定筛选条件。                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| SseKmsEncryptedObjects      | 容器  | 否    | 用于筛选使用SSE-KMS加密对象的容器。如果在复制规则中指定了SourceSelectionCriteria，则必须指定该元素。                                                                                                                                                                                                                                                                                                                                                                                                                       |
| Status                      | 字符串 | 否    | 指定OSS是否复制通过SSE-KMS加密创建的对象。                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| EncryptionConfiguration     | 容器  | 否    | 目标对象加密配置。                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ReplicaKmsKeyID             | 字符串 | 否    | 指定SSE-KMS密钥ID。                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |



示例 
-----------------------

* 请求示例

  




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



* 返回示例

  




    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Thu, 24 Sep 2015 15:39:12 GMT
    Content-Length: 0
    Connection: close
    Server: AliyunOSS



错误码 
------------------------



|                   错误码                    |      状态码       |                                                                                               描述                                                                                                |
|------------------------------------------|----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| InvalidTargetBucket                      | 400 BadRequest | * 目标Bucket名称与源Bucket名称相同。   * 目标Bucket不存在。   * 目标Bucket与源Bucket不属于同一个用户。    |
| InvalidTargetLocation                    | 400 BadRequest | 目标Bucket所在的Location不是请求XML中指定的Location。                                                                                                                                                         |
| BucketAlreadyHasReplicationConfiguration | 400 BadRequest | 一个Bucket仅能与另一个Bucket存在跨区域复制关系，且允许双向配置跨区域复制。 若要开启新的跨区域复制配置，请先删除已有的跨区域复制配置。                                                                                                       |
| BucketReplicationAlreadyExist            | 400 BadRequest | 在已开启一个Bucket另一个Bucket的跨区域复制情况下，再重复开启跨区域复制时返回此错误。                                                                                                                                                |
| BadReplicationLocation                   | 400 BadRequest | 选择的目的数据中心不合法。 您可通过GetBucketReplicationLocation来获得合法的可复制到的数据中心。                                                                                                                  |
| NoReplicationLocation                    | 400 BadRequest | 源Bucket所在的数据中心没有与之配对的可实施跨区域复制的数据中心。 跨区域复制数据中心的配对关系可通过[访问域名和数据中心](/intl.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md)查看。                                               |
| TooManyReplicationRules                  | 400 BadRequest | 目前OSS只允许为一个Bucket配置一条跨区域复制规则。                                                                                                                                                                   |
| MissingArgument                          | 400 BadRequest | 未指定数据传输链路。                                                                                                                                                                                      |
| InvalidArgument                          | 400 BadRequest | 不支持指定的数据传输链路。                                                                                                                                                                                   |



