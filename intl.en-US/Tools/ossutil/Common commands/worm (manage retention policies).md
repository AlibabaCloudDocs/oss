# worm \(manage retention policies\)

OSS supports time-based retention policies. After a retention policy is configured for a bucket, objects stored inside the bucket cannot be deleted or modified during the retention period. This topic describes how to use the worm command to manage the retention policies configured for a bucket.

**Note:**

-   The commands described in this topic are for 64-bit Linux systems. To use the commands in the examples on 64-bit Windows systems, replace ./ossutil64 in the commands with ossutil64.exe.
-   For more information about retention policies, see [Retention policy](/intl.en-US/Developer Guide/Data security/Retention policy.md).

## Create and lock a retention policy

To use a retention policy to protect objects in your bucket, you must create and lock the policy.

1.  Create a retention policy.
    -   Command syntax

        ```
        ./ossutil64 worm init oss://BucketName days
        ```

        The following table describes the parameters that you can configure when you use the worm command to create a retention policy.

        |Parameter|Description|
        |---------|-----------|
        |BucketName|The name of the bucket for which you want to configure the retention policy.|
        |days|The retention period of the retention policy. During the retention period, objects in the bucket cannot be modified or deleted.         -   Unit: days
        -   Valid values: 1 to 25550 |

    -   Examples

        You can run the following command to create a retention policy for a bucket named examplebucket and sets the retention period of the policy to 180 days:

        ```
        ./ossutil64 worm init oss://examplebucket 180
        ```

        If a similar output is displayed, the retention policy is created.

        ```
        init success,worm id is 581D8A7FFA064C80827CAB4076A93A78
        ```

2.  Lock a retention policy.
    -   Command syntax

        ```
        ./ossutil64 worm complete oss://BucketName WormId
        ```

        The following table describes the parameters that you can configure when you use the worm command to lock a retention policy.

        |Parameter|Description|
        |---------|-----------|
        |BucketName|The name of the bucket for which the retention policy you want to lock is configured.|
        |WormId|The ID of the retention policy you want to lock. This parameter is returned when you successfully create a retention policy.|

    -   Examples

        You can run the following command to lock the retention policy configured for a bucket named examplebucket:

        ```
        ./ossutil64 worm complete oss://examplebucket 581D8A7FFA064C80827CAB4076A93A78
        ```

        If a similar output is displayed, the retention policy is locked.

        ```
        0.073810(s) elapsed
        ```


## Extend the retention period of a retention policy

After a retention policy is locked, objects in the bucket cannot be modified or deleted during the retention period of the policy. If the retention period of the retention policy cannot meet your requirements for data protection, you can run the following command to extend the retention policy.

-   Command syntax

    ```
    ./ossutil64 worm extend oss://BucketName days WormId
    ```

-   Examples

    You can run the following command to extend the retention period of the retention policy configured for a bucket named examplebucket to 360 days:

    ```
    ./ossutil64 worm extend oss://examplebucket 360 581D8A7FFA064C80827CAB4076A93A78
    ```

    If a similar output is displayed, the retention period of the retention policy is extended to 360 days.

    ```
    0.067810(s) elapsed
    ```


## Query the configurations of a retention policy

You can run the following command to query the configurations of the retention policy configured for a bucket.

-   Command syntax

    ```
    ./ossutil64 worm get oss://BucketName
    ```

-   Examples

    You can run the following command to query the configurations of the retention policy configured for a bucket named examplebucket:

    ```
    ./ossutil64 worm get oss://examplebucket
    ```

    If a similar output is displayed, the configurations of the retention policy are found. The returned results include the ID, status, retention period, and creation time of the retention policy.

    ```
    <WormConfiguration>
          <WormId>581D8A7FFA064C80827CAB4076A93A78</WormId>
          <State>Locked</State>
          <RetentionPeriodInDays>360</RetentionPeriodInDays>
          <CreationDate>2021-01-19T03:36:53.000Z</CreationDate>
      </WormConfiguration>
    ```


## Delete a retention policy

You can delete a retention policy before the policy is locked.

-   Command syntax

    ```
    ./ossutil64 worm abort oss://BucketName
    ```

-   Examples

    You can run the following command to delete the retention policy configured for a bucket named examplebucket:

    ```
    ./ossutil64 worm abort oss://examplebucket
    ```

    If a similar output is displayed, the retention policy is deleted.

    ```
    0.067810(s) elapsed
    ```


## Common options

To use ossutil to manage buckets that are located in different regions, you can use the -e option to use the endpoint of the specified bucket. To use ossutil to manage buckets that are owned by different Alibaba Cloud accounts, you can use the -i option to use the AccessKey ID of the specified account, and use the -k option to use the AccessKey secret of the specified account.

For example, you can run the following command to create a retention policy for a bucket named test, which is located in the China \(Hangzhou\) region and is owned by another Alibaba Cloud account:

```
./ossutil64 worm init oss://test -e oss-cn-hangzhou.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

For more information about other common options that apply to the worm command, see [Common options](/intl.en-US/Tools/ossutil/View options.md).

