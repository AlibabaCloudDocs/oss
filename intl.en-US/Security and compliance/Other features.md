# Other features

OSS provides versioning to prevent data from being accidentally deleted or overwritten. If your bucket is attacked or used to share illegal content, OSS moves the bucket to the sandbox to prevent your other buckets from being affected.

## Versioning

OSS provides versioning to prevent your data from being accidentally deleted. After versioning is enabled for a bucket, data that is overwritten or deleted in the bucket is stored as a previous version. Versioning allows you to recover objects in a bucket to any previous version and protects your data from being accidentally overwritten or deleted.

Versioning applies to all objects in buckets. After you enable versioning for a bucket, all objects in the bucket are subject to versioning. Each version has a unique version ID. You can upload objects to and list, download, delete, and recover objects in a versioned bucket. You can also suspend versioning for a bucket to stop the generation of new object versions. When versioning is suspended, you can still specify a version ID to download, copy, or delete a previous version of an object. Each version incur storage fees. You can configure lifecycle rules to automatically delete expired versions.

For more information, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md).

## OSS sandbox

If your bucket is attacked or used to share illegal content, OSS moves the bucket to the sandbox. A bucket in the sandbox can respond to requests. However, the service quality is degraded. The users of your application may be aware of the degradation. In this case, you must bear the cost incurred by the attack.

To prevent your bucket from being moved to the sandbox due to attacks, you can use Anti-DDoS Pro to prevent DDoS attacks and HTTP floods. To prevent your bucket from being moved to the sandbox due to the distribution of illegal content that involves pornography, politics, and terrorism, we recommend that you activate Content Moderation to periodically scan your bucket to effectively monitor the distribution of illegal content.

For more information, see [OSS sandbox](/intl.en-US/Developer Guide/Data security/OSS sandbox.md).

