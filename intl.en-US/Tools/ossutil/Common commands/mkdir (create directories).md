# mkdir \(create directories\)

If you want to organize objects that you upload to a bucket, you can create directories. This topic describes how to use the mkdir command to create directories in ossutil.

**Note:** Sample command lines in this topic are based on the 64-bit Linux system. For other systems, replace ./ossutil64 in the commands with the corresponding binary name. For more information, see [ossutil](/intl.en-US/Quick Start/ossutil.md).

## Command syntax

```
./ossutil64 mkdir oss://bucket\_name dir\_name [--encoding-type <value>]
```

The following table describes the parameters that you can configure in this command.

|Parameter|Description|
|---------|-----------|
|bucket\_name|The name of the bucket in which you want to create directories.|
|dir\_name|The name of the directory that you want to create. A directory name must end with a forward slash \(/\). If you do not end the directory name with a forward slash \(/\), ossutil automatically adds one at the end of the specified directory name.|
|--encoding-type|The encoding type used to encode the directory name, which is specified by the part following `oss://bucket_name` in the value of dirname. Valid value: url. If you do not specify this parameter, the directory name is not encoded.|

## Examples

You can perform the following steps to upload an object to a specified directory.

1.  Create a directory.
    -   Create a single-level directory

        ```
        ./ossutil mkdir oss://examplebucket/dir/
        ```

        If the similar output is returned, a directory named dir/ is created in the bucket named examplebucket.

        ```
        0.385877(s) elapsed
        ```

    -   Create a multi-level directory

        To finely classify the objects stored in directories, you can create multi-level directories. For example, you can run the following command to create a multi-level directory oss://examplebucket/Photo/2021/ to store the snapshots that are generated in 2021:

        ```
        ./ossutil mkdir oss://examplebucket/Photo/2021/ 
        ```

        If you accidentally delete the 2021/ directory and no other objects are stored in the Photo/ directory, the Photo/ directory is also deleted.

2.  Upload an object to a directory

    You can run the following command to upload an object named exampleobject.txt to the dir/ directory in a bucket named examplebucket:

    ```
    ./ossutil cp exampleobject.txt oss://examplebucket/dir/
    ```

    If the similar output is returned, the object is uploaded to the specified directory.

    ```
    Succeed: Total num: 1, size: 0. OK num: 1(upload 1 files).
    
    average speed 0(byte/s)
    ```


## Common options

To use ossutil to manage buckets that are located in different regions, you can use the -e option to use the endpoint of the specified bucket. To use ossutil to manage buckets that are owned by multiple Alibaba Cloud accounts, you can use the -i option to use the AccessKey ID of the specified account, and use the -k option to use the AccessKey secret of the specified account.

For example, you can run the following command to create a directory named dir/ in a bucket named examplebucket that is located in the China \(Hangzhou\) region and owned by another Alibaba Cloud account:

```
./ossutil64 mkdir oss://examplebucket/dir/ -e oss-cn-hangzhou.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

For more information about other common options that apply to the mkdir command, see [Common options](/intl.en-US/Tools/ossutil/View options.md).

