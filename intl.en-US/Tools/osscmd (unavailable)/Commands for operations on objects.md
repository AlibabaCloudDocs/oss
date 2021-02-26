# Commands for operations on objects

This topic describes commands that can be used to manage objects.

**Note:** Commands supported by the osscmd tool have been integrated with the [ossutil](/intl.en-US/Tools/ossutil/Overview.md) tool. The osscmd tool is no longer available for download as of July 31, 2019.

## ls\(list\)

Command:

`ls(list) oss://bucket/[prefix] [marker] [delimiter] [maxkeys]`

List objects in a bucket. You can specify a prefix to list all objects whose names start with the specified prefix. For example, you can specify abc as the prefix to list all objects whose names start with abc.

Example:

-   `python osscmd ls oss://mybucket/folder1/folder2`
-   `python osscmd ls oss://mybucket/folder1/folder2 marker1`
-   `python osscmd ls oss://mybucket/folder1/folder2 marker1 /`
-   `python osscmd ls oss://mybucket/`
-   `python osscmd list oss://mybucket/ "" "" 100`

Command:

```
ls(list) oss://bucket/[prefix] --marker=xxx --delimiter=xxx --maxkeys=xxx
        --encoding_type=url
```

List objects in a bucket. You can set encoding\_type to specify the encoding method that is used during transmission. If you set encoding\_type to url, objects whose names contain control characters are encoded.

Example:

-   `python osscmd ls oss://mybucket/folder1/folder2 --delimiter=/`
-   `python osscmd ls oss://mybucket/folder1/folder2 --marker=a`
-   `python osscmd ls oss://mybucket/folder1/folder2 --maxkeys=10`

## mkdir

Command:

`mkdir oss://bucket/dirname`

Create a folder.

Example:

`python osscmd mkdir oss://mybucket/folder`

## listallobject

Command:

`listallobject oss://bucket/[prefix]`

List all objects in a bucket. You can specify a prefix to list objects whose names start with the prefix.

Example:

-   `python osscmd listallobject oss://mybucket`
-   `python osscmd listallobject oss://mybucket/testfolder/`

## deleteallobject

Command:

`deleteallobject oss://bucket/[prefix]`

Delete all objects in a bucket. You can also specify a prefix to delete objects whose names start with the prefix.

Example:

-   `python osscmd deleteallobject oss://mybucket`
-   `python osscmd deleteallobject oss://mybucket/testfolder/`

## downloadallobject

Command:

```
downloadallobject oss://bucket/[prefix] localdir --replace=false
      --thread_num=5
```

Download objects from a bucket to a local directory. This operation ensures that the original directory structure remains the same. You can specify a prefix to download objects whose names start with the specified prefix. `--replace=false` indicates that local files with the same name of the object will not be overwritten during the download. `--replace=true` indicates that local files with the same name of the object will be overwritten. You can also use thread\_num to configure the download thread.

Example:

-   `python osscmd downloadallobject oss://mybucket /tmp/folder`
-   ```
python osscmd downloadallobject oss://mybucket /tmp/folder
        –-replace=false
```

-   ```
python osscmd downloadallobject oss://mybucket /tmp/folder –-replace=true
          --thread_num=5
```


## downloadtodir

Command:

`downloadtodir oss://bucket/[prefix] localdir --replace=false`

Download objects from a bucket to a local directory. This operation ensures that the original directory structure remains the same. You can specify a prefix to download objects whose names start with the specified prefix. `--replace=false` indicates that local files with the same name of the object will not be overwritten during the download. `--replace=true` indicates that local files with the same name of the object will be overwritten. downloadtodir follows the same logic as that of downloadallobject.

Example:

-   `python osscmd downloadtodir oss://mybucket /tmp/folder`
-   `python osscmd downloadtodir oss://mybucket /tmp/folder –-replace=false`
-   ```
python osscmd downloadtodir oss://mybucket /tmp/folder
      –-replace=true
```


## uploadfromdir

Command:

```
uploadfromdir localdir oss://bucket/[prefix] --check_point=check_point_file --replace=false
        --check_md5=false --thread_num=5
```

Upload local files to a bucket.

If local directory `/tmp/` contains the a/b, a/c, and a files, the paths of these files in OSS are oss://bucket/a/b, oss://bucket/a/c, and oss://bucket/a. If a prefix is set to mytest, the paths of these files in OSS are oss://bucket/mytest/a/b, oss://bucket/mytest/a/c, and oss://bucket/mytest/a.

`--check_point=check_point_file` is used to specify a checkpoint file. After the checkpoint file is specified, the osscmd tool will be used to store the timestamps that are recorded when the local files are uploaded. The uploadfromdir command is used to compare the timestamps of the files that are being uploaded and the timestamps that are recorded in the checkpoint file. If the timestamps are different, the files are reuploaded. check\_point\_file is not specified by default. --replace=false indicates that local files with the same name of the object will not be overwritten during the upload. `--replace=true` indicates that local files with the same name of the object will be overwritten. `--check_md5=false` indicates that Content-MD5 is not included in the request header and MD5 verification will not be performed. --check\_md5=true indicates that MD5 verification will be performed.

> Note: The checkpoint file stores upload records of all objects.

Example:

-   `python osscmd uploadfromdir /mytemp/folder oss://mybucket`
-   ```
python osscmd uploadfromdir /mytemp/folder oss://mybucket
          --check_point_file=/tmp/mytemp_record.txt
```

-   ```
python osscmd uploadfromdir C:\Documents and Settings\User\My Documents\Downloads
          oss://mybucket --check_point_file=C:\cp.txt
```


## put

Command:

```
put localfile oss://bucket/object --content-type=[content_type]
        --headers="key1:value1#key2:value2" --check_md5=false
```

When uploading a local file to a bucket, you can set HTTP header fields such as content-type. `--check_md5=false` indicates that Content-MD5 is not included in the request header and MD5 verification will not be performed. --check\_md5=true indicates that MD5 verification will be performed.

Example:

-   `python osscmd put myfile.txt oss://mybucket`
-   `python osscmd put myfile.txt oss://mybucket/myobject.txt`
-   ```
python osscmd put myfile.txt oss://mybucket/test.txt --content-type=plain/text
          --headers=“x-oss-meta-des:test#x-oss-meta-location:CN”
```

-   ```
python osscmd put myfile.txt oss://mybucket/test.txt
        --content-type=plain/text
```


## upload

Command:

```
upload localfile oss://bucket/object --content-type=[content_type]
      --check_md5=false
```

Upload local files to a bucket. `--check_md5=false` indicates that Content-MD5 is not included in the request header and MD5 verification will not be performed. --check\_md5=true indicates that MD5 verification will be performed.

Example:

```
python osscmd upload myfile.txt oss://mybucket/test.txt
        --content-type=plain/text
```

## get

Command:

`get oss://bucket/object localfile`

Download an object to a local file.

Example:

`python osscmd get oss://mybucket/myobject /tmp/localfile`

## multiget\(multi\_get\)

Command:

`multiget(multi_get) oss://bucket/object localfile --thread_num=5`

Use multithreading to download an object to a local file. You can configure the number of threads that are used to download the object.

Example:

-   `python osscmd multiget oss://mybucket/myobject /tmp/localfile`
-   `python osscmd multi_get oss://mybucket/myobject /tmp/localfile`

## cat

Command:

`cat oss://bucket/object`

Read and display object content. Do not run this command if the object is large.

Example:

`python osscmd cat oss://mybucket/myobject`

## meta

Command:

`meta oss://bucket/object`

Read and display the meta information of the object. Meta information contains the content-type, file length, and user metadata.

Example:

`python osscmd meta oss://mybucket/myobject`

## copy

Command:

```
copy oss://source_bucket/source_object oss://target_bucket/target_object
        --headers="key1:value1#key2:value2"
```

Replicate an object from a source bucket to a destination bucket.

Example:

`python osscmd copy oss://bucket1/object1 oss://bucket2/object2`

## rm\(delete,del\)

Command:

`rm(delete,del) oss://bucket/object --encoding_type=url`

Delete an object. When encoding-type is set to url, control characters to be deleted also need to be URL-encoded.

Example:

-   `python osscmd rm oss://mybucket/myobject`
-   `python osscmd delete oss://mybucket/myobject`
-   `python osscmd del oss://mybucket/myobject`
-   `python osscmd del oss://mybucket/my%01object --encoding_type=url`

## signurl\(sign\)

Command:

`signurl(sign) oss://bucket/object --timeout=[timeout_seconds]`

Generate a signed URL containing the timeout value. A signed URL is used to provide access to a specific object when the bucket ACL is private.

Example:

-   `python osscmd sign oss://mybucket/myobject`
-   `python osscmd signurl oss://mybucket/myobject`

