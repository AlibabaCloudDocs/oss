# GetBucketReplicationProgress

GetBucketReplicationProgress用于获取某个存储空间（Bucket）的跨区域复制进度。

## 请求语法

```
GET /?replicationProgress&rule-id=RuleId HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 请求参数

|名称|类型|是否必须|描述|
|--|--|----|--|
|rule-id|字符串|否|复制规则对应的ID。此ID可从GetBucketReplication中获取。|

## 响应元素

|名称|类型|描述|
|--|--|--|
|ReplicationProgress|容器|保存复制进度的容器。父节点：无

子节点：Rule |
|Rule|容器|保存每个复制规则进度条目的容器。父节点：ReplicationConfiguration

子节点：ID、Destination、Status和Progress |
|ID|字符串|复制规则对应的ID。父节点：Rule

子节点：无 |
|PrefixSet|容器|保存Prefix 的容器，每个复制规则中，最多能指定10个Prefix。父节点：Rule

子节点：Prefix |
|Prefix|字符串|待复制Object的Prefix，只有匹配该Prefix的Object才会被复制到目标Bucket。父节点：PrefixSet

子节点：无 |
|Action|字符串|表示被同步到目标Bucket的操作。Action允许以下操作类型，您可以指定一项或者多项。

-   ALL（默认操作）：表示PUT、DELETE、ABORT操作均会被同步到目标Bucket。
-   PUT：表示被同步到目标Bucket的写入操作，包括PutObject、PostObject、AppendObject、CopyObject、PutObjectACL、 InitiateMultipartUpload 、 UploadPart、UploadPartCopy和CompleteMultipartUpload。

父节点：Rule

子节点：无 |
|Destination|容器|保存目标Bucket信息的容器。父节点：Rule

子节点：Bucket和Location |
|Bucket|字符串|数据被复制到的目标Bucket。父节点：Destination

子节点：无 |
|Location|字符串|目标Bucket所在的Location。父节点：Destination

子节点：无 |
|TransferType|字符串|跨区域复制时使用的数据传输类型。取值：

-   internal（默认值）：OSS默认传输链路
-   oss\_acc：传输加速链路 |
|HistoricalObjectReplication|字符串|是否复制历史数据。即开启跨区域复制前，是否将源Bucket中的数据复制到目标Bucket。取值：

-   Enabled（默认值）：表示复制历史数据。
-   Disabled：表示不复制历史数据，仅复制跨区域复制规则生效后新写入的数据。 |
|Progress|容器|保存复制进度的容器，仅当数据处于同步状态（doing）时才返回此元素。父节点：Rule

子节点：HistoricalObject、NewObject |
|HistoricalObject|字符串|显示已复制历史数据的百分比。仅对开启了历史数据复制的Bucket有效。父节点：Progress

子节点：无 |
|NewObject|字符串|显示数据复制到目标Bucket的时间点（GMT格式）。例如Thu, 24 Sep 2015 15:39:18 GMT，表示早于这个时间点写入的数据都已复制到目标Bucket。

父节点：Progress

子节点：无 |

## 示例

-   请求示例

    ```
    GET /?replicationProgress&rule-id=test_replication_1 HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Thu, 24 Sep 2015 15:39:15 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:CTkuxpLAi4XZ+WwIfNm0Fmgb****
    ```

-   返回示例

    **说明：** 仅当传输类型为oss\_acc时，返回的XML消息体中才会包含<TransferType\>元素。

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Thu, 24 Sep 2015 15:39:15 GMT
    Content-Length: 234
    Content-Type: application/xml
    Connection: close
    Server: AliyunOSS
    
    
    <?xml version="1.0" ?>
    <ReplicationProgress>
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
       <Progress>
        <HistoricalObject>0.85</HistoricalObject>
        <NewObject>2015-09-24T15:28:14.000Z </NewObject>
       </Progress>
     </Rule>
    </ReplicationProgress>
    ```


## 错误码

|错误码|HTTP状态码|描述|
|---|-------|--|
|NoSuchBucket|404 NotFound|请求的Bucket不存在。|
|NoSuchReplicationRule|404 NotFound|指定的RuleId不存在。|
|NoSuchReplicationConfiguration|404 NotFound|请求的Bucket没有配置跨区域复制。|
|TooManyReplicationRules|400 BadRequest|请求中配置的跨区域复制规则超过一条。单次请求中只能配置一条跨区域复制规则。 |

