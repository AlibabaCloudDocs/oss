# 开启版本控制下Object的操作

存储空间（Bucket）开启版本控制后，OSS会为Bucket中所有文件（Object）的每个版本指定唯一的ID值，且Bucket中现有Object的内容、权限保持不变。开启版本控制后，还能够防止意外覆盖或者删除Object ，并允许查询、恢复Object的历史版本。

## 注意事项

当您在开启了版本控制的Bucket中进行上传文件、列举文件、下载文件、删除文件、恢复文件等操作时，有如下注意事项：

-   开启版本控制的Bucket会维护一个当前版本Object，以及零个或零个以上历史版本Object。
-   如果在Bucket开启版本控制前上传了Object，则OSS将Object的版本ID值置为null。
-   以下图示中的版本ID均以简短版本ID代替。

有关版本控制的详细说明，请参见[版本控制介绍](/cn.zh-CN/开发指南/数据安全/版本控制/版本控制介绍.md)。

## 上传文件

在开启了版本控制的Bucket上传Object时，OSS会为这些上传的Object自动添加唯一的版本ID。

**说明：** PutObject、PostObject、CopyObject 、MultipartUpload等操作都会为新生成的Object自动添加唯一的版本ID。

通过PUT操作上传Object（key=example.jpg）时，OSS为该Object指定了唯一的版本号（ID=111111），如下图所示。

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4747559951/p40175.jpg)

通过PUT操作第一次上传同名Object（key=example.jpg）时，原始Object版本（ID=111111）作为历史版本，生成的新版本（ID=222222）将作为当前版本保存在Bucket中。当再次上传同名Object时，原始Object版本（包括ID=111111以及ID=222222）将作为历史版本，而生成的新版本（ID=333333）则作为当前版本保存在Bucket中，如下图所示。

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4747559951/p40176.jpg)

您可以通过[cp命令](/cn.zh-CN/常用工具/命令行工具ossutil/常用命令/cp/上传文件.md)、[Java SDK](/cn.zh-CN/SDK 示例/Java/版本控制/上传文件.md)、[PHP SDK](/cn.zh-CN/SDK 示例/PHP/版本控制/上传文件.md)、[Node.js SDK](/cn.zh-CN/SDK 示例/Node.js/版本控制/上传文件.md)、[Python SDK](/cn.zh-CN/SDK 示例/Python/版本控制/上传文件.md)、[.NET SDK](/cn.zh-CN/SDK 示例/.NET/版本控制/上传文件.md)、[Go SDK](/cn.zh-CN/SDK 示例/Go/版本控制/上传文件.md)、[C++ SDK](/cn.zh-CN/SDK 示例/C++/版本控制/上传文件.md)的方式在已开启版本控制的Bucket中上传文件。

## 列举文件

在开启了版本控制的Bucket中，您可以使用GetBucketVersions\(ListObjectVersions\)接口获取Object的所有版本信息，包括删除标记（Delete Marker）。

-   与GetBucketVersions\(ListObjectVersions\)不同的是，GetBucket\(ListObject\)接口仅返回Object的当前版本，且当前版本不为删除标记。
-   单个GetBucketVersions\(ListObjectVersions\)请求最多返回1000个版本Object。您可以通过发送多次请求来获取Object的所有版本。

    例如，如果Bucket中包含两个Key（如example.jpg和photo.jpg），且第一个Key（example.jpg）有900个版本，第二个Key（photo.jpg）有500个版本，则单个请求将先按照Key的字母序，再按照版本的新旧顺序依次列举example.jpg的所有900个版本，另加photo.jpg的100个版本。


如下图所示，在开启了版本控制的Bucket中，调用GetBucketVersions\(ListObjectVersions\)接口时，返回了Bucket中所有Object的所有版本，包含当前版本为删除标记的Object ；调用GetBucket\(ListObject\)接口时，则仅返回Object的当前版本，且当前版本不能为删除标记，因此仅返回当前版本ID为444444的Object。

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4747559951/p40404.jpg)

您可以通过[ls命令](/cn.zh-CN/常用工具/命令行工具ossutil/常用命令/ls（列举）.md)、[Java SDK](/cn.zh-CN/SDK 示例/Java/版本控制/列举文件.md)、[Node.js SDK](/cn.zh-CN/SDK 示例/Node.js/版本控制/列举文件.md)、[Python SDK](/cn.zh-CN/SDK 示例/Python/版本控制/列举文件.md)、[Go SDK](/cn.zh-CN/SDK 示例/Go/版本控制/列举文件.md)的方式在开启版本控制的Bucket中列举文件。

## 下载文件

您可以在开启了版本控制Bucket下载当前版本或指定版本的Object。

通过GET请求下载Object时，如果没有指定Object的版本ID，默认情况下返回Object的当前版本。如下图所示返回Object的当前版本（ID=333333）。

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4747559951/p40220.jpg)

在当前版本为删除标记（Delete Marker）时执行GET操作时返回404 Not Found。

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4747559951/p40666.jpg)

如果要下载指定的Object版本，则通过GET请求下载Object时需要指定其版本ID ，如下图所示获取指定版本ID为222222的Object。

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4747559951/p40221.jpg)

您可以通过[cp命令](/cn.zh-CN/常用工具/命令行工具ossutil/常用命令/cp/下载文件.md)、[Java SDK](/cn.zh-CN/SDK 示例/Java/版本控制/下载文件.md)、[PHP SDK](/cn.zh-CN/SDK 示例/PHP/版本控制/下载文件.md)、[Node.js SDK](/cn.zh-CN/SDK 示例/Node.js/版本控制/下载文件.md)、[Python SDK](/cn.zh-CN/SDK 示例/Python/版本控制/下载文件.md)、[.NET SDK](/cn.zh-CN/SDK 示例/.NET/版本控制/下载文件.md)、[Go SDK](/cn.zh-CN/SDK 示例/Go/版本控制/下载文件.md)、[Go SDK](/cn.zh-CN/SDK 示例/C++/版本控制/下载文件.md)的方式在已开启版本控制的Bucket中下载文件。

## 删除文件

开启版本控制后，您可以通过指定Object版本ID或者配置Lifecycle将Object永久删除。如果删除Object时未指定版本ID，则Bucket中将插入一个删除标记（Delete Marker）作为当前版本。

**说明：** 开启版本控制后，若删除Object时未指定版本ID，默认不会删除Object的当前版本以及历史版本。

此外，您还可以在开启版本控制的Bucket中通过生命周期规则的Expiration元素指定对象当前版本过期，还可以通过NoncurrentVersionExpiration元素永久删除非当前版本对象，对于这两种元素的详细说明如下：

-   Expiration元素应用于当前对象版本，OSS通过添加删除标记将当前版本作为非当前版本保留，而不是删除当前版本对象，然后删除标记将成为对象的当前版本。
-   NoncurrentVersionExpiration元素适用于非当前版本对象，OSS会永久删除这些对象版本，且无法恢复永久删除的对象。

有关版本控制结合生命周期的更多信息，请参见[生命周期配置元素](/cn.zh-CN/开发指南/对象/文件（Object）/文件生命周期/生命周期配置元素.md)。

以下分别说明在未指定版本ID以及指定版本ID的情况下，执行DELETE操作时Object的删除行为。

-   如果未指定Object的版本ID，则OSS会插入一个删除标记作为当前版本，该删除标记也会有相应的唯一版本ID，但没有相关数据和ACL等，如下图所示（当前版本为删除标记，且版本ID=444444）。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5747559951/p40237.jpg)

-   如果指定Object的版本ID，则永久删除该指定版本的Object，如下图所示（即删除版本ID=333333的Object）。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5747559951/p40245.jpg)


您可以通过[rm命令](/cn.zh-CN/常用工具/命令行工具ossutil/常用命令/rm.md)、[Java SDK](/cn.zh-CN/SDK 示例/Java/版本控制/删除文件.md)、[PHP SDK](/cn.zh-CN/SDK 示例/PHP/版本控制/删除文件.md)、[Node.js SDK](/cn.zh-CN/SDK 示例/Node.js/版本控制/删除文件.md)、[Python SDK](/cn.zh-CN/SDK 示例/Python/版本控制/删除文件.md)、[.NET SDK](/cn.zh-CN/SDK 示例/.NET/版本控制/删除文件.md)、[Go SDK](/cn.zh-CN/SDK 示例/Go/版本控制/删除文件.md)、[C++ SDK](/cn.zh-CN/SDK 示例/C++/版本控制/删除文件.md)的方式在已开启版本控制的Bucket中删除文件。

## 恢复文件

开启版本控制后，Bucket中Object的所有版本都将得以保留。您可以通过恢复指定历史版本的方式，使得任意Object的历史版本成为当前版本。

您可以通过以下两种方式将Object的历史版本恢复至当前版本：

-   通过CopyObject来恢复Object的历史版本

    复制的Object将成为该Object的当前版本，且所有Object版本都将保留。

    如下图所示，将原Object的历史版本（ID=222222）复制到同一个Bucket中，OSS将为该Object生成新的版本（ID=444444），并将其置为该Object的当前版本。因此，该Object同时具有历史版本（ID=222222）以及当前版本（ID=444444）。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5747559951/p40309.jpg)

-   通过删除Object的当前版本来恢复Object的历史版本

    如下图所示，当您通过DELETE versionId的方式永久删除当前Object版本（ID=222222）后， 下一个历史版本（ID=111111）成为了该Object的当前版本。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5747559951/p40310.jpg)


**说明：** 由于Object的当前版本删除后无法恢复，建议您通过CopyObject的方式来恢复Object的历史版本。

您可以通过[cp命令](/cn.zh-CN/常用工具/命令行工具ossutil/常用命令/cp/拷贝文件.md)、[Java SDK](/cn.zh-CN/SDK 示例/Java/版本控制/拷贝文件.md)、[PHP SDK](/cn.zh-CN/SDK 示例/PHP/版本控制/拷贝文件.md)、[Node.js SDK](/cn.zh-CN/SDK 示例/Node.js/版本控制/拷贝文件.md)、[Python SDK](/cn.zh-CN/SDK 示例/Python/版本控制/拷贝文件.md)、[.NET SDK](/cn.zh-CN/SDK 示例/.NET/版本控制/拷贝文件.md)、[Go SDK](/cn.zh-CN/SDK 示例/Go/版本控制/拷贝文件.md)、[C++ SDK](/cn.zh-CN/SDK 示例/C++/版本控制/拷贝文件.md)的方式在已开启版本控制的Bucket中恢复历史版本文件。

