# request-payment

When pay-by-requester is enabled for a bucket, requesters pay the request and traffic fees that are incurred when the requesters access objects in the bucket. The bucket owner must still pay the storage fees of the objects in the bucket. If you want to share your data without having to pay for additional fees, you can run the request-payment command to enable pay-by-requester for your bucket.

**Note:** Sample command lines in this topic are based on the 64-bit Linux system. For other systems, replace ./ossutil64 in the commands with the corresponding binary name. For more information, see [ossutil](/intl.en-US/Quick Start/ossutil.md).

For more information about how to enable pay-by-requester, see [Enable pay-by-requester](/intl.en-US/Developer Guide/Buckets/Enable pay-by-requester.md).

## Enable pay-by-requester

-   Command syntax

    ```
    ./ossutil64 request-payment --method put oss://bucket\_name payment
    ```

    The following table describes the parameters you can configure in the command.

    |Parameter|Description|
    |---------|-----------|
    |bucket\_name|The name of the bucket for which you want to enable pay-by-requester.|
    |payment|The payment method for the fees that are incurred when third-party users access data in the bucket. Valid values:

    -   Requester: Requesters are charged for the fee to access objects in the bucket.

Anonymous users cannot access a bucket that has pay-by-requester enabled. Requesters must provide authentication information so that OSS can identify and charge requesters for request and traffic fees. If a requester assumes a RAM role of an Alibaba Cloud account to request data, OSS charges the Alibaba Cloud account for the requests sent by the requester and the generated traffic.

    -   BucketOwner: The bucket owner is charged for the fees when requesters access objects in the bucket. |

-   Examples

    Enable pay-by-requester for a bucket named examplebucket.

    ```
    ./ossutil64 request-payment --method put oss://examplebucket Requester
    ```

    Disable pay-by-requester for a bucket named examplebucket.

    ```
    ./ossutil64 request-payment --method put oss://examplebucket BucketOwner
    ```

    If a similar output is displayed, pay-by-requester is enabled or disabled for the bucket.

    ```
    0.106852(s) elapsed
    ```


## Query the status of pay-by-requester

-   Command syntax

    ```
    ./ossutil64 request-payment --method get oss://bucket\_name
    ```

    bucketname indicates the bucket for which you want to query the pay-by-requester status.

-   Examples

    Query the pay-by-requester status of a bucket named examplebucket.

    ```
    ./ossutil64 request-payment --method get oss://examplebucket
    ```

    The following result shows that pay-by-requester is enabled for examplebucket.

    ```
    Requester
    0.072024(s) elapsed
    ```


## Common options

To use command-line tool ossutil to manage buckets that reside in different regions, you can use the -e option to use the endpoint of the specified bucket. To use command-line tool ossutil to manage buckets within multiple Alibaba Cloud accounts, you can use the -i option to use the AccessKey ID of the specified account and use the -k option to use the AccessKey secret of the specified account.

For example, you can run the following command to enable pay-by-requester for a bucket named testbucket that is located in the China \(Shanghai\) region and owned by another Alibaba Cloud account.

```
./ossutil64 request-payment --method put oss://testbucket -e oss-cn-shanghai.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA**** -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

