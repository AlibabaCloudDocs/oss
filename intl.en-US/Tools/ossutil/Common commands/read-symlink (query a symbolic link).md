# read-symlink \(query a symbolic link\)

The read-symlink command is used to query the information about a symbolic link, including the ETag and last update time. To run this command to query the information about a symbolic link, you must have read permissions on the symbolic link.

**Note:** Sample command lines in this topic are based on the 64-bit Linux system. For other systems, replace ./ossutil64 in the commands with the corresponding binary name. For more information, see [ossutil](/intl.en-US/Quick Start/ossutil.md).

## Command syntax

```
./ossutil64 read-symlink oss://bucketname/objectname [--encoding-type <value>] [--payer <value>]
```

The following table describes the parameters that you can configure in this command.

|Parameter|Description|
|---------|-----------|
|bucketname|The name of the bucket in which the symbolic link you want to query is stored.|
|objectname|The name of the symbolic link you want to query.|
|--encoding-type|The method used to encode the name of the symbolic link. Valid value: url. If you do not specify this parameter, the name of the symbolic link is not encoded.|
|--payer|The payer of the traffic and request fees incurred when the command is run. If you want the requester who accesses the resources in the specified path to pay for the traffic and request fee incurred during queries, set this parameter to requester.|

## Examples

Query the information about a symbolic link named test.jpg in a bucket named examplebucket.

```
./ossutil64 read-symlink oss://examplebucket/test.jpg
```

The following output result indicates that the information about test.jpg is obtained, including the ETag value, last update time, and the object to which the symbolic link points. According to the result, test.jpg points to an object named example.jpg.

```
Etag                    : 938F26218CE422CBEEE0B6543A2B2D
Last-Modified           : 2021-04-21 18:00:13 +0800 CST
X-Oss-Symlink-Target    : example.jpg
0.217317(s) elapsed
```

If the object that you query is not a symbolic link, `NotSymlink` is returned.

## Common options

To use ossutil to manage buckets that are located in different regions, you can use the -e option to use the endpoint of the specified bucket. To use ossutil to manage buckets that are owned by multiple Alibaba Cloud accounts, you can use the -i option to use the AccessKey ID of the specified account, and use the -k option to use the AccessKey secret of the specified account.

For example, you can run the following command to query the information about a symbolic link named testobject.png in a bucket named testbucket, which is located in the China \(Shanghai\) region and owned by another Alibaba Cloud account:

```
./ossutil64 read-symlink oss://testbucket/testobject.png -e oss-cn-shanghai.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

For more information about other common options, see [Common options](/intl.en-US/Tools/ossutil/View options.md).

