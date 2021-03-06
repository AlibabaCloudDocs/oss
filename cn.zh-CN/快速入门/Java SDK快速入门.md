# Java SDK快速入门

本文介绍如何快速使用OSS Java SDK完成常见操作，如创建存储空间（Bucket）、上传文件（Object）、下载文件等。

已安装Java SDK。详情请参见[安装](/cn.zh-CN/SDK 示例/Java/安装.md)。

## 示例工程

OSS Java SDK提供了基于Maven和Ant的示例工程。您可以在本地设备上编译和运行示例工程，或者以示例工程为基础开发您的应用。工程的编译和运行方法，请参见工程目录下的README.md。

-   Maven示例工程：[aliyun-oss-java-sdk-demo-mvn.zip](https://gosspublic.alicdn.com/sdks/java/aliyun-oss-java-sdk-demo-mvn-3.10.2.zip)
-   Ant示例工程：[aliyun-oss-java-sdk-demo-ant.zip](https://gosspublic.alicdn.com/sdks/java/aliyun-oss-java-sdk-demo-ant-3.10.2.zip)

## 创建存储空间

存储空间是OSS的全局命名空间，相当于数据的容器，可以存储若干文件。 以下代码用于创建存储空间：

```
// Endpoint以杭州为例，其它Region请按实际情况填写。
String endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
// 阿里云主账号AccessKey拥有所有API的访问权限，风险很高。强烈建议您创建并使用RAM账号进行API访问或日常运维，请登录RAM控制台创建RAM账号。
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";

// 创建OSSClient实例。
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// 创建存储空间。
ossClient.createBucket(bucketName);

// 关闭OSSClient。
ossClient.shutdown();            
```

存储空间的命名规范详情请参见[基本概念](/cn.zh-CN/开发指南/基本概念.md)中的命名规范。创建存储空间详情请参见[创建存储空间](/cn.zh-CN/控制台用户指南/存储空间管理/创建存储空间.md)。

获取endpoint详情请参见[访问域名和数据中心](/cn.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md)文档。

## 上传文件

以下代码用于上传文件至OSS：

```
// Endpoint以杭州为例，其它Region请按实际情况填写。
String endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
// 阿里云主账号AccessKey拥有所有API的访问权限，风险很高。强烈建议您创建并使用RAM账号进行API访问或日常运维，请登录RAM控制台创建RAM账号。
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
// <yourObjectName>上传文件到OSS时需要指定包含文件后缀在内的完整路径，例如abc/efg/123.jpg。
String objectName = "<yourObjectName>";

// 创建OSSClient实例。
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// 上传文件到指定的存储空间（bucketName）并将其保存为指定的文件名称（objectName）。
String content = "Hello OSS";
ossClient.putObject(bucketName, objectName, new ByteArrayInputStream(content.getBytes()));

// 关闭OSSClient。
ossClient.shutdown();            
```

上传文件详情请参见[上传文件](/cn.zh-CN/SDK 示例/Java/上传文件/概述.md)。

## 下载文件

以下代码用于下载文件：

```
// Endpoint以杭州为例，其它Region请按实际情况填写。
String endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
// 阿里云主账号AccessKey拥有所有API的访问权限，风险很高。强烈建议您创建并使用RAM账号进行API访问或日常运维，请登录RAM控制台创建RAM账号。
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
// <yourObjectName>从OSS下载文件时需要指定包含文件后缀在内的完整路径，例如abc/efg/123.jpg。
String objectName = "<yourObjectName>";

// 创建OSSClient实例。
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// 调用ossClient.getObject返回一个OSSObject实例，该实例包含文件内容及文件元信息。
OSSObject ossObject = ossClient.getObject(bucketName, objectName);
// 调用ossObject.getObjectContent获取文件输入流，可读取此输入流获取其内容。
InputStream content = ossObject.getObjectContent();
if (content != null) {
    BufferedReader reader = new BufferedReader(new InputStreamReader(content));
    while (true) {
        String line = reader.readLine();
        if (line == null) break;
        System.out.println("\n" + line);
    }
    // 数据读取完成后，获取的流必须关闭，否则会造成连接泄漏，导致请求无连接可用，程序无法正常工作。
    content.close();
}

// 关闭OSSClient。
ossClient.shutdown();            
```

下载文件详情请参见[下载文件](/cn.zh-CN/SDK 示例/Java/下载文件/概述.md)。

## 列举文件

以下代码用于列举指定存储空间下的文件。默认列举100个文件。

```
// Endpoint以杭州为例，其它Region请按实际情况填写。
String endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
// 阿里云主账号AccessKey拥有所有API的访问权限，风险很高。强烈建议您创建并使用RAM账号进行API访问或日常运维，请登录RAM控制台创建RAM账号。
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";

// 创建OSSClient实例。
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// ossClient.listObjects返回ObjectListing实例，包含此次listObject请求的返回结果。
ObjectListing objectListing = ossClient.listObjects(bucketName);
// objectListing.getObjectSummaries获取所有文件的描述信息。
for (OSSObjectSummary objectSummary : objectListing.getObjectSummaries()) {
    System.out.println(" - " + objectSummary.getKey() + "  " +
            "(size = " + objectSummary.getSize() + ")");
}

// 关闭OSSClient。
ossClient.shutdown();            
```

列举文件详情请参见[列举文件](/cn.zh-CN/SDK 示例/Java/管理文件/列举文件.md)。

