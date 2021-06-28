# Configuration examples

This topic provides several common examples of lifecycle configurations for you to better manage objects in your bucket.

## Specify filter conditions

Each lifecycle rule contains at least one filter condition, which determines whether the lifecycle rule applies to a part of or all objects in a bucket. The following examples describe how to specify a filter condition in lifecycle configurations:

-   Example 1

    In the following lifecycle configuration example, the `doc/` prefix is specified as a filter condition. This condition indicates that the lifecycle rule applies only to objects whose names are prefixed with `doc/`, such as `doc/test1.txt` and `doc/test2.jpg`. The following operations are performed on these objects based on the lifecycle rule:

    -   Transition: Convert the storage class of the specified objects to Infrequent Access \(IA\) 180 days after the objects are last modified.
    -   Expiration: Delete the specified objects 365 days after the objects are last modified.
    ```
    <LifecycleConfiguration>
      <Rule>
        <ID>test-rule0</ID>
        <Prefix>doc/</Prefix>
        <Status>Enabled</Status>
        <Expiration>
          <Days>365</Days>
        </Expiration>
        <Transition>
          <Days>180</Days>
          <StorageClass>IA</StorageClass>
        </Transition>
      </Rule>
    </LifecycleConfiguration>
    ```

-   Example 2

    In the following lifecycle configuration example, the filter condition indicates that the lifecycle rule applies to all objects in the bucket. All objects in the bucket expire 300 days after the objects are last modified based on the lifecycle rule.

    ```
    <LifecycleConfiguration>
      <Rule>
        <ID>test-rule1</ID>
        <Prefix></Prefix>
        <Status>Enabled</Status>
        <Expiration>
          <Days>300</Days>
        </Expiration>
      </Rule>
    </LifecycleConfiguration>
    ```

-   Example 3

    In the following lifecycle configuration example, the value of Prefix is not configured, which indicates that the lifecycle rule applies to all objects in the bucket. Objects that are modified before December 30, 2019 expire based on the lifecycle rule.

    ```
    <LifecycleConfiguration>
      <Rule>
        <ID>test-rule0</ID>
        <Prefix></Prefix>
        <Status>Enabled</Status>
        <Expiration>
          <CreatedBeforeDate>2019-12-30T00:00:00.000Z</CreatedBeforeDate>
        </Expiration>
      </Rule>
    </LifecycleConfiguration>
    ```


## Overlapping filter conditions

The following examples show whether the operations specified in lifecycle rules conflict when the filter conditions of the lifecycle rules overlap.

-   Example 1

    Two lifecycle rules are configured in the example. A tag filter condition is specified in each lifecycle rule:

    -   The first rule specifies a filter condition based on a tag, which is a key-value pair of tag1 and value1. This lifecycle rule specifies that the storage classes of objects that have the tag are converted to IA 180 days after the objects are last modified.
    -   The second rule specifies a filter condition based on a tag, which is a key-value pair of tag2 and value2. This lifecycle specifies that objects that have the tag expire 10 days after the objects are last modified.
    If an object has the two tags specified in the two lifecycle rules, both lifecycle rules apply to the object. In this case, the object expires 10 days after it is last modified based on the second lifecycle rule. After the object is deleted, the transition operation specified by the first lifecycle rule cannot be performed. Therefore, only the expiration operation specified in the second lifecycle takes effect in this example.

    ```
    <LifecycleConfiguration>
      <Rule>
        <ID>test-rule1</ID>
        <Prefix></Prefix>
        <Tag>
          <Key>tag1</Key>
          <Value>value1</Value>
        </Tag>
        <Status>Enabled</Status>
        <Transition>
          <Days>180</Days>
          <StorageClass>IA</StorageClass>
        </Transition>
      </Rule>
      <Rule>
        <ID>test-rule2</ID>
        <Prefix></Prefix>
        <Tag>
          <Key>tag2</Key>
          <Value>value2</Value>
        </Tag>
        <Status>Enabled</Status>
        <Expiration>
          <Days>10</Days>
        </Expiration>
      </Rule>
    </LifecycleConfiguration>
    ```

-   Example 2

    In this example, two lifecycle rules are configured. The prefixes specified in the two lifecycle rules do not conflict. Therefore, the operations specified in both of the two lifecycle rules take effect.

    -   The first rule specifies an empty prefix as a filter condition, which indicates that the rule applies to all objects in the bucket. All objects in the bucket are deleted 365 days after the objects are last modified based on this rule.
    -   The second rule specifies the test/ prefix as a filter condition. The storage classes of objects whose names contain the test/ prefix are converted to Archive 30 days after the objects are last modified based on this rule.
    ```
    <LifecycleConfiguration>
      <Rule>
        <ID>test-rule1</ID>
        <Prefix></Prefix>
        <Status>Enabled</Status>
        <Expiration>
          <Days>365</Days>
        </Expiration>
      </Rule>
      <Rule>
        <ID>test-rule2</ID>
        <Prefix>test/</Prefix>
        <Status>Enabled</Status>
        <Transition>
          <Days>30</Days>
          <StorageClass>Archive</StorageClass>
        </Transition>
      </Rule>
    </LifecycleConfiguration>
    ```


## Disable lifecycle rules

In the following lifecycle configuration example, two lifecycle rules are configured. The first rule is disabled, and the second rule is enabled.

-   The first rule specifies that the storage classes of objects whose names contain the `logs/` prefix are converted to IA one day after the objects are created.
-   The second rule specifies that the storage classes of objects whose names contain the `documents/` prefix are converted to Archive one day after they are created.

After the lifecycle configurations take effect, only the second lifecycle rule in which the value of the <Status\> field is Enabled takes effect. In this case, the storage classes of objects whose names contain the `documents/` prefix are converted to Archive one day after they are created.

```
<LifecycleConfiguration>
  <Rule>
    <ID>test-rule1</ID>
    <Prefix>logs/</Prefix>
    <Status>Disabled</Status>
    <Transition>
      <Days>1</Days>
      <StorageClass>IA</StorageClass>
    </Transition>
  </Rule>
  <Rule>
    <ID>test-rule2</ID>
    <Prefix>documents/</Prefix>
    <Status>Enabled</Status>
    <Transition>
      <Days>1</Days>
      <StorageClass>Archive</StorageClass>
    </Transition>
  </Rule>
</LifecycleConfiguration>
```

## Configure lifecycle rules for versioning-enabled buckets

In a versioning-enabled bucket, each object has a current version and may have previous versions. For more information about versioning, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md).

-   Example 1

    The lifecycle rule configured in this example specifies that the storage classes of all objects in the bucket are converted to IA 10 days after they are last modified and to Archive 60 days after they become previous versions. In addition, objects are deleted 90 days after they become previous versions.

    ```
    <LifecycleConfiguration>
      <Rule>
        <ID>test-rule0</ID>
        <Prefix></Prefix>
        <Status>Enabled</Status>
        <Transition>
          <Days>10</Days>
          <StorageClass>IA</StorageClass>
        </Transition>
        <NoncurrentVersionTransition>
          <NoncurrentDays>60</NoncurrentDays>
          <StorageClass>Archive</StorageClass>
        </NoncurrentVersionTransition>
        <NoncurrentVersionExpiration>
          <NoncurrentDays>90</NoncurrentDays>
        </NoncurrentVersionExpiration>
      </Rule>
    </LifecycleConfiguration>
    ```

-   Example 2

    If the current version of an object is a delete marker and other versions of the object are deleted, the delete marker expires. You can configure the following lifecycle rule to delete expired delete markers:

    ```
    <LifecycleConfiguration>
      <Rule>
        <ID>test-rule0</ID>
        <Prefix></Prefix>
        <Status>Enabled</Status>
        <Expiration>
          <ExpiredObjectDeleteMarker>true</ExpiredObjectDeleteMarker>
        </Expiration>
      </Rule>
    </LifecycleConfiguration>
    ```


## Configure lifecycle rules to clean up expired parts

If you use multipart upload to upload an object and do not call the CompleteMultipartUpload operation to complete the multipart upload task, the parts of the object are generated and stored in Object Storage Service \(OSS\). You can configure the following lifecycle rule to delete the parts of objects whose names contain the logs prefix five days after the parts are generated:

```
<LifecycleConfiguration>
  <Rule>
    <ID>lifecyclerule1</ID>
    <Prefix>logs/</Prefix>
    <Status>Enabled</Status>
    <AbortMultipartUpload>
      <Days>5</Days>
    </AbortMultipartUpload>
  </Rule>
</LifecycleConfiguration>
```

