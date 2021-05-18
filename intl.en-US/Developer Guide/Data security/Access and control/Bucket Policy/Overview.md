# Overview

You can configure bucket policies to authorize other users to access your specific Object Storage Service \(OSS\) resources.

## Scenarios

Bucket policies can be used for access authorization in the following scenarios:

-   You need to grant permissions to another Alibaba Cloud account or anonymous users to access or manage all resources or part of resources in a bucket.
-   You need to grant different permissions such as read-only, read/write, or complete management to the RAM users of the same Alibaba Cloud to access or manage resources in your bucket.

## Implementation methods

The following table describes the methods that you can use to configure bucket policies in different scenarios.

|Implementation method|Description|
|---------------------|-----------|
|Console|-   [Configure bucket policies by using graphical interfaces.](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure bucket policies to authorize other users to access OSS resources.md)
-   [Configure bucket policies by specifying policy syntax.](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure bucket policies to authorize other users to access OSS resources.md)
-   [Implement data sharing across departments based on bucket policies](/intl.en-US/Developer Guide/Data security/Access and control/Bucket Policy/Implement data sharing across departments based on bucket policies.md)
-   [Authorize a RAM user under another Alibaba Cloud account by adding a bucket policy](/intl.en-US/Developer Guide/Data security/Access and control/Bucket Policy/Tutorial: Authorize a RAM user under another Alibaba Cloud account by adding a bucket
         policy.md) |
|ossutil|[bucket-policy](/intl.en-US/Tools/ossutil/Common commands/bucket-policy.md)|
|SDKs for different programming languages|-   [OSS SDK for Java](/intl.en-US/SDK Reference/Java/Buckets/Bucket policy.md)
-   [OSS SDK for PHP](/intl.en-US/SDK Reference/PHP/Buckets/Bucket policy.md)
-   [OSS SDK for Node.js](/intl.en-US/SDK Reference/Node. js/Buckets/Bucket policy.md)
-   [OSS SDK for Python](/intl.en-US/SDK Reference/Python/Buckets/Bucket policy.md)
-   [OSS SDK for .NET](/intl.en-US/SDK Reference/. NET/Buckets/Bucket policy.md)
-   [OSS SDK for Go](/intl.en-US/SDK Reference/Go/Buckets/Bucket policy.md)
-   [OSS SDK for C++](/intl.en-US/SDK Reference/C++/Buckets/Bucket policy.md) |

