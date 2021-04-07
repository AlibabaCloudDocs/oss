# bucket-tagging

OSS allows you to configure bucket tagging to classify and manage buckets. For example, you can use this feature to list buckets that have specific tags and configure access control lists \(ACLs\) for buckets that have specific tags. The bucket-tagging command is used to add, modify, query, or delete tagging configurations for a bucket.

**Note:**

-   The commands involved in this topic are based on 64-bit Linux systems. If you use a 64-bit Windows system, replace ./ossutil64 in the following examples with ossutil64.exe.
-   For more information about bucket tagging, see [Configure bucket tagging](/intl.en-US/Developer Guide/Buckets/Configure bucket tagging.md).

## Add tags to a bucket or modify tags of a bucket.

Bucket tagging uses a key-value pair to identify buckets. Each bucket can have up to 10 tags. Only the bucket owner and users who are granted the PutBucketTags permission can add tags to the bucket or modify tags of the bucket. If other users add tags to the bucket or modify tags of the bucket, 403 Forbidden is returned with error code AccessDenied.

-   Command syntax

    ```
    ./ossutil64 bucket-tagging --method put oss://bucket\_name key\#value
    ```

    The following table describes the parameters you can configure in the command.

    |Parameter|Description|
    |---------|-----------|
    |bucket\_name|The name of the bucket to which you want to add a tag or of which you want to modify a tag.|
    |key\#value|The key-value pair of the tag.     -   The key and value of the tag are separated with a number sign \(\#\). The key and value of the tag must be encoded in UTF-8.
    -   The key of the tag can be a maximum of 64 bytes in length. The key cannot be null and cannot start with `http://`, `https://`, or `Aliyun`.
    -   The value of the tag can be a maximum of 128 bytes in length and can be null. |

    If a bucket has no tags, you can run this command to add tags to the bucket. If a bucket has tags, you can run this command to overwrite existing tags.

-   Examples

    A bucket named examplebucket is configured with two tags. One tag has the key of tag1 and value of test1, and the other tag has the key of tag2 and value of test2.

    ```
    ./ossutil64 bucket-tagging --method put oss://examplebucket  tag1#test1 tag2#test2
    ```

    If a similar output is displayed, the bucket tags are added:

    ```
    0.300600(s) elapsed
    ```


## Query bucket tags

-   Command syntax

    ```
    ./ossutil64 bucket-tagging --method get oss://bucket\_name
    ```

-   Examples

    The following code provides an example on how to query the tagging configurations of examplebucket:

    ```
    ./ossutil64 bucket-tagging --method get oss://examplebucket
    ```

    If a similar output is displayed, examplebucket is configured with two tags. One tag has the key of tag1 and value of test1, and the other tag has the key of tag2 and value of test2.

    ```
    index     tag key       tag value
    ---------------------------------------------------
    0         "tag1"        "test1"
    1         "tag2"        "test2"
    
    0.283359(s) elapsed
    ```


## Delete bucket tags

-   Command syntax

    ```
    ./ossutil64 bucket-tagging --method delete oss://bucket\_name 
    ```

-   Examples

    The following code provides an example on how to delete all tagging information of examplebucket:

    ```
    ./ossutil64 bucket-tagging --method delete oss://examplebucket
    ```

    If a similar output is displayed, all tagging information of examplebucket is deleted:

    ```
    0.530750(s) elapsed
    ```


## Common options

When you use ossutil to manage buckets across regions, you can add the -e option to use the endpoint of the region in which the specified bucket is located. When you use ossutil to manage buckets owned by multiple Alibaba Cloud accounts, you can add the -i option to commands to use the AccessKey ID of the specified Alibaba Cloud account and add the -k option to use the AccessKey secret of the specified Alibaba Cloud account.

For example, you can run the following command to configure bucket tagging for a bucket named examplebucket, which is owned by another Alibaba Cloud account in the China \(Hangzhou\) region:

```
./ossutil64 bucket-tagging--method put oss://examplebucket key#value -e oss-cn-hangzhou.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

For more information about other common options that apply to the bucket-tagging command, see [Common options](/intl.en-US/Tools/ossutil/View options.md).

