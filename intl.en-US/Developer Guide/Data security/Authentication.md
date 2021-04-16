# Authentication

By default, the access control list \(ACL\) of OSS resources including buckets and objects is set to private to ensure data security. Only the owners of the resources and authorized users can access these buckets and objects. OSS allows you to use a variety of policies to grant other users specific permissions to access or use your OSS resources. A user can access authorized OSS resources only after the user is authenticated by OSS based on all policies.

## Overview

When OSS receives a request, OSS determines whether to allow or deny the request based on authentication. The authentication is performed based on identity verification, role-based session policies, Resource Access Management \(RAM\) policies, bucket policies, object ACLs, and bucket ACLs.

![process](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3832458161/p254657.jpg)

In the preceding figure, the following states are used to indicate the authentication result of a request:

-   Allow: The request matches an Allow rule specified in a policy. In this case, OSS allows the request.
-   Explicit Deny: The request matches a Deny rule specified in a policy. In this case, OSS explicitly denies the request.
-   Implicit Deny: The request does not match the Allow or Deny rules specified in a policy or the policy that OSS uses to authenticate the request does not exist. In this case, OSS implicitly denies the request.

## Process

1.  OSS checks whether the request passes identity verification.

    After OSS receives a request, OSS compares the signature contained in the request with the signature calculated by the OSS server.

    -   If the two signatures are inconsistent, the request is denied.
    -   If the two signatures are consistent, OSS checks whether the request needs to be authenticated based on role-based session policies.
2.  OSS checks whether the request needs to be authenticated based on role-based session policies.

    If the request needs to be authenticated based on role-based session policies, OSS checks whether the request matches the session policies.

    -   If the matching result of the request is Explicit Deny or Implicit Deny, the request is denied.
    -   If the matching result of the request is Allow, OSS authenticates the request based on RAM policies and bucket policies.
    If the request does not need to be authenticated based on role-based session policies, OSS proceeds to authenticate the request based on RAM policies and bucket policies.

3.  OSS checks whether the request matches RAM policies and bucket policies.

    RAM policies are configured based on users. You can configure RAM policies to control the resources that can be accessed by users. When OSS authenticates a request based on RAM policies, OSS determines whether to allow or deny the request based on the account used to sent the request.

    -   If the request is sent by using the AccessKey pair of an Alibaba Cloud account, OSS implicitly denies the request.
    -   If the request is sent by using the AccessKey pair of a RAM user or an STS credential to access a bucket that does not belong to the RAM user or the Alibaba Cloud account, OSS implicitly denies the request.
    -   OSS calls the authentication operation provided by RAM to authenticate requests. Authentication based on accounts and the resource groups of buckets is supported. After the authentication, OSS determines whether to allow, explicitly deny, or implicitly deny the request.
    Bucket policies are resource-based authorization policies. The owner of a bucket can configure bucket policies to authorize RAM users or other Alibaba Cloud accounts to access the bucket or specific resources in the bucket.

    -   If no bucket policy is configured for the bucket, OSS implicitly denies the request.
    -   If bucket policies are configured for the bucket, OSS checks whether the request matches the bucket policies and then determines whether to allow, explicitly deny, or implicitly deny the request.
4.  OSS checks whether the request matches an Explicit Deny rule specified in all policies described in the preceding three steps.

    If the request matches an Explicit Deny rule, OSS denies the request. If the request does not match an Explicit Deny rule, OSS checks whether the request matches an Allow rule specified in all policies described in the preceding three steps.

    1.  OSS checks whether the request matches an Allow rule specified in RAM policies and bucket policies.

        If the request matches an Allow rule, OSS allows the request. If the request does not match an Allow rule, OSS checks the source of the request.

    2.  OSS checks the source of the request.

        If the request is sent by using a management API operation, OSS denies the requests. If the request is sent by using a data API operation, OSS checks the ACLs of the object and bucket that the request is sent to access.

        Management API operations include service-related operations, bucket-related operations, and LiveChannel-related operations. Service-related operations include GetService \(Listbuckets\). Bucket-related operations include PutBucket and GetBucketLifecycle. LiveChannel-related operations include PutLiveChannel and DeleteLiveChannel.

        Data API operations include object-related operations, such as PutObject and GetObject.

5.  OSS authenticates the request based on the ACLs of the object and bucket that the request is sent to access.

    OSS authenticates the request based on the [object ACL](/intl.en-US/API Reference/Object operations/ACL/PutObjectACL.md), whether the user that sends the request is the bucket owner, and whether the request is a read request or a write request.

    -   If the authentication result is Allow, OSS allows the request.
    -   If the authentication result is Deny, OSS denies the request.
    If the ACL of the object is inherited from the bucket, OSS authenticates the request based on the ACL of the bucket in which the object is stored.

    OSS authenticates the request based on the [bucket ACL](/intl.en-US/API Reference/Bucket operations/ACL/PutBucketAcl.md), and whether the user that sends the request is the bucket owner.

    -   If the authentication result is Allow, OSS allows the request.
    -   If the authentication result is Deny, OSS denies the request.

