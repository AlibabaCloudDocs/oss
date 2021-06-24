# Enable pay-by-requester

When pay-by-requester is enabled for a bucket, requesters pay the request and traffic fees that are incurred when the requesters access objects in the bucket. The bucket owner is still charged for the storage fees of the objects. You can enable pay-by-requester to share your data in OSS without having to pay for additional fees on your own.

## Implementation methods

|Implementation method|Description|
|---------------------|-----------|
|[Console](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure the pay-by-requester mode.md)|A user-friendly and intuitive web application|
|[ossutil](/intl.en-US/Tools/ossutil/Common commands/request-payment (configure pay-by-requester).md)|A high-performance command-line tool|
|[OSS SDK for Java](/intl.en-US/SDK Reference/Java/Buckets/Enable pay-by-requester mode.md)|SDK demos for various programming languages|
|[OSS SDK for PHP](/intl.en-US/SDK Reference/PHP/Buckets/Pay-by-requester.md)|
|[OSS SDK for Node.js](/intl.en-US/SDK Reference/Node. js/Buckets/Pay-by-requester mode.md)|
|[OSS SDK for Python](/intl.en-US/SDK Reference/Python/Buckets/Enable pay-by-requester mode.md)|
|[OSS SDK for .NET](/intl.en-US/SDK Reference/. NET/Buckets/Enable pay-by-requester.md)|
|[OSS SDK for Go](/intl.en-US/SDK Reference/Go/Buckets/Pay-by-requester mode.md)|
|[OSS SDK for C++](/intl.en-US/SDK Reference/C++/Buckets/Pay-by-requester mode.md)|

## Use cases

-   Share large datasets such as postal code directories, reference data, geospatial information, or web crawling data. For example, a research institute provides a public dataset to share data with their customers. Requesters are expected to pay the request and traffic fees. In this case, you can perform the following steps to configure pay-by-requester:
    1.  Enable pay-by-requester for the bucket. For more information, see [Configure the pay-by-requester mode](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure the pay-by-requester mode.md).
    2.  Configure a bucket policy to authorize the RAM users of Alibaba Cloud accounts owned by your customers to access your bucket. For more information, see [Use bucket policies to authorize other users to access OSS resources](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure bucket policies to authorize other users to access OSS resources.md).
-   Deliver data to your customers or partners. For example, a company must deliver production data to its partners and expects the partners to pay for request and traffic fees generated in data download.

    In this case, you can perform the following steps to configure pay-by-requester:

    1.  Enable pay-by-requester for the bucket.
    2.  Set the ACL of the bucket to private.
    3.  Configure a bucket policy to authorize the RAM users of Alibaba Cloud accounts owned by your partners to access your bucket. For more information, see [Authorize a RAM user under another Alibaba Cloud account by adding a bucket policy](/intl.en-US/Developer Guide/Data security/Access and control/Bucket Policy/Tutorial: Authorize a RAM user under another Alibaba Cloud account by adding a bucket
         policy.md).
    **Note:** You must authorize the RAM users of Alibaba Cloud accounts owned by your partners to access your bucket, instead of providing the AccessKey pairs of your RAM users. If your customers or partners use the AccessKey pairs of your RAM users to access your bucket, you are charged for requests and traffic because you are the requester.


## Request method

-   Requests from anonymous users are not allowed

    If you enable pay-by-requester for a bucket, anonymous users are not allowed to access the bucket. Requesters must provide authentication information so that OSS can identify and charge requesters for request and traffic fees.

    If a requester assumes a RAM role of an Alibaba Cloud account to request data, OSS charges the Alibaba Cloud account for the requests sent by the requester and the generated traffic.

-   Requests must contain the x-oss-request-payer header

    If you enable pay-by-requester for a bucket, requesters must specify the `x-oss-request-payer` header in POST, GET, or HEAD requests. This header indicates that requesters understand that they are charged for the requests and downloaded data. Otherwise, the requests cannot be authenticated.

    -   POST, GET, and HEAD requests must contain the `x-oss-request-payer:requester` header.
    -   Requests that use signed URLs must contain the `x-oss-request-payer=requester` header.
    Bucket owners do not need to specify the `x-oss-request-payer` header in requests sent to access their buckets. The bucket owner is charged for their own requests and traffic.


## Billing

When pay-by-requester is enabled, requesters are charged for one or more of the following billing items based on the request content: outbound traffic over the Internet, CDN back-to-origin traffic, and traffic generated when requesters call API operations, perform Image Processing \(IMG\), take video snapshots, and retrieve IA objects or Archive objects. The bucket owner pays other fees such as storage, object tagging, and transfer acceleration fees. In the following scenarios, a request to a bucket with pay-by-requester fails. In these cases, HTTP status code 403 is returned and the bucket owner is charged for the request.

-   The request is a POST, GET, or HEAD request that does not contain the x-oss-request-payer header or is a RESTful operation request that does not contain the x-oss-request-payer parameter.
-   The request fails to be authenticated.
-   The request is anonymous.

