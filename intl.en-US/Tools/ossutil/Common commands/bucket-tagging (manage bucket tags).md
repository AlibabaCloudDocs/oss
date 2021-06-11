# bucket-tagging \(manage bucket tags\)

OSS allows you to configure bucket tagging to classify and manage buckets. For example, you can use this feature to list buckets that have specific tags and configure access control lists \(ACLs\) for buckets that have specific tags. The bucket-tagging command is used to add, modify, query, or delete tagging configurations for a bucket.

**Note:**

-   The commands described in this topic are for 64-bit Linux systems. To use the commands in the examples on 64-bit Windows systems, replace ./ossutil64 in the commands with ossutil64.exe.
-   For more information about bucket tagging, see [Bucket tagging](/intl.en-US/Developer Guide/Buckets/Bucket tagging.md).

## Add tags to a bucket or modify the tags of a bucket

Bucket tagging uses a key-value pair as a tag to identify buckets. Each bucket can have up to 10 tags. Only the bucket owner and users who are granted the PutBucketTags permission can add tags to the bucket or modify the tags of the bucket. If other users add tags to the bucket or modify tags of the bucket, 403 Forbidden is returned with error code AccessDenied.

-   Command syntax

    ```
    ./ossutil64 bucket-tagging --method put oss://bucketname key\#value
    ```

    The following table describes the parameters that you can configure when you run this command.

    |Parameter|Description|
    |---------|-----------|
    |bucketname|The name of the bucket to which you want to add a tag or of which you want to modify a tag.|
    |key\#value|The key-value pair that contains the tag information.     -   The key and value of the tag are separated by a number sign \(\#\). The key and value of the tag must be encoded in UTF-8.
    -   Each tag must have a key. The key of a tag can be up to 64 bytes in length and cannot start with `http://`, `https://`, or `Aliyun`.
    -   The value of a tag can be up to 128 bytes in length and can be empty. |

    If a bucket has no tags, you can run this command to add tags to the bucket. If a bucket has tags, you can run this command to overwrite existing tags.

-   Examples

    You can run the following command to add two tags to a bucket named examplebucket. One tag has a key of tag1 and a value of test1, and the other tag has a key of tag2 and a value of test2.

    ```
    ./ossutil64 bucket-tagging --method put oss://examplebucket  tag1#test1 tag2#test2
    ```

    If a similar output is displayed, the tags are added to the bucket:

    ```
    0.300600(s) elapsed
    ```


## Query bucket tags

-   Command syntax

    ```
    ./ossutil64 bucket-tagging --method get oss://bucketname
    ```

-   Examples

    You can run the following command to query the tags of a bucket named examplebucket:

    ```
    ./ossutil64 bucket-tagging --method get oss://examplebucket
    ```

    If the following results are displayed, it indicates that two tags are configured to examplebucket. One tag has a key of tag1 and a value of test1, and the other tag has a key of tag2 and a value of test2.

    ```
    index     tag key       tag value
    ---------------------------------------------------
    0         "tag1"        "test1"
    1         "tag2"        "test2"
    
    0.283359(s) elapsed
    ```


## Remove bucket tags

-   Command syntax

    ```
    ./ossutil64 bucket-tagging --method delete oss://bucketname 
    ```

-   Examples

    You can run the following command to remove all tags of a bucket named examplebucket:

    ```
    ./ossutil64 bucket-tagging --method delete oss://examplebucket
    ```

    If a similar output is displayed, all tags of examplebucket are removed:

    ```
    0.530750(s) elapsed
    ```


## Common options

To use ossutil to manage buckets that are located in different regions, you can use the -e option to use the endpoint of the specified bucket. To use ossutil to manage buckets that are owned by different Alibaba Cloud accounts, you can use the -i option to use the AccessKey ID of the specified account, and use the -k option to use the AccessKey secret of the specified account.

For example, you can run the following command to configure tags for a bucket named examplebucket, which is located in the China \(Hangzhou\) region and owned by another Alibaba Cloud account:

```
./ossutil64 bucket-tagging--method put oss://examplebucket key#value -e oss-cn-hangzhou.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

For more information about other common options that apply to the bucket-tagging command, see [Common options](/intl.en-US/Tools/ossutil/View options.md).

