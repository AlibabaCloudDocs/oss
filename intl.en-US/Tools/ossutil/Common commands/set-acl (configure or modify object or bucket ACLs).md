# set-acl \(configure or modify object or bucket ACLs\)

Access control lists \(ACLs\) are policies used to manage the access permissions of buckets and objects. You can configure an ACL for a bucket when you create the bucket or for an object after you upload the object to Object Storage Service \(OSS\). You can also modify the ACLs of objects and buckets at any time. The set-acl command is used to configure or modify the ACLs of buckets or objects.

**Note:** Sample command lines in this topic are based on the 64-bit Linux system. For other systems, replace ./ossutil64 in the commands with the corresponding binary name. For more information, see [ossutil](/intl.en-US/Quick Start/ossutil.md).

## Configure or modify the ACL of a bucket

-   Command syntax

    ```
    ./ossutil64 set-acl oss://bucketname acl -b [--retry-times <value>]
    ```

    The following table describes the parameters that you can configure when you use this command to configure or modify the ACLs of buckets.

    |Parameter|Description|
    |---------|-----------|
    |bucketname|The name of the bucket for which you want to configure or modify ACL.|
    |acl|The ACL of the bucket. Valid values:    -   private: Only the bucket owner can perform read and write operations on objects in the bucket. Other users cannot access the objects in the bucket.
    -   public-read: Only the bucket owner can perform write operations on objects in the bucket. Other users, including anonymous users, can perform only read operations on the objects in the bucket. If you set the bucket ACL to this value, data leakage may occur and you may be charged additional fees. Exercise caution when you set the bucket ACL to this value.
    -   public-read-write: All users, including anonymous users, can perform read and write operations on the objects in the bucket. If you set the bucket ACL to this value, data leakage may occur and you may be charged additional fees. If a user writes illegal information to your objects, your legitimate interests and rights may be infringed. We recommend that you do not set the bucket ACL to this value except in special scenarios. |
    |-b|If you do not specify this parameter in the command, the ACL specified in the command is the ACL of objects. To use the command to configure bucket ACLs, you must specify this parameter.|
    |--retry-times|The number of retries after the command fails to be run. Default value: 10. Valid values: 1 to 500.|

-   Examples

    You can run the following command to set the ACL of a bucket named examplebucket to private:

    ```
    ./ossutil64 set-acl oss://examplebucket private -b   
    ```


## Configure or modify the ACLs of objects

-   Command syntax

    ```
    ./ossutil64 set-acl oss://bucketname[/prefix]acl 
    [-r]
    [--include <value>] 
    [--exclude <value>]
    [--version-id <value>]
    [--job <value>] 
    [--retry-times <value>]
    [--encoding-type <value>]
    ```

    The following table describes the parameters that you can configure when you use this command to configure or modify the ACLs of objects.

    |Parameter|Description|
    |---------|-----------|
    |bucketname|The name of the bucket in which the objects whose ACLs you want to configure or modify.|
    |prefix|The resources in the bucket, such as directories and objects.|
    |acl|The ACL of the objects. Default value: private. Valid values:    -   default: The ACL of the objects is the same as the ACL of the bucket in which the objects are stored.
    -   private: Only the bucket owner can perform read and write operations on objects in the bucket. Other users cannot access the objects in the bucket.
    -   public-read: Only the bucket owner can perform write operations on objects in the bucket. Other users, including anonymous users, can perform only read operations on the objects in the bucket. If you set the object ACL to this value, data leakage may occur and you may be charged additional fees. Exercise caution when you set the bucket ACL to this value.
    -   public-read-write: All users, including anonymous users, can perform read and write operations on the objects in the bucket. If you set the object ACL to this value, data leakage may occur and you may be charged additional fees. If a user writes illegal information to your objects, your legitimate interests and rights may be infringed. We recommend that you do not set the bucket ACL to this value except in special scenarios. |
    |-r|If you specify this parameter in the command, ossutil configures the ACL of all objects whose names contain the prefix specified by the prefix parameter. If you do not specify this parameter in the command, ossutil configures the ACL only of the object specified by cloud\_url.|
    |--include|Specifies the command to include all objects that meet the specified conditions.|
    |--exclude|Specifies that the command applies to all objects that do not meet the specified conditions.|
    |--version-id|The version ID of the object for which you want to configure ACL. This parameter applies only to objects in buckets for which versioning is enabled or suspended.|
    |--job|The number of concurrent tasks performed across multiple objects. Valid values: 1 to 10000. Default value: 3.|
    |--retry-times|The number of retries after the command fails to be run. Default value: 10. Valid values: 1 to 500.|
    |--encoding-type|Specifies the method used to encode the prefix specified by the object name that follows `oss://bucket_name` in the full object path. Valid value: url. If this parameter is not specified, the prefix is not encoded.|

-   Examples
    -   You can run the following command to set the ACL of an object named exampleobject.txt in a bucket named examplebucket to private:

        ```
        ./ossutil64 set-acl oss://examplebucket/exampleobject.txt private
        ```

    -   You can run the following command to set the ACL of a version of an object named exampleobject.txt in a bucket named examplebucket to private. The ID of the version is `CAEQARiBgID8rumR2hYiIGUyOTAyZGY2MzU5MjQ5ZjlhYzQzZjNlYTAyZDE3****`.

        ```
        ./ossutil64 set-acl oss://examplebucket/exampleobject.txt private --version-id CAEQARiBgID8rumR2hYiIGUyOTAyZGY2MzU5MjQ5ZjlhYzQzZjNlYTAyZDE3****
        ```

    -   You can run the following command to set the ACL of objects whose names contain the "test" prefix in a bucket named examplebucket to default:

        ```
        ./ossutil64 set-acl oss://examplebucket/test default -r
        ```

    -   You can run the following command to set the ACL of objects whose names contain the ".jpg" suffix in a bucket named examplebucket to private:

        ```
        ./ossutil64 set-acl oss://examplebucket private --include "*.jpg" -r
        ```

    -   You can run the following command to set the ACL of objects whose names contain the string "abc" and do not contain the ".png" and ".txt" suffixes in a bucket named examplebucket to default:

        ```
        ./ossutil64 set-acl oss://examplebucket default --include "*abc*" --exclude "*.png" --exclude "*.txt" -r
        ```


## Common options

To use ossutil to manage buckets that are located in different regions, you can use the -e option to use the endpoint of the specified bucket. To use ossutil to manage buckets that are owned by different Alibaba Cloud accounts, you can use the -i option to use the AccessKey ID of the specified account, and use the -k option to use the AccessKey secret of the specified account.

For example, you can run the following command to set the ACL of a bucket named examplebucket to private, which is located in the China \(Hangzhou\) region and owned by another Alibaba Cloud account.

```
./ossutil64 set-acl oss://testbucket private -b -e oss-cn-shanghai.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

For more information about other common options that apply to the set-acl command, see [Common options](/intl.en-US/Tools/ossutil/View options.md).

