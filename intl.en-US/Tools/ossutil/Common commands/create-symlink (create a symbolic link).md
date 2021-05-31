# create-symlink \(create a symbolic link\)

Symbolic links can be used to access objects that are commonly used in buckets. The create-symlink command is run to create a symbolic link. After you create a symbolic link for an object, you can use the symbolic link to access the object. Symbolic links work in a similar manner to shortcuts in Windows.

**Note:** Sample command lines in this topic are based on the 64-bit Linux system. For other systems, replace ./ossutil64 in the commands with the corresponding binary name. For more information, see [ossutil](/intl.en-US/Quick Start/ossutil.md).

## Command syntax

```
./ossutil64 create-symlink cloud\_url target\_object [--encoding-type <value>] [--payer <value>]
```

The following table describes the parameters that you can configure in this command.

|Parameter|Description|
|---------|-----------|
|cloud\_url|The full path of the symbolic link you want to create.|
|target\_object|The full path of the object to which the symbolic link points. A symbolic link and the object to which the symbolic link points must be in the same bucket.|
|--encoding-type|The method used to encode the object names specified in `cloud_url` and `target_object`. Valid value: url. If you do not specify this parameter, the object names are not encoded.|
|--payer|The payer of the traffic and request fees incurred when the command is run. If you want that the requester who accesses the resources in the specified path charged for the traffic and request fees incurred when the command is run, set this parameter to requester.|

## Examples

When you run this command to create a symbolic link, ossutil does not check whether the object to which the symbolic link points to exists. If the object exists, the created symbolic link can access the object. If the object does not exist, the created symbolic link cannot access the object. To determine whether the object to which a symbolic link points exists, run the [ls](/intl.en-US/Tools/ossutil/Common commands/ls (list buckets, objects, or parts).md) command to query all objects in the bucket.

The following examples show how to create a symbolic link that points to an existing object.

**Note:** If the name of the symbolic link you want to create is the same as that of an existing symbolic link, the existing symbolic link is overwritten.

-   Create a symbolic link named test.jpg in the root directory of a bucket named examplebucket and point the symbolic link to an object named exampleobject.jpg in the root directory of examplebucket.

    ```
    ./ossutil64 create-symlink  oss://examplebucket/test.jpg  oss://examplebucket/exampleobject.jpg
    ```

-   Create a symbolic link named example.jpg in the destfolder directory of a bucket named examplebucket and point the symbolic link to an object named test.jpg in the root directory of examplebucket. Specify that all fees incurred when the command is run are paid by the requester.

    ```
    ./ossutil64 create-symlink  oss://examplebucket/destfolder/example.jpg  oss://examplebucket/test.jpg --payer requester
    ```

-   If a similar output is displayed, the symbolic link is created.

    ```
    0.106744(s) elapsed
    ```

    After you create a symbolic link, you can run the [read-symlink](/intl.en-US/Tools/ossutil/Common commands/read-symlink.md) or [stat](/intl.en-US/Tools/ossutil/Common commands/stat.md) command to query the information about the symbolic link, such as the ETag value and last update time.


## Common options

To use ossutil to manage buckets that are located in different regions, you can use the -e option to use the endpoint of the specified bucket. To use ossutil to manage buckets that are owned by multiple Alibaba Cloud accounts, you can use the -i option to use the AccessKey ID of the specified account, and use the -k option to use the AccessKey secret of the specified account.

For example, you can run the following command to create a symbolic link named testobject.png that points to an object named exampleobject.png in a bucket named testbucket, which is located in the China \(Shanghai\) region and owned by another Alibaba Cloud account:

```
./ossutil64 create-symlink  oss://testbucket/testobject.png  oss://testbucket/exampleobject.png -e oss-cn-shanghai.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

For more information about other common options, see [Common options](/intl.en-US/Tools/ossutil/View options.md).

