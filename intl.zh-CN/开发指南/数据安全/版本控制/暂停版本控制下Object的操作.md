# 暂停版本控制下Object的操作

您可以暂停版本控制以停止在存储空间（Bucket ） 中继续累积同一文件（Object）的新版本。暂停版本控制后，您可以上传文件，并通过指定版本ID（versionId）的方式对历史版本Object进行下载和删除操作。

## 上传文件

向暂停版本控制的存储空间（Bucket）上传文件（Object）时，OSS将为新生成的Object添加versionId为null的版本，且每个Object只会保留一个versionId为null的版本。

-   如下图所示，向暂停版本控制的Bucket中通过PUT操作上传Object时，OSS会为上传的Object自动添加null的版本ID。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6747559951/p40340.jpg)

-   如下图所示，如果暂停版本控制的Bucket中存在开启版本控制时生成的Object版本（ID=111111），通过PUT操作向该Bucket上传同名Object时，OSS会为新版本Object分配null的版本ID ，且该版本作为当前版本，同时开启版本控制时生成的Object版本（ID=111111）将作为历史版本保存下来。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6747559951/p40343.jpg)

-   如果暂停版本控制的Bucket中已存在版本ID为null的Object ，通过PUT操作向该Bucket上传同名Object时，原版本ID为null的版本将被覆盖。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6747559951/p40352.jpg)


您可以通过[cp命令](/intl.zh-CN/常用工具/命令行工具ossutil/常用命令/cp/上传文件.md)、[Java SDK](/intl.zh-CN/SDK 示例/Java/版本控制/上传文件.md)、[PHP SDK](/intl.zh-CN/SDK 示例/PHP/版本控制/上传文件.md)、[Node.js SDK](/intl.zh-CN/SDK 示例/Node.js/版本控制/上传文件.md)、[Python SDK](/intl.zh-CN/SDK 示例/Python/版本控制/上传文件.md)、[.NET SDK](/intl.zh-CN/SDK 示例/.NET/版本控制/上传文件.md)、[Go SDK](/intl.zh-CN/SDK 示例/Go/版本控制/上传文件.md)、[C++ SDK](/intl.zh-CN/SDK 示例/C++/版本控制/上传文件.md)的方式在已暂停版本控制的Bucket中上传文件。

## 下载文件

您可以在暂停版本控制的存储空间（Bucket）中下载当前版本或指定版本的文件（Object）。

通过GET请求下载Object时：

-   如果没有指定Object的版本ID，默认情况下返回Object的当前版本。如下图所示返回版本ID为null的当前版本。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6747559951/p40355.jpg)

-   如果要下载指定的版本，则通过GET请求下载Object时需要指定其版本ID ，如下图所示获取的指定版本为（ID=222222）。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6747559951/p40356.jpg)


您可以通过[cp命令](/intl.zh-CN/常用工具/命令行工具ossutil/常用命令/cp/下载文件.md)、[Java SDK](/intl.zh-CN/SDK 示例/Java/版本控制/下载文件.md)、[PHP SDK](/intl.zh-CN/SDK 示例/PHP/版本控制/下载文件.md)、[Node.js SDK](/intl.zh-CN/SDK 示例/Node.js/版本控制/下载文件.md)、[Python SDK](/intl.zh-CN/SDK 示例/Python/版本控制/下载文件.md)、[.NET SDK](/intl.zh-CN/SDK 示例/.NET/版本控制/下载文件.md)、[Go SDK](/intl.zh-CN/SDK 示例/Go/版本控制/下载文件.md)、[Go SDK](/intl.zh-CN/SDK 示例/C++/版本控制/下载文件.md)的方式在已暂停版本控制的Bucket中下载文件。

## 删除文件

在暂停版本控制的Bucket中执行DELETE操作时，分以下三种情形：

-   如果对Bucket中当前版本ID不为null的Object执行DELETE操作时，则OSS会插入版本ID为null的删除标记（Delete Marker）作为当前版本。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6747559951/p40362.jpg)

-   如果对Bucket中当前版本ID为null的Object执行DELETE操作时，则OSS会插入版本ID为null的删除标记（Delete Marker）作为当前版本。由于OSS保证同一个Object只允许存在一个null的版本，因此原版本ID为null的版本将被覆盖。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6747559951/p40363.jpg)

-   如果通过DELETE+versionId的方式删除Object ，则该指定版本的Object将被永久删除，如下图所示（即删除版本ID=333333的Object ）。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6747559951/p40360.jpg)


您可以通过[rm命令](/intl.zh-CN/常用工具/命令行工具ossutil/常用命令/rm.md)、[Java SDK](/intl.zh-CN/SDK 示例/Java/版本控制/删除文件.md)、[PHP SDK](/intl.zh-CN/SDK 示例/PHP/版本控制/删除文件.md)、[Node.js SDK](/intl.zh-CN/SDK 示例/Node.js/版本控制/删除文件.md)、[Python SDK](/intl.zh-CN/SDK 示例/Python/版本控制/删除文件.md)、[.NET SDK](/intl.zh-CN/SDK 示例/.NET/版本控制/删除文件.md)、[Go SDK](/intl.zh-CN/SDK 示例/Go/版本控制/删除文件.md)、[C++ SDK](/intl.zh-CN/SDK 示例/C++/版本控制/删除文件.md)的方式在已暂停版本控制的Bucket中删除文件。

