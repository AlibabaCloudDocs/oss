# GetObjectACL {#reference_lzc_24w_wdb .reference}

GetObjectACL用来获取某个Bucket下的某个Object的访问权限。

## 请求语法 {#section_pzf_h4w_wdb .section}

```
GET /ObjectName?acl HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 响应元素\(Response Elements\) {#section_qrd_j4w_wdb .section}

|名称|类型|描述|
|:-|:-|:-|
|AccessControlList|容器|存储ACL信息的容器父节点：AccessControlPolicy

|
|AccessControlPolicy|容器|保存Get Object ACL结果的容器 父节点：None

|
|DisplayName|字符串|Bucket拥有者的名称.。\(目前和ID一致\)父节点：AccessControlPolicy.Owner

|
|Grant|枚举字符串|Object的ACL权限有效值：private，public-read，public-read-write

父节点：AccessControlPolicy.AccessControlList

|
|ID|字符串|Bucket拥有者的用户ID 父节点：AccessControlPolicy.Owner

|
|Owner|容器|保存Bucket拥有者信息的容器。父节点：AccessControlPolicy

|

## 细节分析 {#section_rgf_k4w_wdb .section}

-   只有Bucket的拥有者才能使用GetObjectACL这个接口来获取该Bucket下某个Object的ACL，非Bucket Owner调用该接口时，返回403 Forbidden消息。错误码：AccessDenied，提示You do not have read acl permission on this object。
-   如果从来没有对某个Object设置过ACL，则调用GetObjectACL时，OSS返回的ObjectACL会是default，表明该Object ACL遵循Bucket ACL。即：如果Bucket是private的，则该object也是private的；如果该object是public-read-write的，则该object也是public-read-write的。

## 示例 {#section_swy_3pw_wdb .section}

**请求示例：**

```
GET /test-object?acl HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Wed, 29 Apr 2015 05:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:CTkuxpLAi4XZ+WwIfNm0FmgbrQ0=
```

**返回示例：**

```
HTTP/1.1 200 OK
x-oss-request-id: 559CC9BDC755F95A64485981
Date: Wed, 29 Apr 2015 05:21:12 GMT
Content-Length: 253
Content-Type: application/xml
Connection: keep-alive
Server: AliyunOSS

<?xml version="1.0" ?>
<AccessControlPolicy>
    <Owner>
        <ID>00220120222</ID>
        <DisplayName>00220120222</DisplayName>
    </Owner>
    <AccessControlList>
        <Grant>public-read </Grant>
    </AccessControlList>
</AccessControlPolicy>
```

