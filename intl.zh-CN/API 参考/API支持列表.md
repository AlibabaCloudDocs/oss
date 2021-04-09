API支持列表 
============================

本文介绍存储空间（Bucket）未开启分层命名空间和已开启分层命名空间时的API支持列表。
**说明**

√表示支持，×表示不支持。

关于Service操作 
--------------------------------



|                                                          API                                                           | 未开启分层命名空间 | 已开启分层命名空间 |
|------------------------------------------------------------------------------------------------------------------------|-----------|-----------|
| [GetService (ListBuckets)](/intl.zh-CN/API 参考/关于Service操作/GetService (ListBuckets).md) | √         | √         |





关于Bucket操作 
-------------------------------




|                                                                                   API                                                                                    || 未开启分层命名空间 | 已开启分层命名空间 |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|-----------|-----------|
| 基础操作                  | [PutBucket](/intl.zh-CN/API 参考/关于Bucket的操作/基础操作/PutBucket.md)                                                                     | √         | √         |
| 基础操作                  | [DeleteBucket](/intl.zh-CN/API 参考/关于Bucket的操作/基础操作/DeleteBucket.md)                                                               | √         | √         |
| 基础操作                  | [GetBucket (ListObjects)](/intl.zh-CN/API 参考/关于Bucket的操作/基础操作/GetBucket (ListObjects).md)                                         | √         | √         |
| 基础操作                  | [GetBucketV2 (ListObjectsV2)](/intl.zh-CN/API 参考/关于Bucket的操作/基础操作/GetBucketV2 (ListObjectsV2).md)                                 | √         | √         |
| 基础操作                  | [GetBucketInfo](/intl.zh-CN/API 参考/关于Bucket的操作/基础操作/GetBucketInfo.md)                                                             | √         | √         |
| 基础操作                  | [GetBucketLocation](/intl.zh-CN/API 参考/关于Bucket的操作/基础操作/GetBucketLocation.md)                                                     | √         | √         |
| 合规保留策略（WORM）          | [InitiateBucketWorm](/intl.zh-CN/API 参考/关于Bucket的操作/合规保留策略（WORM）/InitiateBucketWorm.md)                                           | √         | ×         |
| 合规保留策略（WORM）          | [AbortBucketWorm](/intl.zh-CN/API 参考/关于Bucket的操作/合规保留策略（WORM）/AbortBucketWorm.md)                                                 | √         | ×         |
| 合规保留策略（WORM）          | [CompleteBucketWorm](/intl.zh-CN/API 参考/关于Bucket的操作/合规保留策略（WORM）/CompleteBucketWorm.md)                                           | √         | ×         |
| 合规保留策略（WORM）          | [ExtendBucketWorm](/intl.zh-CN/API 参考/关于Bucket的操作/合规保留策略（WORM）/ExtendBucketWorm.md)                                               | √         | ×         |
| 合规保留策略（WORM）          | [GetBucketWorm](/intl.zh-CN/API 参考/关于Bucket的操作/合规保留策略（WORM）/GetBucketWorm.md)                                                     | √         | ×         |
| 权限控制（ACL）             | [PutBucketAcl](/intl.zh-CN/API 参考/关于Bucket的操作/权限控制（ACL）/PutBucketAcl.md)                                                          | √         | √         |
| 权限控制（ACL）             | [GetBucketAcl](/intl.zh-CN/API 参考/关于Bucket的操作/权限控制（ACL）/GetBucketAcl.md)                                                          | √         | √         |
| 生命周期（Lifecycle）       | [PutBucketLifecycle](/intl.zh-CN/API 参考/关于Bucket的操作/生命周期（Lifecycle）/PutBucketLifecycle.md)                                        | √         | ×         |
| 生命周期（Lifecycle）       | [GetBucketLifecycle](/intl.zh-CN/API 参考/关于Bucket的操作/生命周期（Lifecycle）/GetBucketLifecycle.md)                                        | √         | ×         |
| 生命周期（Lifecycle）       | [DeleteBucketLifecycle](/intl.zh-CN/API 参考/关于Bucket的操作/生命周期（Lifecycle）/DeleteBucketLifecycle.md)                                  | √         | ×         |
| 版本控制（Versioning）      | [PutBucketVersioning](/intl.zh-CN/API 参考/关于Bucket的操作/版本控制（Versioning）/PutBucketVersioning.md)                                     | √         | ×         |
| 版本控制（Versioning）      | [GetBucketVersioning](/intl.zh-CN/API 参考/关于Bucket的操作/版本控制（Versioning）/GetBucketVersioning.md)                                     | √         | ×         |
| 版本控制（Versioning）      | [GetBucketVersions(ListObjectVersions)](/intl.zh-CN/API 参考/关于Bucket的操作/版本控制（Versioning）/GetBucketVersions(ListObjectVersions).md) | √         | ×         |
| 跨区域复制（Replication）    | [PutBucketReplication](/intl.zh-CN/API 参考/关于Bucket的操作/跨区域复制（Replication）/PutBucketReplication.md)                                 | √         | ×         |
| 跨区域复制（Replication）    | [GetBucketReplication](/intl.zh-CN/API 参考/关于Bucket的操作/跨区域复制（Replication）/GetBucketReplication.md)                                 | √         | ×         |
| 跨区域复制（Replication）    | [GetBucketReplicationLocation](/intl.zh-CN/API 参考/关于Bucket的操作/跨区域复制（Replication）/GetBucketReplicationLocation.md)                 | √         | ×         |
| 跨区域复制（Replication）    | [GetBucketReplicationProgress](/intl.zh-CN/API 参考/关于Bucket的操作/跨区域复制（Replication）/GetBucketReplicationProgress.md)                 | √         | ×         |
| 跨区域复制（Replication）    | [DeleteBucketReplication](/intl.zh-CN/API 参考/关于Bucket的操作/跨区域复制（Replication）/DeleteBucketReplication.md)                           | √         | ×         |
| 授权策略（Policy）          | [PutBucketPolicy](/intl.zh-CN/API 参考/关于Bucket的操作/授权策略（Policy）/PutBucketPolicy.md)                                                 | √         | √         |
| 授权策略（Policy）          | [GetBucketPolicy](/intl.zh-CN/API 参考/关于Bucket的操作/授权策略（Policy）/GetBucketPolicy.md)                                                 | √         | √         |
| 授权策略（Policy）          | [DeleteBucketPolicy](/intl.zh-CN/API 参考/关于Bucket的操作/授权策略（Policy）/DeleteBucketPolicy.md)                                           | √         | √         |
| 清单（Inventory）         | [PutBucketInventory](/intl.zh-CN/API 参考/关于Bucket的操作/清单（Inventory）/PutBucketInventory.md)                                          | √         | ×         |
| 清单（Inventory）         | [GetBucketInventory](/intl.zh-CN/API 参考/关于Bucket的操作/清单（Inventory）/GetBucketInventory.md)                                          | √         | ×         |
| 清单（Inventory）         | [ListBucketInventory](/intl.zh-CN/API 参考/关于Bucket的操作/清单（Inventory）/ListBucketInventory.md)                                        | √         | ×         |
| 清单（Inventory）         | [DeleteBucketInventory](/intl.zh-CN/API 参考/关于Bucket的操作/清单（Inventory）/DeleteBucketInventory.md)                                    | √         | ×         |
| 日志管理（Logging）         | [PutBucketLogging](/intl.zh-CN/API 参考/关于Bucket的操作/日志管理（Logging）/PutBucketLogging.md)                                              | √         | √         |
| 日志管理（Logging）         | [GetBucketLogging](/intl.zh-CN/API 参考/关于Bucket的操作/日志管理（Logging）/GetBucketLogging.md)                                              | √         | √         |
| 日志管理（Logging）         | [DeleteBucketLogging](/intl.zh-CN/API 参考/关于Bucket的操作/日志管理（Logging）/DeleteBucketLogging.md)                                        | √         | √         |
| 静态网站（Website）         | [PutBucketWebsite](/intl.zh-CN/API 参考/关于Bucket的操作/静态网站（Website）/PutBucketWebsite.md)                                              | √         | ×         |
| 静态网站（Website）         | [GetBucketWebsite](/intl.zh-CN/API 参考/关于Bucket的操作/静态网站（Website）/GetBucketWebsite.md)                                              | √         | ×         |
| 静态网站（Website）         | [DeleteBucketWebsite](/intl.zh-CN/API 参考/关于Bucket的操作/静态网站（Website）/DeleteBucketWebsite.md)                                        | √         | ×         |
| 防盗链（Referer）          | [PutBucketReferer](/intl.zh-CN/API 参考/关于Bucket的操作/防盗链（Referer）/PutBucketReferer.md)                                               | √         | √         |
| 防盗链（Referer）          | [GetBucketReferer](/intl.zh-CN/API 参考/关于Bucket的操作/防盗链（Referer）/GetBucketReferer.md)                                               | √         | √         |
| 标签（Tags）              | [PutBucketTags](/intl.zh-CN/API 参考/关于Bucket的操作/标签（Tags）/PutBucketTags.md)                                                         | √         | √         |
| 标签（Tags）              | [GetObjectTagging](/intl.zh-CN/API 参考/关于Object操作/标签（Tagging）/GetObjectTagging.md)                                                 | √         | √         |
| 标签（Tags）              | [DeleteBucketTags](/intl.zh-CN/API 参考/关于Bucket的操作/标签（Tags）/DeleteBucketTags.md)                                                   | √         | √         |
| 加密（Encryption）        | [PutBucketEncryption](/intl.zh-CN/API 参考/关于Bucket的操作/加密（Encryption）/PutBucketEncryption.md)                                       | √         | √         |
| 加密（Encryption）        | [GetBucketEncryption](/intl.zh-CN/API 参考/关于Bucket的操作/加密（Encryption）/GetBucketEncryption.md)                                       | √         | √         |
| 加密（Encryption）        | [DeleteBucketEncryption](/intl.zh-CN/API 参考/关于Bucket的操作/加密（Encryption）/DeleteBucketEncryption.md)                                 | √         | √         |
| 请求者付费（RequestPayment） | [PutBucketRequestPayment](/intl.zh-CN/API 参考/关于Bucket的操作/请求者付费（RequestPayment）/PutBucketRequestPayment.md)                        | √         | √         |
| 请求者付费（RequestPayment） | [GetBucketRequestPayment](/intl.zh-CN/API 参考/关于Bucket的操作/请求者付费（RequestPayment）/GetBucketRequestPayment.md)                        | √         | √         |
| 跨域资源共享（CORS）          | [PutBucketCors](/intl.zh-CN/API 参考/关于Bucket的操作/跨域资源共享（CORS）/PutBucketCors.md)                                                     | √         | ×         |
| 跨域资源共享（CORS）          | [GetBucketCors](/intl.zh-CN/API 参考/关于Bucket的操作/跨域资源共享（CORS）/GetBucketCors.md)                                                     | √         | ×         |
| 跨域资源共享（CORS）          | [DeleteBucketCors](/intl.zh-CN/API 参考/关于Bucket的操作/跨域资源共享（CORS）/DeleteBucketCors.md)                                               | √         | ×         |
| 跨域资源共享（CORS）          | [OptionObject](/intl.zh-CN/API 参考/关于Bucket的操作/跨域资源共享（CORS）/OptionObject.md)                                                       | √         | ×         |



关于Object操作 
-------------------------------




|                                                                        API                                                                         || 未开启分层命名空间 | 已开启分层命名空间 |
|------------------------|----------------------------------------------------------------------------------------------------------------------------|-----------|-----------|
| 基础操作                   | [PutObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/PutObject.md)                                               | √         | √         |
| 基础操作                   | [GetObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/GetObject.md)                                               | √         | √         |
| 基础操作                   | [CopyObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/CopyObject.md)                                             | √         | √         |
| 基础操作                   | [AppendObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/AppendObject.md)                                         | √         | ×         |
| 基础操作                   | [DeleteObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/DeleteObject.md)                                         | √         | √         |
| 基础操作                   | [DeleteMultipleObjects](/intl.zh-CN/API 参考/关于Object操作/基础操作/DeleteMultipleObjects.md)                       | √         | ×         |
| 基础操作                   | [HeadObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/HeadObject.md)                                             | √         | √         |
| 基础操作                   | [GetObjectMeta](/intl.zh-CN/API 参考/关于Object操作/基础操作/GetObjectMeta.md)                                       | √         | √         |
| 基础操作                   | [PostObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/PostObject.md)                                             | √         | √         |
| 基础操作                   | [Callback](/intl.zh-CN/API 参考/关于Object操作/基础操作/Callback.md)                                                 | √         | ×         |
| 基础操作                   | [RestoreObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/RestoreObject.md)                                       | √         | ×         |
| 基础操作                   | [SelectObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/SelectObject.md)                                         | √         | ×         |
| 目录管理                   | [CreateDirectory](/intl.zh-CN/API 参考/关于Object操作/目录管理/CreateDirectory.md)                                   | ×         | √         |
| 目录管理                   | [Rename](/intl.zh-CN/API 参考/关于Object操作/目录管理/Rename.md)                                                     | ×         | √         |
| 目录管理                   | [DeleteDirectory](/intl.zh-CN/API 参考/关于Object操作/目录管理/DeleteDirectory.md)                                   | ×         | √         |
| 分片上传（MulitipartUpload） | [InitiateMultipartUpload](/intl.zh-CN/API 参考/关于Object操作/分片上传（MulitipartUpload）/InitiateMultipartUpload.md) | √         | √         |
| 分片上传（MulitipartUpload） | [UploadPart](/intl.zh-CN/API 参考/关于Object操作/分片上传（MulitipartUpload）/UploadPart.md)                           | √         | √         |
| 分片上传（MulitipartUpload） | [UploadPartCopy](/intl.zh-CN/API 参考/关于Object操作/分片上传（MulitipartUpload）/UploadPartCopy.md)                   | √         | √         |
| 分片上传（MulitipartUpload） | [CompleteMultipartUpload](/intl.zh-CN/API 参考/关于Object操作/分片上传（MulitipartUpload）/CompleteMultipartUpload.md) | √         | √         |
| 分片上传（MulitipartUpload） | [AbortMultipartUpload](/intl.zh-CN/API 参考/关于Object操作/分片上传（MulitipartUpload）/AbortMultipartUpload.md)       | √         | √         |
| 分片上传（MulitipartUpload） | [ListMultipartUploads](/intl.zh-CN/API 参考/关于Object操作/分片上传（MulitipartUpload）/ListMultipartUploads.md)       | √         | √         |
| 分片上传（MulitipartUpload） | [ListParts](/intl.zh-CN/API 参考/关于Object操作/分片上传（MulitipartUpload）/ListParts.md)                             | √         | √         |
| 权限控制（ACL)              | [PutObjectACL](/intl.zh-CN/API 参考/关于Object操作/权限控制（ACL)/PutObjectACL.md)                                    | √         | √         |
| 权限控制（ACL)              | [GetObjectACL](/intl.zh-CN/API 参考/关于Object操作/权限控制（ACL)/GetObjectACL.md)                                    | √         | √         |
| 软链接（Symlink）           | [PutSymlink](/intl.zh-CN/API 参考/关于Object操作/软链接（Symlink）/PutSymlink.md)                                     | √         | ×         |
| 软链接（Symlink）           | [GetSymlink](/intl.zh-CN/API 参考/关于Object操作/软链接（Symlink）/GetSymlink.md)                                     | √         | ×         |
| 标签（Tagging）            | [PutObjectTagging](/intl.zh-CN/API 参考/关于Object操作/标签（Tagging）/PutObjectTagging.md)                          | √         | √         |
| 标签（Tagging）            | [GetObjectTagging](/intl.zh-CN/API 参考/关于Object操作/标签（Tagging）/GetObjectTagging.md)                          | √         | √         |
| 标签（Tagging）            | [DeleteObjectTagging](/intl.zh-CN/API 参考/关于Object操作/标签（Tagging）/DeleteObjectTagging.md)                    | √         | √         |



关于LiveChanne操作 
-----------------------------------



|                                                  API                                                  | 未开启分层命名空间 | 已开启分层命名空间 |
|-------------------------------------------------------------------------------------------------------|-----------|-----------|
| [PutLiveChannel](/intl.zh-CN/API 参考/关于LiveChannel的操作/PutLiveChannel.md)               | √         | ×         |
| [ListLiveChannel](/intl.zh-CN/API 参考/关于LiveChannel的操作/ListLiveChannel.md)             | √         | ×         |
| [DeleteLiveChannel](/intl.zh-CN/API 参考/关于LiveChannel的操作/DeleteLiveChannel.md)         | √         | ×         |
| [PutLiveChannelStatus](/intl.zh-CN/API 参考/关于LiveChannel的操作/PutLiveChannelStatus.md)   | √         | ×         |
| [GetLiveChannelInfo](/intl.zh-CN/API 参考/关于LiveChannel的操作/GetLiveChannelInfo.md)       | √         | ×         |
| [GetLiveChannelStat](/intl.zh-CN/API 参考/关于LiveChannel的操作/GetLiveChannelStat.md)       | √         | ×         |
| [GetLiveChannelHistory](/intl.zh-CN/API 参考/关于LiveChannel的操作/GetLiveChannelHistory.md) | √         | ×         |
| [PostVodPlaylist](/intl.zh-CN/API 参考/关于LiveChannel的操作/PostVodPlaylist.md)             | √         | ×         |
| [GetVodPlaylist](/intl.zh-CN/API 参考/关于LiveChannel的操作/GetVodPlaylist.md)               | √         | ×         |



关于分层命名空间的更多信息，请参见[分层命名空间](/intl.zh-CN/开发指南/对象/文件（Object）/分层命名空间.md)。

