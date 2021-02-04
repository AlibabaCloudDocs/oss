# Configuration elements

This topic describes the elements used to configure lifecycle rules for objects.

Lifecycle configuration files are in XML format. Example:

```
<LifecycleConfiguration>
<Rule>
<ID>delete logs after 10 days</ID>
<Prefix>logs/</Prefix>
<Status>Enabled</Status>
<Expiration>
<Days>10</Days>
</Expiration>
</Rule>
<Rule>
<ID>delete doc</ID>
<Prefix>doc/</Prefix>
<Status>Disabled</Status>
<Expiration>
<CreatedBeforeDate>2017-12-31T00:00:00.000Z</CreatedBeforeDate>
</Expiration>
</Rule>
<Rule>
<ID>delete xx=1</ID>
<Prefix>rule2</Prefix>
<Tag><Key>xx</Key><Value>1</Value></Tag>
<Status>Enabled</Status>
<Transition>
<Days>60</Days>
<StorageClass>Archive</StorageClass>
</Transition>
</Rule>
</LifecycleConfiguration>
		
```

In the preceding example, three lifecycle rules are defined as follows:

-   The first rule is configured to delete objects whose names contain the prefix logs/ and that have not been modified for more than 10 days.
-   The second rule is configured to delete objects whose names contain the prefix doc/ and that have not been modified since December 31, 2017. However, this rule will not take effect because it is in the Disabled state.
-   The third rule is configured to convert the storage class of objects whose tagging configurations are xx=1 and that have not been modified for more than 60 days to Archive.

The following section describes the elements such as the ID and operation elements that are used to configure lifecycle rules.

## ID element

Specifies the ID of the lifecycle rule configured for the bucket. An ID is composed of up to 255 bytes. If this element is not specified or its value is null, OSS automatically generates a unique ID for the lifecycle rule.

## Status element

Specifies the status of the lifecycle rule, which can be Enabled or Disabled. OSS applies only to enabled rules.

## Prefix element

Specifies the object name prefix based on which the lifecycle rules are applied to all or some of the objects in the bucket.

## Time elements

-   Date

    The <CreatedBeforeDate\> child element specifies an absolute date. All objects that were last modified before this date expire and the specified operation is performed on these objects.

-   Days

    The <Days\> child element specifies the validity period during which objects are retained after they were last modified. After the validity period expires, the specified operation is performed on these objects.


## Operation elements

You can instruct OSS to perform specific operations on an object during its lifecycle by specifying one or more operation elements in the lifecycle rule. The effect of these operations depends on the versioning status of the bucket. The following section provides an overview of the relationship between the operation on an object as specified in the lifecycle rule and the versioning status of the bucket that contains the object.

-   Unversioned buckets

    |Operation|Description|
    |---------|-----------|
    |Transition|Specifies the date or validity period after which the storage class of the object is converted.|
    |Expiration|Specifies the date or validity period after which the object is permanently deleted.|

-   Versioned buckets

    The following section describes the elements used to configure lifecycle rules for objects in versioned \(versioning-enabled or versioning-suspended\) buckets.

    -   Delete or storage class conversion operation on the current version of an object

        |Operation|Versioning-enabled bucket|Versioning-suspended bucket|
        |---------|-------------------------|---------------------------|
        |Expiration|If the current version of the object is not a delete marker, OSS inserts a delete marker with a unique version ID. The delete marker becomes the current version of the object. The previous current version is overwritten.|If the current version of the object is not a delete marker, OSS inserts a delete marker with the null version ID. The delete marker becomes the current version of the object. The previous version with the null version ID is deleted.|
        |If the current version of the object is a delete marker:

         -   When one or more previous versions of the object remain and match the lifecycle rule, no actions are triggered.
        -   When only one version of the object remains and matches the lifecycle rule, the delete marker expires.

OSS determines whether to remove the expired delete marker based on the ExpiredObjectDeleteMarker child element. If the ExpiredObjectDeleteMarker child element is set to true, OSS detects and then removes the expired delete marker to delete the object.

**Note:**

            -   When you configure a lifecycle rule or initiate a delete operation without specifying the object version, the current version is overwritten. If you use lifecycle rules or delete operations to permanently delete all previous versions of an object, only the expired delete marker remains.
            -   You cannot configure ExpiredObjectDeleteMarker or tagging at the same time. |
        |Transition|Specifies the date or validity period after which the storage class of the current version of the object is converted to the specified one.|

    -   Delete or storage class conversion operation on previous versions of an object

        |Operation|Description|Child element|
        |---------|-----------|-------------|
        |NoncurrentVersionExpiration|Specifies the date or validity period after which previous versions are deleted.|The <NoncurrentDays\> child element specifies the validity period during which previous versions are retained. After the validity period expires, the previous versions are permanently deleted. **Note:** For example, assume some version was the current version of an object. On May 1, 2019, the PutObject operation was called and this version became a previous version. In the NoncurrentVersionExpiration element, <NoncurrentDays\> is set to 3. On May 4, 2019, the version was permanently deleted. Each time the PutObject operation is performed on an object, the current version of the object becomes the most recent previous version. The uploaded version becomes the current version of the object. OSS uses the creation time of the next version to determine when a version becomes a previous version. |
        |NoncurrentVersionTransition|Specifies the date or validity period after which the storage class of previous versions is converted.|        -   The <NoncurrentDays\> child element specifies the validity period during which previous versions are retained. After the validity period, the storage class of previous versions is converted.
        -   The <StorageClass\> child element specifies the storage class to which the storage class of matched objects is converted. |


