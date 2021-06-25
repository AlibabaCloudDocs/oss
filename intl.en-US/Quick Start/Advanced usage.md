# Advanced usage

After you are familiar with basic Object Storage Service \(OSS\) operations such as uploading and downloading objects, you can use the advanced features provided by OSS.

The following table lists the common advanced features of OSS.

|Feature|Description|
|-------|-----------|
|[Enable pay-by-requester](/intl.en-US/Developer Guide/Buckets/Enable pay-by-requester.md)|When pay-by-requester is enabled for a bucket, requesters pay the request and traffic fees that are incurred when the requesters access objects in the bucket. The bucket owner is still charged for the storage fees of the objects. You can enable pay-by-requester to share your data in OSS without having to pay for additional fees on your own.|
|[Lifecycle rules](/intl.en-US/Developer Guide/Buckets/Lifecycle/Lifecycle rules.md)|After you configure lifecycle rules for a bucket, OSS converts the storage class of objects in the bucket to Infrequent Access \(IA\), Archive, Cold Archive, or deletes expired objects and parts to save storage costs on a regular basis.|
|[Static website hosting](/intl.en-US/Developer Guide/Static website hosting/Static website hosting.md)|Static websites are websites in which all web pages consist only of static content, including scripts such as JavaScript code running on the client. You can use the static website hosting feature to host your static website in an OSS bucket and use the endpoint of the bucket to access the website.|
|[Versioning](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md)|OSS allows you to configure versioning for a bucket to protect objects stored in the bucket. After you enable versioning for a bucket, objects that are overwritten or deleted in the bucket are saved as previous versions. You can use versioning to recover a previous version of an object that is accidentally overwritten or deleted.|
|[Access control](/intl.en-US/Developer Guide/Data security/Access and control/Overview.md)|OSS provides you with the following access control methods to manage access to objects in buckets: bucket policies, access control list \(ACL\), Resource Access Management \(RAM\) policies, Security Token Service \(STS\)-based temporary access authorization, and hotlink protection. Bucket policies and ACLs are implemented based on resources. RAM policies are implemented based on users. Hotlink protection is implemented by configuring whitelists.|
|Data encryption|[Server-side encryption](/intl.en-US/Developer Guide/Data security/Data encryption/Server-side encryption.md): When you upload an object to a bucket for which server-side encryption is enabled, OSS encrypts the object and stores the encrypted object. When you download the encrypted object from OSS, OSS automatically decrypts the object and returns the decrypted object to you. A header is added in the response to indicate that the object is encrypted on the OSS server. [Client-side encryption](/intl.en-US/Developer Guide/Data security/Data encryption/Client-side encryption.md): Before you upload an object to a bucket, the object is encryption on the local client. |
|[Cross-region replication](/intl.en-US/Developer Guide/Data security/Disaster recovery/Cross-region replication.md)|Cross-region replication \(CRR\) provides automatic and asynchronous \(near real-time\) replication of objects across buckets in different OSS regions. Operations such as creating, overwriting, and deleting objects can be synchronized from a source bucket to a destination bucket. CRR can meet your requirements on geo-disaster recovery and data replication.|
