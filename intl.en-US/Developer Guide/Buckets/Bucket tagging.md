# Bucket tagging

Object Storage Service \(OSS\) allows you to classify and manage buckets by using bucket tags. You can use this feature to list buckets that have specific tags and configure access control lists \(ACLs\) for buckets that have specific tags.

The bucket tagging feature uses a key-value pair to identify a bucket. You can add tags to buckets that are used for different purposes and manage the buckets by tags.

-   Only the owner of a bucket or authorized RAM users can configure tags for the bucket. Otherwise, 403 Forbidden is returned with the error code AccessDenied.
-   You can configure up to 20 tags for a bucket.
-   Each tag must have a key. The key of a tag can be up to 64 bytes in length and cannot start with `http://`, `https://`, or `Aliyun`.
-   The value of a tag can be up to 128 bytes in length and can be empty.
-   The key and value of a tag must be UTF-8-encoded.

## Implementation methods

|Implementation method|Description|
|---------------------|-----------|
|[Console](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure bucket tagging.md)|A user-friendly and intuitive web application|
|[ossutil](/intl.en-US/Tools/ossutil/Common commands/bucket-tagging.md)|A high-performance command-line tool|
|[OSS SDK for Java](/intl.en-US/SDK Reference/Java/Buckets/Bucket tagging.md)|SDK demos for various programming languages|
|[OSS SDK for Python](/intl.en-US/SDK Reference/Python/Buckets/Bucket tagging.md)|
|[OSS SDK for Go](/intl.en-US/SDK Reference/Go/Buckets/Bucket tagging.md)|
|[OSS SDK for C++](/intl.en-US/SDK Reference/C++/Buckets/Bucket tagging.md)|
|[OSS SDK for Node.js](/intl.en-US/SDK Reference/Node. js/Buckets/Bucket tagging.md)|
|[OSS SDK for .NET](/intl.en-US/SDK Reference/. NET/Buckets/Bucket tagging.md)|

## Usage notes

After you add tags to buckets, you can manage multiple buckets that have the same tag. For example, you can list buckets that have the same tag and authorize RAM users to manage buckets that have the same tag.

-   Lists buckets that have specific tags

    You can list buckets that have specific tags. For more information, see the following SDK demos:

    -   [OSS SDK for Java](/intl.en-US/SDK Reference/Java/Buckets/Bucket tagging.mdsection_nt0_gl3_aey)
    -   [OSS SDK for Python](/intl.en-US/SDK Reference/Python/Buckets/Bucket tagging.mdsection_nt0_gl3_aey)
    -   [OSS SDK for Go](/intl.en-US/SDK Reference/Go/Buckets/Bucket tagging.mdsection_vzl_qwy_q7p)
-   Authorize RAM users to manage buckets that have specific tags

    When you have a large number of buckets, you can classify buckets based on tags and configure RAM policy to authorize specific users to manage buckets that have specific tags. For example, you can configure the following RAM policy to authorize User A to list buckets that have the tagging configuration of keytest=valuetest tag:

    ```
    {
        "Version": "1",
        "Statement": [
            {
                "Action": [
                    "oss:ListBuckets"
                ],
                "Resource": [
                    "acs:oss:*:193248792425xxxx:*"
                ],
                "Effect": "Allow",
                "Condition": {
                    "StringEquals": {
                        "oss:BucketTag/keytest": "valuetest"
                    }
                }
            }
        ]
    }
    ```


