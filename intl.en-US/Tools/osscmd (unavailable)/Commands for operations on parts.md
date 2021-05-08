# Commands for operations on parts

This topic describes commands that can be used to manage parts.

**Note:** Commands supported by the osscmd tool have been integrated with the [ossutil](/intl.en-US/Tools/ossutil/Overview.md) tool. The osscmd tool is no longer available for downloads as of July 31, 2019.

## init

Command:

`init oss://bucket/object`

Initialize an upload event to generate an upload ID. You can add this upload ID to the multiupload command to perform operations on parts.

Example:

`python osscmd init oss://mybucket/myobject`

## listpart

Command:

`listpart oss://bucket/object --upload_id=xxx`

List the parts that are uploaded by using the upload ID of a specified object. For more information about related concepts, see [OSS API Reference](/intl.en-US/API Reference/Description.md). You must specify the upload ID.

Example:

```
python osscmd listpart oss://mybucket/myobject --upload_id=
          75835E389EA648C0B93571B6A46023F3
```

## listparts

Command:

`listparts oss://bucket`

List the objects and upload IDs of multipart upload events that have not been completed for a bucket. When you want to delete a bucket but the system prompts that the bucket is not empty, you can run this command to check whether there are fragments in the bucket.

Example:

`python osscmd listparts oss://mybucket`

## getallpartsize

Command:

`getallpartsize oss://bucket`

List the total size of parts that are uploaded by using the existing upload IDs.

Example:

`python osscmd getallpartsize oss://mybucket`

## cancel

Command:

`cancel oss://bucket/object --upload_id=xxx`

Terminate the multipart upload event that uses the upload ID.

Example:

```
python osscmd cancel oss://mybucket/myobject --upload_id=
          D9D278DB6F8845E9AFE797DD235DC576
```

## multiupload\(multi\_upload,mp\)

Command:

```
multiupload(multi_upload,mp) localfile oss://bucket/object --check_md5=false
        --thread_num=10
```

Use multipart upload to upload a local file to OSS.

Example:

-   `python osscmd multiupload /tmp/localfile.txt oss://mybucket/object`
-   `python osscmd multiup_load /tmp/localfile.txt oss://mybucket/object`
-   `python osscmd mp /tmp/localfile.txt oss://mybucket/object`

Command:

```
multiupload(multi_upload,mp) localfile oss://bucket/object --upload_id=xxx --thread_num=10
        --max_part_num=1000 --check_md5=false
```

Use multipart upload to upload a local file to OSS. The part count of the local file is defined by the max\_part\_num parameter. When this command is run, the system first determines whether the MD5 value of ETags of parts that use the upload ID is the same with the MD5 value of the local file. If their values are the same, the parts are uploaded. Generate an upload ID before this upload event is started. Add the upload ID to the command. If the upload fails, you can run the same multiupload command to upload the parts in the same way you use resumable upload. `--check_md5=false` indicates that Content-MD5 is not included in the request header and MD5 verification will not be performed. --check\_md5=true indicates that MD5 verification will be performed.

Example:

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

Command:

```
copylargefile oss://source_bucket/source_object oss://target_bucket/target_object
        --part_size=10*1024*1024 --upload_id=xxx
```

To replicate an object that is larger than 1 GB, use multipart to replicate the object to the destination bucket. Ensure that the source bucket and destination bucket are in the same region. The upload\_id parameter is optional. If you need to resume the transmission of a multipart copy event, you can import the upload\_id parameter for the multipart copy event. The part\_size parameter is used to define the size of each part. A single part must be at least 100 KB in size. A maximum of 10,000 parts are supported for a multipart copy event. If the value of part\_size is smaller than 100 KB, the program automatically adjusts the part size.

Example:

```
python osscmd copylargefile oss://source_bucket/source_object
          oss://target_bucket/target_object --part_size=10*1024*1024
```

## uploadpartfromfile \(upff\)

Command:

```
uploadpartfromfile (upff) localfile oss://bucket/object --upload_id=xxx
        --part_number=xxx
```

This command is used for tests only.

## uploadpartfromstring\(upfs\)

Command:

```
uploadpartfromstring(upfs) oss://bucket/object --upload_id=xxx --part_number=xxx
        --data=xxx
```

This command is used for tests only.

