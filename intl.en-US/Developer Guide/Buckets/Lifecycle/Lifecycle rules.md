# Lifecycle rules

You can call PutBucketLifecycle to configure lifecycle rules. After you configure lifecycle rules for a bucket, Object Storage Service \(OSS\) converts the storage class of expired objects to IA, Archive, or Cold Archive, or deletes expired objects and parts to save storage costs.

**Note:** For more information about the operation called to configure lifecycle rules, see [PutBucketLifecycle](/intl.en-US/API Reference/Bucket operations/Lifecycle/PutBucketLifecycle.md).

## Scenarios

You can configure a lifecycle rule to regularly convert the storage class of cold data to IA, Archive, or Cold Archive, or delete objects that are no longer accessed. This makes data management easier and saves storage costs. For example, you can configure lifecycle rules in the following scenarios:

-   A medical institution stores its medical records in OSS. These objects are occasionally accessed within six months after they are uploaded, and almost never after that. You can configure a lifecycle rule to convert the storage class of these objects to Archive 180 days after they are uploaded.
-   A company stores the call records of its service hotline in OSS. These objects are frequently accessed within the first two months, occasionally after two months, and almost never after six months. After two years, these objects no longer need to be stored. You can configure a lifecycle rule to convert the storage class of these objects to IA 60 days after they are uploaded and to Archive 180 days after they are uploaded, and delete them 730 days after they are uploaded.
-   You can manually delete up to 1,000 objects each time. If a bucket contains more than 1,000 objects, you must delete the objects multiple times. To delete all objects in the bucket at the same time, you can configure a lifecycle rule that is used to delete all objects in the bucket the next day.

## Implementation methods

|Implementation method|Description|
|---------------------|-----------|
|[Console](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure lifecycle rules.md)|A user-friendly and intuitive web application|
|[ossutil](/intl.en-US/Tools/ossutil/Common commands/lifecycle (manage lifecycle rules).md)|A high-performance command-line tool|
|[Java SDK](/intl.en-US/SDK Reference/Java/Buckets/Manage lifecycle rules.md)|SDK demos for a variety of programming languages|
|[Python SDK](/intl.en-US/SDK Reference/Python/Buckets/Manage lifecycle rules.md)|
|[PHP SDK](/intl.en-US/SDK Reference/PHP/Buckets/Manage lifecycle rules.md)|
|[Go SDK](/intl.en-US/SDK Reference/Go/Buckets/Manage lifecycle rules.md)|
|[C SDK](/intl.en-US/SDK Reference/C/Buckets/Manage lifecycle rules.md)|
|[.NET SDK](/intl.en-US/SDK Reference/. NET/Buckets/Manage lifecycle rules.md)|
|[Node.js SDK](/intl.en-US/SDK Reference/Node. js/Buckets/Manage lifecycle rules.md)|
|[Ruby SDK](/intl.en-US/SDK Reference/Ruby/Buckets/Manage lifecycle rules.md)|

## Usage notes

-   API operation calling fees

    Successful API operations that are asynchronously called to perform actions triggered by lifecycle rules are recorded in access logs and incur fees. Failed operations are not recorded or charged.

-   Storage fees for IA, Archive, and Cold Archive objects that are stored for less than the minimum storage duration

    -   Fees incurred when the storage class of objects is converted to IA or Archive and the objects are deleted within the minimum storage duration based on lifecycle rules

        Data stored in the IA storage class has a minimum storage duration period of 30 days, and data stored in the Archive storage class has a minimum storage duration period of 60 days. The minimum storage duration of an IA or Archive object is calculated from the time specified by the Last-Modified header of the object. For example, OSS converts a Standard object to an IA object 10 days after the object is created, converts the IA object to an Archive object after 20 days, and then deletes the Archive object after five days based on a lifecycle rule. In this case, you are also charged for the storage duration of the remaining 25 days.

    -   Fees incurred when the storage class of objects is converted to Cold Archive and the objects are deleted within the minimum storage duration based on lifecycle rules

        Data stored in the Cold Archive storage class has a minimum storage duration period of 180 days. The minimum storage duration of a Cold Archive object is calculated from the time when the storage class of the object is converted to Cold Archive. For example, OSS converts a Standard object to a Cold Archive object 10 days after the object is created and then deletes the Cold Archive object the next day based on a lifecycle rule. In this case, you are also charged for the storage duration of the remaining 179 days.

    For more information about storage fees for IA, Archive, and Cold Archive objects that are stored for less than the minimum storage duration, see [Storage fees](/intl.en-US/Pricing/Billing items and methods/Storage fees.md).

-   Number of lifecycle rules

    You can configure up to 1,000 lifecycle rules for each bucket.

-   Effective time

    After a lifecycle rule is configured, OSS loads the rule within 24 hours. After the lifecycle rule is loaded, OSS executes the rule every day at 08:00:00 \(UTC+8\) and completes the actions triggered by the rule within 24 hours. The interval between the last modified time of an object and the time when the lifecycle rule is executed must be longer than 24 hours. For example, if you configure a lifecycle rule for a bucket to delete objects one day after they are uploaded, objects that are uploaded on July 20, 2020 are deleted on a different date based on the specific time when the objects are uploaded.

    -   Objects uploaded before 08:00:00 \(UTC+8\) are deleted from 08:00:00 \(UTC+8\) on July 21, 2020 to 08:00:00 \(UTC+8\) on July 22, 2020.
    -   Objects uploaded after 08:00:00 \(UTC+8\) are deleted from 08:00:00 \(UTC+8\) on July 22, 2020 to 08:00:00 \(UTC+8\) on July 23, 2020.
    **Note:** When you update a lifecycle rule, tasks performed based on the rule on the current day are suspended. We recommend that you do not frequently update lifecycle rules.


## Component elements

A lifecycle rule consists of the following elements:

-   Policy: specifies the objects and parts that match the rule.
    -   Match by prefix: indicates that the rule matches objects and parts by prefix. You can create multiple lifecycle rules to configure different prefixes. Each prefix must be unique. The naming conventions for prefixes are the same as those for objects. For more information, see [object](/intl.en-US/Developer Guide/Terms.md).
    -   Match by tag: indicates that the rule matches objects by tag key and tag value. You can specify multiple tags in a single lifecycle rule. The lifecycle rule applies to all objects that have the specified tags. Lifecycle rules cannot match parts by tag.

        **Note:** For more information, see [Object tagging](/intl.en-US/Developer Guide/Objects/Manage files/Configure object tagging.md).

    -   Match by prefix and tag: indicates that the rule matches objects by using specified prefixes and tags.
    -   Match by bucket: indicates that the rule matches all objects and parts stored in the bucket. After you configure a lifecycle rule for a bucket to match all objects and parts in a bucket, other lifecycle rules cannot be configured for the bucket.
-   Object lifecycle policy: specifies the validity period or expiration date of objects and the operation to perform on expired objects.

    -   Validity period: specifies the validity period of objects in unversioned buckets and the current versions of objects in versioned buckets and the operation to perform on these objects after they expire. Objects that match the lifecycle rule are retained for the specified validity period after the objects are last modified. The specified operation is performed on these objects after they expire.
    -   Expiration date: specifies the expiration date of objects in unversioned buckets and the current versions of objects in versioned buckets and the operation to perform on these objects after they expire. All objects that are last modified before this date expire and the specified operation is performed on these objects.
    -   Validity period for the previous versions of objects: specifies the validity period for the previous versions of objects and the operation to perform on these previous versions. Objects that match the lifecycle rule are retained for the specified validity period after the objects become previous versions. The specified operation is performed on these objects after they expire.
    You can configure lifecycle rules to convert the storage class of expired objects or delete expired objects. For more information, see [Configuration elements](/intl.en-US/Developer Guide/Buckets/Lifecycle/Configuration elements.md).

-   Part lifecycle policy: specifies the validity period or expiration date of parts and the operation to perform on these expired parts.
    -   Validity period: specifies the validity period for parts. Parts that match the lifecycle rule are retained within the validity period and are deleted after they expire.
    -   Expiration date: specifies the expiration date of parts. Parts that are last modified before this date expire and are deleted.

## Matching logic

-   A lifecycle rule takes effect

    For example, the following objects are stored in a bucket:

    ```
    logs/program.log.1
    logs/program.log.2
    logs/program.log.3
    doc/readme.txt
    ```

    If the prefix specified by a rule is logs/, the rule applies to the first three objects whose names are prefixed with logs/. If the prefix specified by a rule is doc/readme.txt, the rule applies only to the object doc/readme.txt.

    When GetObject or HeadObject operations are performed on an object based on a lifecycle rule, the `x-oss-expiration` header is contained in the response. This header contains two parameters: `expiry-date` that specifies the expiration date of the object, and `rule-id` that specifies the ID of the matched lifecycle rule.

-   Multiple lifecycle rules conflict
    -   Same prefix and tags are specified in multiple lifecycle rules

        When objects that have the same prefix and tags match multiple lifecycle rules at the same time, lifecycle rules that are configured to delete objects take precedence over lifecycle rules that are configured to convert the storage class of objects. For example, both rule1 and rule2 described in the following table apply to objects that have the abc prefix in their names and have the a=1 tag. rule1 is configured to delete matched objects 20 days after they are last modified. rule2 is configured to convert the storage class of matched objects to Archive 20 days after they are last modified. If rule1 and rule2 are configured for a bucket at the same time, rule2 does not take effect.

        |Rule|Prefix|Tag|Action|
        |----|------|---|------|
        |rule1|abc|a=1|Delete matched objects 20 days after they are last modified.|
        |rule2|abc|a=1|Convert the storage class of matched objects to Archive 20 days after they are last modified.|

    -   Overlapped prefixes and same tags are specified in multiple lifecycle rules

        For example, rule1 described in the following table applies to all objects that have the a=1 tag and is configured to convert the storage class of matched objects to IA 10 days after they are last modified. rule2 described in the following table applies to all objects that have the abc prefix in their names and have the a=1 tag. rule2 is configured to delete matched objects 120 days after they are last modified.

        |Rule|Prefix|Tag|Action|
        |----|------|---|------|
        |rule1|-|a=1|Convert the storage class of matched objects to IA 10 days after they are last modified.|
        |rule2|abc|a=1|Delete matched objects 120 days after they are last modified.|

        rule3 described in the following table applies to all objects that have the a=1 tag and is configured to convert the storage class of matched objects to Archive 20 days after they are last modified. rule4 described in the following table applies to objects that have the abc prefix in their names and have the a=1 tag and is configured to convert the storage class of matched objects to IA 30 days after they are last modified. If rule3 and rule4 are configured for a bucket at the same time, the storage class of objects that have the abc prefix in their names and have the a=1 tag is converted to Archive 20 days after they are last modified based on rule3 first. Archive objects cannot be converted to IA objects. Therefore, rule4 does not take effect.

        |Rule|Prefix|Tag|Action|
        |----|------|---|------|
        |rule3|-|a=1|Convert the storage class of matched objects to Archive 20 days after they are last modified.|
        |rule4|abc|a=1|Convert the storage class of matched objects to IA 30 days after they are last modified.|


## FAQ

-   Does OSS impose a limit on the minimum storage duration of objects whose storage class is converted by using CopyObject?

    OSS imposes limits on the minimum storage duration of IA, Archive, and Cold Archive objects. For example, if you use CopyObject to convert the storage class of an IA object to Archive 10 days after the object is created, you are charged for the entire minimum storage duration of the IA object. In addition, the Last-Modified value of the object is updated to the time when the storage class of the object is converted. The converted Archive object must be stored for at least 60 days. Otherwise, you are charged for the minimum storage duration of Archive objects.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9395688951/p34481.png)

-   Does OSS record operations when the storage class of objects is converted or expired objects are deleted based on lifecycle rules?

    Yes, OSS records operations when the storage class of objects is converted or expired objects are deleted based on lifecycle rules. The logs include the following fields:

    -   Operation
        -   CommitTransition: the storage class to which the object is converted based on lifecycle rules. Example: IA, Archive, or Cold Archive.
        -   ExpireObject: the expired objects that are deleted based on lifecycle rules.
    -   Sync Request

        lifecycle: the operations that are triggered by lifecycle rules, such as deleting expired objects and converting the storage class of objects.


