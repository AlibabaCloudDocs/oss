# Lifecycle rules

You can call PutBucketLifecycle to configure lifecycle rules. After the rules are configured, the storage class of expired objects is converted to IA, Archive, or Cold Archive, or the objects and parts are deleted, which minimizes costs.

**Note:** For more information about the PutBucketLifecycle operation, see [PutBucketLifecycle](/intl.en-US/API Reference/Bucket operations/Lifecycle/PutBucketLifecycle.md).

## Scenarios

You can configure a lifecycle rule to regularly convert the storage class of non-hot data to IA, Archive, or Cold Archive, and delete data that is no longer accessed, which minimizes human resource or storage costs. Examples:

-   A medical institution stores their medical records in OSS. These objects are occasionally accessed within six months after they are uploaded, and almost never after that. A lifecycle rule can be used to convert the storage class of these objects to Archive 180 days after they are uploaded.
-   A company stores the call records of their hotline service in OSS. These objects are frequently accessed within the first two months, occasionally after two months, and almost never after six months. After two years, these objects no longer need to be stored. A lifecycle rule can be used to convert the storage class of these objects to IA 60 days after they are uploaded and to Archive 180 days after they are uploaded, and delete them 730 days after they are uploaded.
-   You can manually delete up to 1,000 objects each time. If a bucket contains more than 1,000 objects, you must perform multiple delete operations. You can configure a lifecycle rule that applies to the whole bucket to delete all objects in the bucket after one day.

## Implementation methods

|Implementation method|Description|
|---------------------|-----------|
|[Console](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure lifecycle rules.md)|A user-friendly and intuitive web application|
|[ossutil](/intl.en-US/Tools/ossutil/Common commands/lifecycle.md)|A high-performance command-line tool|
|[Java SDK](/intl.en-US/SDK Reference/Java/Buckets/Manage lifecycle rules.md)|SDK demos for a variety of programming languages|
|[Python SDK](/intl.en-US/SDK Reference/Python/Buckets/Manage lifecycle rules.md)|
|[PHP SDK](/intl.en-US/SDK Reference/PHP/Buckets/Manage lifecycle rules.md)|
|[Go SDK](/intl.en-US/SDK Reference/Go/Buckets/Manage lifecycle rules.md)|
|[C SDK](/intl.en-US/SDK Reference/C/Buckets/Manage lifecycle rules.md)|
|[.NET SDK](/intl.en-US/SDK Reference/. NET/Buckets/Manage lifecycle rules.md)|
|[Node.js SDK](/intl.en-US/SDK Reference/Node. js/Buckets/Manage lifecycle rules.md)|
|[Ruby SDK](/intl.en-US/SDK Reference/Ruby/Buckets/Manage lifecycle rules.md)|

## Usage notes

-   Fees

    Requests that asynchronously trigger lifecycle rule actions are recorded in access logs and incur related request fees. Requests that fail to trigger lifecycle rule actions are not recorded or charged.

-   Maximum available rules

    You can configure up to 1,000 lifecycle rules for each bucket.

-   Effective time

    After a lifecycle rule is configured, it is loaded within 24 hours, and is implemented within 24 hours. After the lifecycle rule is loaded, OSS runs the rule every day at 08:00:00 \(UTC+8\) and implements the rule within 24 hours. The interval between the last modified time of the object and the time for the lifecycle rule to run must be greater than 24 hours. For example, if a lifecycle rule specifies that objects are deleted one day after they are uploaded, the time to delete the object that is uploaded on July 20, 2020 is determined based on the following conditions:

    -   Objects uploaded before 08:00:00 \(UTC+8\) are deleted from 08:00:00 \(UTC+8\) on July 21, 2020 to 08:00:00 \(UTC+8\) on July 22.
    -   Objects uploaded after 08:00:00 \(UTC+8\) are deleted from 08:00:00 \(UTC+8\) on July 22, 2020 to 08:00:00 \(UTC+8\) on July 23.
    **Note:** When you update lifecycle rules, the corresponding lifecycle tasks on the current day are suspended. We recommend that you do not frequently update lifecycle rules.

-   Prefix and tag
    -   The naming conventions for prefixes are the same as those for objects.
    -   If a rule does not specify any prefixes, the rule applies to all objects in a bucket.
    -   The prefixes specified by different rules cannot overlap. If a bucket has two rules that specify prefixes logs/ and logs/program, OSS returns an error.
    -   The tag key is required. The key and value of a tag can contain letters, digits, spaces, and special characters such as + - = . \_ : /.
    -   Object name prefixes can overlap when you configure a combination of object name prefixes and tags in rules. For example, a bucket has two rules configured: Rule 1 specifies the prefix for object names as logs/ and tags as K1=V1. Rule 2 specifies the prefix for object names as logs/program and tags as K1=V2. OSS triggers operations on the objects whose names are prefixed with logs/program and for which tagging configurations are K1=V1 \(Rule 1\) or K1=V2 \(Rule 2\), and objects whose names are prefixed with logs/ and for which tagging configurations are K1=V1 \(Rule 1\).

## Lifecycle rule configurations

Each lifecycle rule contains the following information:

-   Policy: specifies the mode to match objects and parts.
    -   Match by prefix: matches objects and parts by prefix. You can create multiple rules to configure different prefixes. Each prefix must be unique.
    -   Match by tag: matches objects by tag key and tag value. You can configure multiple tags for a single rule. OSS runs lifecycle rules for all objects that have these tags. Tags cannot be configured for parts to match lifecycle rules.

        **Note:** For more information, see [Object tagging](/intl.en-US/Developer Guide/Objects/Manage files/Configure object tagging.md).

    -   Match by prefix and tag: matches objects by specifying a prefix and one or more tags.
    -   Match by bucket: matches all objects and parts contained in the bucket. You can create only one rule if you select this method.
-   Object lifecycle policy: specifies the validity period or expiration date and the operation to perform on these objects when they expire.

    -   Validity period: specifies the number of days for unversioned and current objects and the operation to perform on these objects. Objects that meet the specified conditions are retained for the specified period of time after the objects are last modified. The specified operation is performed on these objects when they expire.
    -   Expiration date: specifies a date for unversioned and current objects and the operation to perform on these objects. All objects that are last modified before this date expire and the specified operation is performed on these objects.
    -   Validity period for the previous versions of objects: specifies the number of days for the previous versions of objects and the operation to perform on these objects. Objects that meet the specified conditions are retained for the specified period of time after the objects become previous versions. The specified operation is performed on these objects when they expire.
    **Note:** You can convert the storage classes of these objects to IA, Archive, or Cold Archive, or delete these objects that match the conditions. For more information, see [Configuration elements](/intl.en-US/Developer Guide/Objects/Object lifecycle/Configuration elements.md).

-   Part lifecycle policy: specifies the validity period or expiration date and the operation to perform on these parts when they expire.
    -   Validity period: specifies the number of days for which to retain parts after they are last modified and the delete operation to perform on these parts when they expire.
    -   Expiration date: specifies a date based on which to delete parts. Parts that are last modified before this date expire.

## Matching logic of lifecycle rules

-   Matching logic

    A rule applies to an object if the object name prefix matches the prefix specified in the rule. For example, a bucket contains the following objects:

    ```
    logs/program.log.1
    logs/program.log.2
    logs/program.log.3
    doc/readme.txt
    ```

    If the prefix specified by a rule is logs/, the rule applies to the first three objects that are prefixed with logs/. If the prefix specified by a rule is doc/readme.txt, the rule applies only to the object doc/readme.txt.

    You can also configure rules to delete expired objects. For example, you can configure a rule to delete objects whose names are prefixed with logs/ and that are last modified 30 days ago, or to delete the doc/readme.txt object on a specific date.

    When an object matches such a rule, OSS includes the x-oss-expiration header in the response to the GetObject or HeadObject request. This header contains two parameters: expiry-date that specifies the expiration date of the object, and rule-id that specifies the ID of the matched rule.

-   Matching conflicts
    -   Rules that have the same prefixing and tagging configurations

        |rule|prefix|tag|action|
        |----|------|---|------|
        |rule1|abc|a=1|Delete objects 20 days after they are last modified|
        |rule2|abc|a=1|Convert the storage class of the objects to Archive 20 days after they are last modified|

        Results: All objects whose names are prefixed with abc and for which tagging configurations are a=1 are deleted 20 days after they are last modified. In this case, objects have been deleted and rule2 makes no sense.

        |rule|prefix|tag|action|
        |----|------|---|------|
        |rule1|abc|a=1|Convert the storage class of objects to IA 365 days after they are last modified|
        |rule2|abc|a=1|Convert the storage class of objects to Archive if they are last modified before March 1, 2018|

        Results: The storage class of all objects whose names are prefixed with abc and for which tagging configurations are a=1 is converted to Archive if objects match the two rules at the same time. If objects match only one rule, OSS runs the specified rule.

    -   Rules that have the overlapping prefixing and same tagging configurations

        |rule|prefix|tag|action|
        |----|------|---|------|
        |rule1|-|a=1|Convert the storage class of the objects to IA 20 days after they are last modified|
        |rule2|abc|a=1|Delete objects 120 days after they are last modified|

        Results: The storage class of all objects for which tagging configurations are a=1 is converted to IA 20 days after they are last modified. Objects whose names are prefixed with abc and for which tagging configurations are a=1 are deleted 120 days after they are last modified.

        |rule|prefix|tag|action|
        |----|------|---|------|
        |rule1|-|a=1|Convert the storage class of the objects to Archive 10 days after they are last modified|
        |rule2|abc|a=1|Convert the storage class of the objects to IA 20 days after they are last modified|

        Results: The storage class of all objects for which tagging configurations are a=1 is converted to Archive 10 days after they are last modified. rule2 becomes invalid because the storage class of objects cannot be converted from Archive to IA.


## FAQ

-   Does OSS require a minimum storage duration when I set lifecycle rules to convert the storage class of objects or delete expired objects?
    -   OSS does not require a minimum storage duration when you set lifecycle rules to convert the storage class of objects. OSS requires a minimum storage duration when you convert the storage class of objects by calling CopyObject.

        Example 1: You set a lifecycle rule to convert the storage class of an object from IA to Archive 10 days after the object is created. The object creation time does not change. After the IA object converts to the Archive object, the Archive object must be stored for at least 50 days.

        ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8395688951/p34477.png)

        Example 2: An IA object is overwritten by CopyObject 10 days after the object is created, and the IA object is converted to the Archive object. This way, you are charged for the minimum storage duration, including the remaining duration. The converted Archive object must be stored for at least 60 days.

        ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9395688951/p34481.png)

    -   OSS requires a minimum storage duration when you set lifecycle rules to delete the storage class of objects.

        IA objects are stored for at least 30 days. Archive objects are stored for at least 60 days. Cold Archive objects are stored for at least 180 days. When you delete these objects after they are stored for a period less than the minimum storage duration, you are charged for the minimum storage duration, including the remaining duration. For more information, see [Storage fees](/intl.en-US/Pricing/Billing items and methods/Storage fees.md).

-   Does OSS record operations when the storage classes of objects are converted and expired objects are deleted based on lifecycle rules?

    Yes, OSS records operations when the storage classes of objects are converted and expired objects are deleted based on lifecycle rules. The logs include the following fields:

    -   Operation
        -   CommitTransition: The lifecycle rule is set to convert the storage class of an object.
        -   ExpireObject: The lifecycle rule is set to delete an expired object.
    -   Sync Request

        lifecycle: The operation to be performed based on the lifecycle rule.


