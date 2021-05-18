# du \(query object sizes\)

The du command is used to obtain the total size of all objects within a bucket or directory.

**Note:** Sample command lines in this topic are based on the 64-bit Linux system. For other systems, replace ./ossutil64 in the commands with the corresponding binary name. For more information, see [ossutil](/intl.en-US/Quick Start/ossutil.md).

## Command syntax

```
./ossutil64 du oss://bucketname[/prefix] [--payer requester] [--all-versions][--block-size <value>]
```

The following table describes the parameters that you can configure in the du command.

|Parameter|Description|
|---------|-----------|
|bucketname|The name of the bucket in which the objects whose total size you want to query are stored.|
|prefix|The path of a directory in which the objects whose total size you want to query are stored, or a prefix that is contained in the names of all objects whose size you want to query.|
|--payer|The payer of the traffic and request fees incurred during queries. If you want that the traffic and request fees incurred during queries are paid by the requester who accesses the resources in the specified path, set this parameter to requester.|
|--all-versions|Specifies whether to query the total size of all versions of objects. By default, if you do not specify this parameter in the command, the total size of only the current versions of objects is queried.|
|--block-size|The unit of the obtained total size of objects within the specified bucket or directory. Valid values: KB, MB, GB, and TB. By default, if you do not specify this parameter in the command, the obtained total size of objects is measured in bytes. **Note:** This parameter applies to ossutil 1.7.3 and later versions. |

## Query the total size of all versions of objects in the specified bucket

You can run the following command to query the total size of all versions of objects in a bucket named examplebucket:

```
./ossutil64 du oss://examplebucket --all-versions
```

The following output results are returned. The results show that 13 objects of 132,116,024 bytes in size are stored in examplebucket, in which 12 objects are Standard objects and one object is an Archive object.

```
storage class   object count            sum size(byte)
----------------------------------------------------------
Standard        12                       132115210
Archive         1                        814
----------------------------------------------------------
total object count: 13                          total object sum size: 132116024
total part count:   0                           total part sum size:   0

total du size(byte):132116024

0.382978(s) elapsed
```

## Query the total size of the current versions of objects in the specified bucket

You can run the following command to query the total size of the current versions of objects in the dir directory in a bucket named examplebucket. The obtained size is measured in GB.

```
./ossutil64 du oss://examplebucket/dir/  --block-size GB
```

The following output results are returned. The results show that five Standard objects of 0.0002 GB in size are stored in the dir directory in examplebucket.

```
storage class   object count            sum size(byte)
----------------------------------------------------------
Standard        5                       232277
----------------------------------------------------------
total object count: 5                           total object sum size: 232277
total part count:   0                           total part sum size:   0

total du size(GB):0.0002

0.078757(s) elapsed
```

## Query the total size of all version of objects whose names contain the specified prefix

You can run the following command to query the total size of all versions of objects whose names contain the "test" prefix in a bucket named examplebucket. The obtained size is measured in KB.

```
./ossutil64 du oss://examplebucket/test --all-versions --block-size KB
```

The following output results are returned. The results show that four Standard objects whose names contain the "test" prefix are stored in examplebucket and the total size of these objects is 448.1455 KB.

```
storage class   object count            sum size(byte)
----------------------------------------------------------
Standard        4                       439425
----------------------------------------------------------
total object count: 4                           total object sum size: 439425
total part count:   0                           total part sum size:   0

total du size(KB):448.1455

0.126340(s) elapsed
```

## Common options

To use ossutil to manage buckets that are located in different regions, you can use the -e option to use the endpoint of the specified bucket. To use ossutil to manage buckets that are owned by multiple Alibaba Cloud accounts, you can use the -i option to use the AccessKey ID of the specified account, and use the -k option to use the AccessKey secret of the specified account.

For example, you can run the following command to query the total size of all versions of objects in a bucket named testbucket, which is located in the China \(Shanghai\) region and owned by another Alibaba Cloud account:

```
./ossutil64 du oss://testbucket --all-versions -e oss-cn-shanghai.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

For more information about other common options, see [Common options](/intl.en-US/Tools/ossutil/View options.md).

