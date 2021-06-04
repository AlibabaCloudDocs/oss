GetBucketReplicationLocation 
=================================================

GetBucketReplicationLocation接口用于获取可复制到的目标存储空间（Bucket）所在的地域。您可以根据返回结果决定将源Bucket的数据复制到哪个地域。

请求语法 
-------------------------

    GET /?replicationLocation HTTP/1.1
    Host: BucketName.oss-cn-hangzhou.aliyuncs.com 
    Date: GMT Date
    Authorization: SignatureValue



响应元素 
-------------------------



|               名称               | 类型  |                                                                                                       描述                                                                                                        |
|--------------------------------|-----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ReplicationLocation            | 容器  | 可复制地域的容器。                                                                                                                                                                                                       |
| Location                       | 字符串 | 可复制到的目标Bucket所在的地域，例如oss-cn-beijing。 父节点：ReplicationLocation 子节点：无 **说明** 如果有多个可复制到的目标地域，那么返回的结果中包含多个Location。如果没有可复制到的目标地域，则返回的Location为空。                     |
| LocationTransferTypeConstraint | 容器  | 包含TransferType约束的Location信息容器。                                                                                                                                                                                  |
| LocationTransferType           | 容器  | 包含TransferType的Location信息容器。                                                                                                                                                                                    |
| TransferTypes                  | 容器  | 传输类型容器。                                                                                                                                                                                                         |
| Type                           | 字符串 | 数据复制时使用的数据传输类型。 取值： * internal（默认值）：OSS默认传输链路。   * oss_acc：传输加速链路。只有创建跨区域复制规则时才能使用传输加速链路。    |



示例 
-----------------------

* 请求示例

  




    GET /?replicationLocation HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com 
    Date: Thu, 24 Sep 2015 15:39:15 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:CTkuxpLAi4XZ+WwIfNm0Fmgb****



* 返回示例

  **说明**

  仅当传输类型为oss_acc时，返回的XML示例中才会包含\<TransferType\>元素。
  




    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Thu, 24 Sep 2015 15:39:15 GMT
    Content-Length: 84
    Content-Type: application/xml Connection: close
    Server: AliyunOSS
    
    <?xml version="1.0" ?>
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



错误码 
------------------------



|     错误码      |     状态码      |      描述       |
|--------------|--------------|---------------|
| NoSuchBucket | 404 NotFound | 请求的Bucket不存在。 |





