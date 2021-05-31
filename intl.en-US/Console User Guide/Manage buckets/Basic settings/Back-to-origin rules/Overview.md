# Overview

You can configure back-to-origin rules for a bucket. If a request is sent to access an object that does not exist in a bucket for which back-to-origin rules are configured, OSS obtains the requested object from the origin specified by the back-to-origin rules. You can configure mirroring-based or redirection-based back-to-origin rules for hot migration and specific request redirection.

For more information, see [Manage back-to-origin configurations](/intl.en-US/Developer Guide/Objects/Manage files/Manage back-to-origin configurations.md).

## Mirroring-based back-to-origin

After you configure mirroring-based back-to-origin rules for a bucket, if a requester accesses an object that does not exist in a bucket, OSS obtains the object from the origin specified by the back-to-origin rules. OSS returns the object obtained from the origin to the requester and stores the object in the bucket. For more information about how to configure mirroring-based back-to-origin rules, see [Basic configurations of mirroring-based back-to-origin](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Back-to-origin rules/Basic configurations of mirroring-based back-to-origin.md).

Mirroring-based back-to-origin is used to migrate data to OSS. This feature allows you to migrate a service that already runs on an origin that you create or on another cloud service to OSS without service interruptions. You can use mirroring-based back-to-origin rules during migration to obtain the data that is not migrated to OSS. This ensures service continuity. For detailed examples, see [Seamlessly migrate data from a cloud service to OSS]().

## Redirection-based back-to-origin

After you configure redirection-based back-to-origin rules for a bucket, if an error occurs when a requester accesses the bucket, OSS redirects the request to the origin specified by the redirection-based back-to-origin rules. You can use this feature to redirect requests for objects and develop various services based on redirection. For more information about how to configure redirection-based back-to-origin rules, see [Configure redirection-based back-to-origin](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Back-to-origin rules/Configure redirection-based back-to-origin.md).

## Rules

You can configure up to 20 back-to-origin rules for a bucket in the OSS console. By default, the rules are used to match a request in the sequence that they are configured. You can click **Move Up** or **Move Down** on the right side of the rules to change the order of the sequence.

![3](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6772542261/p270031.jpg)

If a request matches a rule, subsequent rules are not used to match the request. OSS determines whether a request matches a back-to-origin rule based on whether the request meets the conditions specified in the rule. OSS does not check if a request can obtain the requested object from the origin.

