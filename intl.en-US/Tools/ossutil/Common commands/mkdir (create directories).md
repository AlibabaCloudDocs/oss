# mkdir \(create directories\)

To organize objects in a bucket, you can use directories. This topic describes how to use the mkdir command to create directories in ossutil.

**Note:** Sample command lines in this topic are based on the 64-bit Linux system. For other systems, replace ./ossutil64 in the commands with the corresponding binary name. For more information, see [ossutil](/intl.en-US/Quick Start/ossutil.md).

## Command syntax

```
./ossutil64 mkdir oss://bucketname dirname [--encoding-type <value>]
```

The following table describes the parameters that you can configure when you run this command.

|Parameter|Description|
|---------|-----------|
|bucketname|The name of the bucket in which you want to create directories.|
|dirname|The name of the directory that you want to create. A directory name must end with a forward slash \(/\). If you do not end the directory name with a forward slash \(/\), ossutil automatically adds one at the end.|
|--encoding-type|The encoding type used to encode the directory name, which is specified by the part following `oss://bucket_name` in the value of dirname. Valid value: url. If you do not specify this parameter, the directory name is not encoded.|

## Examples

You can perform the following steps to upload an object to a specified directory:

1.  Create a directory.
    -   Create a single-level directory

        ```
        ./ossutil mkdir oss://examplebucket/dir/
        ```

        If a similar output is returned, a directory named dir/ is created in the bucket named examplebucket.

        ```
        0.385877(s) elapsed
        ```

    -   Create a multi-level directory

        To further classify objects stored in a directory, you can create multi-level directories. For example, you can run the following command to create a subdirectory that is named 2021/ within the Photo/ directory to store images that are generated in 2021:

        ```
        ./ossutil mkdir oss://examplebucket/Photo/2021/ 
        ```

        When you delete a subdirectory, the parent directory is also deleted if it does not contain any objects.

2.  Upload an object to a directory

    You can run the following command to upload an object named exampleobject.txt to the dir/ directory in a bucket named examplebucket:

    ```
    ./ossutil cp exampleobject.txt oss://examplebucket/dir/
    ```

    If a similar output is returned, the object is uploaded to the specified directory.

    ```
    Succeed: Total num: 1, size: 0. OK num: 1(upload 1 files).
    
    average speed 0(byte/s)
    ```


## Common options

When you use ossutil to manage buckets that are located in different regions, you can use the -e option to use the endpoint of the specified bucket. When you use ossutil to manage buckets that are owned by different Alibaba Cloud accounts, you can use the -i option to use the AccessKey ID of the specified account, and use the -k option to use the AccessKey secret of the specified account.

For example, you can run the following command to create a directory named dir/ in a bucket named examplebucket, which is located in the China \(Hangzhou\) region and owned by another Alibaba Cloud account:

```
./ossutil64 mkdir oss://examplebucket/dir/ -e oss-cn-hangzhou.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

For more information about the mkdir command, see [Common options](/intl.en-US/Tools/ossutil/View options.md).

