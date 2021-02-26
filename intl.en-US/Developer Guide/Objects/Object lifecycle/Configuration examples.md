# Configuration examples

This topic describes several common examples of lifecycle configurations for you to better manage the objects in your bucket.

## Specify filter conditions

Each lifecycle rule contains at least one filter condition. The filter determines whether the lifecycle rule applies to a part of or all of the objects in a bucket. The following examples describe how to specify a filter condition in lifecycle rule configurations.

-   Example 1

    In the following lifecycle rule configuration, the `doc/` prefix is specified as the filter, which indicates that the lifecycle rule applies only to objects whose names are prefixed with `doc/` such as `doc/test1.txt` and `doc/test2.jpg`. The lifecycle rule also specifies the following operations on these objects:

    -   Transition: Convert the storage class of objects to Infrequent Access \(IA\) 180 days after the objects are last modified.
    -   Expiration: Delete objects 365 days after objects are last modified.
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

    In the following lifecycle rule configuration, the filter condition indicates that the lifecycle rule applies to all objects in the bucket. The lifecycle rule specifies that all objects in the bucket expire 300 days after the objects are last modified.

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

    In the following lifecycle rule configuration, the value of Prefix is null, which indicates that the lifecycle rule applies to all objects in the bucket. The lifecycle rule specifies that each object that is modified before December 30, 2019 expires.

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

The following examples describe whether the operations specified in lifecycle rules conflict when the filter conditions overlap.

-   Example 1

    Two lifecycle rules are configured in the example and each rule specifies a tag filter condition:

    -   The first rule specifies a filter condition based on a tag that is a key-value pair of tag1 and value1. Based on this rule, OSS converts the storage class of objects to IA 180 days after the objects are last modified.
    -   The second rule specifies a filter condition based on a tag that is a key-value pair of tag2 and value2. Based on this rule, objects expire 10 days after they are last modified.
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

    **Note:** If an object is configured with the two tags, both lifecycle rules apply to this object. In this case, the object expires 10 days after it is last modified. After the object is deleted, the transition operation specified on the object becomes invalid.

-   Example 2

    Two lifecycle rules are configured with overlapping prefixes as their filter conditions.

    -   The first rule specifies an empty prefix as its filter condition, which indicates that the rule applies to all objects in the bucket. This rule specifies that all objects in the bucket are deleted 365 days after they are last modified.
    -   The second rule specifies the test/ prefix as its filter condition. This rule specifies that the storage classes of objects that are prefixed with test/ are converted to Archive 30 days after they are last modified.
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

You can temporarily disable lifecycle rules. In the following example, two lifecycle rules are configured. The first rule is disabled. The second rule is enabled based on the policy.

-   The first rule specifies that the storage classes of objects whose names contain the `logs/` prefix are converted to IA one day after they are created.
-   The second rule specifies that the storage classes of objects whose names contain the `documents/` prefix are converted to Archive one day after they are created.

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

## Lifecycle rules for versioning-enabled buckets

In a versioning-enabled bucket, each object has a current version and may have previous versions. For more information about versioning, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md).

-   Example 1

    In the following lifecycle rule configuration, the storage classes of objects are converted to IA 10 days after they are last modified and to Archive 60 days after they become previous versions. Objects are deleted 90 days after they become previous versions.

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

    If an object has only a delete marker as the current version and other versions of the object are deleted, the delete marker expires when the object expires based on lifecycle rules. The following code provides an example on how to delete an expired delete marker:

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


