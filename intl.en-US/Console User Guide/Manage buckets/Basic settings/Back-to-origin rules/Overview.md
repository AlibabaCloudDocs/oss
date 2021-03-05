# Overview

OSS allows you to configure back-to-origin rules. If the requested object does not exist in a bucket for which back-to-origin rules are configured, OSS obtains the requested object from the origin specified by the back-to-origin rules. You can configure mirroring-based or redirection-based back-to-origin rules for hot migration and specific request redirection.

-   Mirroring-based back-to-origin

    After you configure mirroring-based back-to-origin rules for a bucket, if a requester accesses an object that does not exist in a bucket, OSS obtains the object from the origin specified by the back-to-origin rules. OSS returns the object obtained from the origin to the requester and stores the object in the specified bucket.

    Mirroring-based back-to-origin is used to migrate data to OSS. This feature allows you to migrate any service that already runs on an origin that you create or in another cloud service to OSS without interrupting services. You can configure mirroring-based back-to-origin rules to obtain the data that is not migrated to OSS during migration, which ensures that the service is not interrupted. For detailed examples, see [Seamlessly migrate data from a cloud service to OSS]().

-   Redirection-based back-to-origin

    After you configure redirection-based back-to-origin rules for a bucket, if an error occurs when a requester accesses the bucket, OSS redirects the request to the origin specified by the redirection-based back-to-origin rules. You can use this feature to redirect requests for objects and develop various services based on redirection.


You can configure up to 20 back-to-origin rules, which are executed in a sequence that they are configured. For more information, see [Manage back-to-origin configurations](/intl.en-US/Developer Guide/Objects/Manage files/Manage back-to-origin configurations.md).

