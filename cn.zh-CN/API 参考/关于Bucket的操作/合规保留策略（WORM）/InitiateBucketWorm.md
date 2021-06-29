# InitiateBucketWorm

调用InitiateBucketWorm接口新建一条合规保留策略。

## 注意事项

对象存储OSS支持WORM（Write Once Read Many）特性，允许以不可删除、不可篡改的方式保存和使用数据。OSS允许针对存储空间（Bucket）设置基于时间的合规保留策略，保护周期为1天到70年。

-   当基于时间的合规保留策略创建24小时后未提交锁定，则该策略自动失效。当合规保留策略锁定后，您可以在Bucket中上传和读取文件（Object），但是在Object的保留时间到期之前，不允许删除Object及合规保留策略。Object的保留时间到期后，才可以删除Object。关于合规保留策略的更多信息，请参见[合规保留策略](/cn.zh-CN/开发指南/数据安全/合规保留策略.md)。
-   同一个Bucket中，版本控制和合规保留策略无法同时配置。如果Bucket已开启版本控制功能，则无法再配置保留策略。关于版本控制功能更多信息，请参见[版本控制介绍](/cn.zh-CN/开发指南/数据安全/版本控制/版本控制介绍.md)。

## 请求元素

|名称|类型|是否必须|描述|
|--|--|----|--|
|InitiateWormConfiguration|容器|是|根节点 子节点：RetentionPeriodInDays |
|RetentionPeriodInDays|正整数|是|指定Object保留天数。|

## 示例

-   请求示例

    ```
    POST /?worm HTTP/1.1
    Date: Thu, 15 May 2014 11:18:32 GMT
    Content-Length：556
    Content-Type: application/xml
    Host: BucketName.oss.aliyuncs.com
    Authorization: OSS nxj7dtlhcyl5hp****:COS3OQkfQPnKmYZTEHYv2****
    
    <InitiateWormConfiguration>
      <RetentionPeriodInDays>365</RetentionPeriodInDays>
    </InitiateWormConfiguration>
    ```

-   返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 5374A2880232A65C2300****
    x-oss-worm-id: 1666E2CFB2B3418****
    Date: Thu, 15 May 2014 11:18:32 GMT
    ```


