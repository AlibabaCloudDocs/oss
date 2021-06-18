# Bucket naming conventions

A bucket is a container that is used to stored objects in Object Storage Service \(OSS\). Every object is contained in a bucket. You can configure a variety of bucket attributes such as the region, ACL, and storage class. You can create buckets of different storage classes to store data.

## Naming conventions

The maximum number of buckets that can be created by using an Alibaba Cloud account within a region is 100. After a bucket is created, its name cannot be modified. OSS supports the following bucket naming conventions:

-   The name of a bucket must be unique in OSS in an Alibaba Cloud account.
-   The name can contain only lowercase letters, digits, and hyphens \(-\).
-   The name must start and end with a lowercase letter or a digit.
-   The name must be 3 to 63 characters in length.

## Examples

The following examples of bucket names are valid:

-   examplebucket1
-   test-bucket-2021
-   aliyun-oss-bucket

The following examples show invalid bucket names and the reasons why the names are invalid:

-   Examplebucket1 \(Uppercase letters are included.\)
-   test\_bucket\_2021 \(Underscores \(\_\) are included.\)
-   aliyun-oss-bucket- \(The name ends with a hyphen \(-\).\)

