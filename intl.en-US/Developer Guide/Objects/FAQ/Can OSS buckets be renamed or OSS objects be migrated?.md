# Can OSS buckets be renamed or OSS objects be migrated?

No, OSS buckets cannot be renamed, and OSS objects cannot be migrated. To use another bucket name, we recommend that you create a bucket, migrate the objects from the source object to the destination \(new\) bucket, and delete the source bucket.

If the number of objects in the source bucket is small, you can copy the objects to migrate your data. For more information, see [Copy objects](/intl.en-US/Developer Guide/Objects/Manage files/Copy objects.md).

If the number of objects in the source bucket is large, use the following methods to migrate your data:

-   Use cross-region replication \(CRR\). For more information, see [Cross-region replication](/intl.en-US/Developer Guide/Data security/Disaster recovery/Cross-region replication.md).
-   Use Data Online Migration. For more information, see [Architectures and configurations]().
-   Use ossimport. For more information, see [ossimport](/intl.en-US/Tools/ossimport/Architectures and configurations.md).

