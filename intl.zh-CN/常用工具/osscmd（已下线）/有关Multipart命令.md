# 有关Multipart命令

本文主要介绍与碎片（part）相关的命令。

**说明：** osscmd操作命令已整合到[ossutil](/intl.zh-CN/常用工具/命令行工具ossutil/概述.md)中，工具已于2019年7月31日下线，给您带来不便敬请谅解。

## init

命令说明：

`init oss://bucket/object`

初始化生成一个Upload ID。这个Upload ID可以配合后面的multiupload命令来使用。

使用示例：

`python osscmd init oss://mybucket/myobject`

## listpart

命令说明：

`listpart oss://bucket/object --upload_id=xxx`

显示指定object的Upload ID下已经上传的Parts。相关概念见[OSS API文档](/intl.zh-CN/API 参考/简介.md)。必须要指定Upload ID。

使用示例：

```
python osscmd listpart oss://mybucket/myobject --upload_id=
          75835E389EA648C0B93571B6A46023F3
```

## listparts

命令说明：

`listparts oss://bucket`

显示bucket中未完成的multipart Upload ID和object。一般在删除bucket提示bucket非空的情况下，可以用这个命令查看是否有multipart相关的内容。

使用示例：

`python osscmd listparts oss://mybucket`

## getallpartsize

命令说明：

`getallpartsize oss://bucket`

显示bucket中还存在的Upload ID下已经上传的Parts的总大小。

使用示例：

`python osscmd getallpartsize oss://mybucket`

## cancel

命令说明：

`cancel oss://bucket/object --upload_id=xxx`

终止Upload ID对应的Multipart Upload事件。

使用示例：

```
python osscmd cancel oss://mybucket/myobject --upload_id=
          D9D278DB6F8845E9AFE797DD235DC576
```

## multiupload\(multi\_upload,mp\)

命令说明：

```
multiupload(multi_upload,mp) localfile oss://bucket/object --check_md5=false
        --thread_num=10
```

将本地文件以multipart的方式上传到OSS。

使用示例：

-   `python osscmd multiupload /tmp/localfile.txt oss://mybucket/object`
-   `python osscmd multiup_load /tmp/localfile.txt oss://mybucket/object`
-   `python osscmd mp /tmp/localfile.txt oss://mybucket/object`

命令说明：

```
multiupload(multi_upload,mp) localfile oss://bucket/object --upload_id=xxx --thread_num=10
        --max_part_num=1000 --check_md5=false
```

将本地文件以multipart的方式上传到OSS。本地文件划分的块数由max\_part\_num来指定。这个命令在实现的时候，会先去判断Upload ID对应的Parts的ETag是否和本地文件的MD5值是否相等，相等则跳过上传。所以在使用之前生成一个Upload ID，作为参数传进来。即使上传没有成功，重复执行相同的multiupload命令可以达到一个断点续传的效果。`--check_md5=false`表示上传文件时，不会做携带Content-MD5请求头校验。true则会做校验。

使用示例：

-   ```
python osscmd multiupload /tmp/localfile.txt oss://mybucket/object --upload_id=
          D9D278DB6F8845E9AFE797DD235DC576
```

-   ```
python osscmd multiup_load /tmp/localfile.txt oss://mybucket/object
        --thread_num=5
```

-   `python osscmd mp /tmp/localfile.txt oss://mybucket/object --max_part_num=100`

## copylargefile

命令说明：

```
copylargefile oss://source_bucket/source_object oss://target_bucket/target_object
        --part_size=10*1024*1024 --upload_id=xxx
```

对于超过1G的大文件进行复制时，采用multipart的方式将object复制到指定位置（源bucket必须与目标bucket处于同一region）。其中upload\_id为可选参数，当需要对某一次multipart copy事件进行续传的时候，可以传入该事件的upload\_id。part\_size用来设定分块大小，分块最小需要大于100KB，最多支持10000块分块。如果part\_size设定值导致与OSS限制冲突，程序会帮你自动调节分块大小。

使用示例：

```
python osscmd copylargefile oss://source_bucket/source_object
          oss://target_bucket/target_object --part_size=10*1024*1024
```

## uploadpartfromfile \(upff\)

命令说明：

```
uploadpartfromfile (upff) localfile oss://bucket/object --upload_id=xxx
        --part_number=xxx
```

主要用于测试，不推荐使用。

## uploadpartfromstring\(upfs\)

命令说明：

```
uploadpartfromstring(upfs) oss://bucket/object --upload_id=xxx --part_number=xxx
        --data=xxx
```

主要用于测试，不推荐使用。

