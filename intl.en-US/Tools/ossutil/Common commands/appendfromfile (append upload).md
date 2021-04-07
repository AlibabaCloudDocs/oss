# appendfromfile \(append upload\)

The appendfromfile command is used to append content to the end of objects that are uploaded by using the AppendObject operation.

**Note:**

-   The commands described in this topic apply to 64-bit Linux systems. To use the commands in the following examples in 64-bit Windows systems, replace ./ossutil64 in the commands with ossutil64.exe.
-   For more information about append upload, see [Append upload](/intl.en-US/Developer Guide/Objects/Upload files/Append upload.md).

## Command syntax

```
./ossutil64 appendfromfile local\_file\_name oss://bucket\_name object\_name [--meta ]
```

The following table describes the options you can configure in the command.

|Option|Description|
|------|-----------|
|local\_file\_name|The complete path of the file you want to upload.|
|bucket\_name|The name of the bucket in which the object you want to append is stored.|
|object\_name|The name of the object to which you want to append content. When you upload an object, you can retain the original name of the uploaded file or specify another name.|
|--meta|Configures the metadata of the uploaded object. This option is supported only when the appendfromfile command is used to upload an object for the first time. Example: `--meta "x-oss-object-acl:private"`. After the metadata of an object is configured, you can use the [set-meta](/intl.en-US/Tools/ossutil/Common commands/set-meta.md) command to modify the metadata of the object. |

## Examples

In the following examples, the file named exampleobject.txt in the root directory is uploaded for the first time to a bucket named examplebucket by using append upload Then, append upload is used to append content to exampleobject.txt multiple times.

1.  Run the following command to upload the file named exampleobject.txt and set the ACL of the uploaded object to private:

    ```
    ./ossutil64 appendfromfile exampleobject.txt oss://examplebucket/exampleobject.txt --meta "x-oss-object-acl:private"
    ```

    If the following results are displayed, it indicates that exampleobject.txt is uploaded to the bucket and is 5 bytes in size.

    ```
    total append 5(100.00%) byte,speed is 0.00(KB/s)
    local file size is 5,the object new size is 5,average speed is 0.04(KB/s)
    ```

2.  Run the following command to append the content of the file named dest.txt to exampleobject.txt.

    If you want to append more content to exampleobject.txt, run the following command but with dest.txt changed to file name whose content you want to append to exampleobject.txt.

    ```
    ./ossutil64 appendfromfile dest.txt oss://examplebucket/exampleobject.txt
    ```

    If the following results are displayed, it indicates that content is appended to exampleobject.txt. The size of exampleobject.txt becomes 150 bytes after the append upload.

    ```
    total append 150(100.00%) byte,speed is 0.00(KB/s)
    local file size is 150,the object new size is 150,average speed is 1.19(KB/s)
    ```


## Common options

To use ossutil to manage buckets in different regions, add the -e option followed by the endpoint of the region in which the bucket resides To use ossutil to manage buckets owned by different Alibaba Cloud accounts, add the -i option to commands to use the AccessKey ID of the specified Alibaba Cloud account and add the -k option to use the AccessKey secret of the specified Alibaba Cloud account.

For example, you can run the following command to use append upload to upload an object named exampleobject.txt to a bucket named examplebucket, which is owned by another Alibaba Cloud account in the China \(Shanghai\) region.

```
./ossutil64 appendfromfile exampleobject.txt oss://examplebucket/exampleobject.txt -e shanghai.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

For more information about other common options that apply to the appendfromfile command, see [Common options](/intl.en-US/Tools/ossutil/View options.md).

