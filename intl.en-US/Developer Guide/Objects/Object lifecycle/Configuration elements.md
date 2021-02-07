# Configuration elements

This topic describes the elements that can be configured in lifecycle rules for objects.

The configuration file of a lifecycle rule for objects is in the XML format. Example:

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

-   The first rule is configured to delete objects whose names contain the logs/ prefix and that are not modified for more than 10 days.
-   The second rule is configured to delete objects whose names contain the doc/ prefix and that are not modified since December 31, 2017. However, this rule does not take effect because this rule is in the Disabled state.
-   The third rule is configured to convert the storage class of objects whose tagging configurations are xx=1 and that are not modified for more than 60 days to Archive.

The following section describes the elements such as ID and operation that can be configured in lifecycle rules.

## ID

The ID element specifies the ID of the lifecycle rule configured for the bucket. The ID of an lifecycle rule can be up to 255 bytes in length. If the value of this parameter is null or not specified, OSS automatically generates a unique ID for the lifecycle rule.

## Status

The Status element specifies the status of the lifecycle rule, which can be Enabled or Disabled. Only rules in which the value of Status is Enabled take effect.

## Prefix

Specifies the object name prefix based on which the lifecycle rules are applied. The lifecycle rule applies to objects whose names contain the specified prefix.

## Time elements

-   Date

    The <CreatedBeforeDate\> child element specifies an absolute date. All objects that were last modified before this date expire and the specified operation is performed on these objects.

-   Days

    The <Days\> child element specifies the validity period during which objects are retained after they were last modified. After the validity period expires, the specified operation is performed on these objects.


## Operation elements

You can instruct OSS to perform specific operations on an object during its lifecycle by specifying one or more operation elements in the lifecycle rule. The effect of these operations depends on the versioning status of the bucket. The following section provides an overview of the relationship between the operation on an object as specified in the lifecycle rule and the versioning status of the bucket in which the object is stored.

-   Unversioned buckets

    |Operation|Description|
    |---------|-----------|
    |Transition|Specifies the date or validity period after which the storage class of the object is converted.|
    |Expiration|Specifies the date or validity period after which the object is permanently deleted.|

-   Versioned buckets

    The following section describes the elements used to configure lifecycle rules for objects in versioned \(versioning-enabled or versioning-suspended\) buckets.

    -   Delete or storage class conversion operation on the current version of an object

        |Operation|Description|
        |---------|-----------|
        |Expiration|The operation performed on an expired object is different based on whether the current version of the object is a delete marker.        -   If the current version of the object is not a delete marker:
            -   When you configure the lifecycle rule on an object in a versioning-enabled bucket, OSS inserts a delete marker that has a unique version ID. The delete marker becomes the current version of the object. The previous current version is overwritten.
            -   When you configure the lifecycle rule on an object in a versioning-suspended bucket, OSS inserts a delete marker whose version ID is null. The delete marker becomes the current version of the object. The previous null version is overwritten.
        -   If the current version of the object is a delete marker:
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
        |NoncurrentVersionExpiration|Specifies the date or validity period after which previous versions are deleted.|The <NoncurrentDays\> child element specifies the validity period during which previous versions are retained. After the validity period expires, the previous versions are permanently deleted. **Note:** For example, the PutObject operation was called to overwrite the current version of an object on May 1, 2019. The current version became a previous version. In the NoncurrentVersionExpiration element, <NoncurrentDays\> is set to 3. On May 4, 2019, the version was permanently deleted. Each time the PutObject operation is performed on an object, the current version of the object becomes the most recent previous version. The uploaded version becomes the current version of the object. OSS determines when a version becomes a previous version based on the creation time of the next version. |
        |NoncurrentVersionTransition|Specifies the date or validity period after which the storage class of previous versions is converted.|        -   The <NoncurrentDays\> child element specifies the validity period during which previous versions are retained. After the validity period, the storage class of previous versions is converted.
        -   The <StorageClass\> child element specifies the storage class to which the storage class of matched objects is converted. |


