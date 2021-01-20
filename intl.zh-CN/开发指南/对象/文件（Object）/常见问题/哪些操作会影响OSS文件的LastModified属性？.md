# 哪些操作会影响OSS文件的LastModified属性？

LastModified（最后修改时间）是OSS文件（Object）的一个重要属性，在计费、增量迁移、生命周期规则等场景中都会涉及。您在使用OSS时，部分针对Object的操作可能会更新Object的LastModified，下表列举了针对Object的常见操作。

|常见操作|使用接口|是否会更新Object的LastModified|
|----|----|------------------------|
|修改ACL|CopyObject|是|
|PutObjectACL|是|
|修改usermeta|CopyObject|是|
|修改存储类型|通过覆写修改：CopyObject|是|
|通过生命周期修改：CommitTransition|否|
|修改加密算法|CopyObject|是|
|覆写Object|上传同名文件覆写：PutObject|是|
|拷贝同名文件覆写：CopyObject|是|
|添加或修改Object标签|PutObjectTagging|否|
|删除Object标签|DeleteObjectTagging|否|
|归档、冷归档类型文件的解冻|RestoreObject|否|

**说明：**

-   通过控制台、ossutil、ossbrowser、SDK等方式修改Object存储类型时，都是通过覆写Object并指定新的存储类型的方式实现的。
-   覆写Object以及通过CopyObject修改Object的存储类型、加密方式会更新Object的LastModified，若上述操作涉及的Object类型为低频、归档、冷归档存储类型且存储未满规定时长，还会产生存储不足规定时长容量费用。详情请参见[存储费用](/intl.zh-CN/计量计费/计量项和计费项/存储费用.md)。

    例如某个低频存储类型的Object，您在其存储了12天后通过OSS控制台将其修改为归档存储类型，此时Object的LastModified将被相应修改。另外，因低频访问类型文件最低存储时长要求为30天，还将产生18天的存储不足规定时长容量费用。


