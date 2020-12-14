# What are the operations that affect the LastModified attribute of OSS objects?

LastModified \(last modified time\) is an important attribute of OSS objects. It is used in billing, incremental migration, and lifecycle rules. When you perform some operations on objects, the LastModified values of the objects are updated. The following table lists the common operations on objects.

|Common operation|API operation|Whether the LastModified value of the object is updated|
|----------------|-------------|-------------------------------------------------------|
|Modify object ACLs|CopyObject|Yes|
|PutObjectACL|Yes|
|Modify the user metadata of an object|CopyObject|Yes|
|Modify the storage class of an object|CopyObject|Yes|
|CommitTransition|No|
|Modify the encryption algorithm for an object|CopyObject|Yes|
|Overwrite an object|PutObject|Yes|
|CopyObject|Yes|
|Add or modify object tags|PutObjectTagging|No|
|Delete an object tag|DeleteObjectTagging|No|
|Restore an Archive or Cold Archive object|RestoreObject|No|

**Note:**

-   When you modify the storage class of an object by using the OSS console, ossutil, ossbrowser, or SDKs, OSS generates a new object of the specified storage class to overwrite the original object.
-   The last modified time of an object is updated when the object is overwritten or the storage class and encryption method are changed by using CopyObject. If the object whose last modified time is updated is an IA, Archive, or Cold Archive object and is stored for a period less than the minimum storage duration, you are charged for the minimum storage duration. For more information, see [Storage fees](/intl.en-US/Pricing/Billing items and methods/Storage fees.md).

    For example, if you change the storage class of an object from IA to Archive by using the OSS console after the object is stored for 12 days, the last modified time of the object is updated. In addition, you are charged for the minimum storage duration of an IA object, which is 30 days.


