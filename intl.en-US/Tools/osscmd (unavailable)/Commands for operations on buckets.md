# Commands for operations on buckets

This topic describes commands that can be used to manage buckets.

**Note:** Commands supported by the osscmd tool have been integrated with the [ossutil](/intl.en-US/Tools/ossutil/Overview.md) tool. The osscmd tool is no longer available for downloads as of July 31, 2019.

## config

Command:

```
config --id=[accessid] --key=[accesskey] --host=[host] --sts_token=[sts_token]
```

Example:

-   `python osscmd config --id=your_id --key=your_key`
-   ```
python osscmd config --id=your_id --key=your_key
        --host=oss-internal.aliyuncs.com
```


## getallbucket\(gs\)

Command:

`getallbucket(gs)`

Obtain created buckets. gs is short for get allbucket. You can run the gs or allbucket command to obtain a list of created buckets.

Example:

-   `python osscmd getallbucket`
-   `python osscmd gs`

## createbucket\(cb,mb,pb\)

Command:

`createbucket(cb,mb,pb) oss://bucket --acl=[acl]`

Create a bucket.

-   cb is short for create bucket. mb is short for make bucket. pb is short for put bucket.
-   You can set oss://bucket to specify a bucket name.
-   The acl parameter is optional.

Example:

-   `python osscmd createbucket oss://mybucket`
-   `python osscmd cb oss://myfirstbucket --acl=public-read`
-   `python osscmd mb oss://mysecondbucket --acl=private`
-   `python osscmd pb oss://mythirdbucket`

## deletebucket\(db\)

Command:

`deletebucket(db) oss://bucket`

Delete a bucket. db is short for delete bucket.

Example:

-   `python osscmd deletebucket oss://mybucket`
-   `python osscmd db oss://myfirstbucket`

## deletewholebucket

**Warning:** All data is deleted if you run this command. Deleted data cannot be recovered. Exercise caution when you run this command.

Command:

`deletewholebucket oss://bucket`

Delete a bucket, and all objects and fragments in the bucket.

Example:

`python osscmd deletewholebucket oss://mybucket`

## getacl

Command:

`getacl oss://bucket`

Obtain the bucket ACL.

Example:

`python osscmd getacl oss://mybucket`

## setacl

Command:

`setacl oss://bucket --acl=[acl]`

Modify the bucket ACL. You can set the bucket ACL to private, public-read, or public-read-write.

Example:

`python osscmd setacl oss://mybucket --acl=private`

## putlifecycle

Command:

`putlifecycle oss://mybucket lifecycle.xml`

Set lifecycle rules. In the command, lifecycle.xml indicates a file that is used to configure lifecycle rules. For more information, see [API Reference](/intl.en-US/API Reference/Bucket operations/Lifecycle/PutBucketLifecycle.md).

Example:

`python osscmd putlifecycle oss://mybucket lifecycle.xml`

Example:

```
<LifecycleConfiguration>
    <Rule>
        <ID>1125</ID>
        <Prefix>log_backup/</Prefix>
        <Status>Enabled</Status>
        <Expiration>
            <Days>2</Days>
        </Expiration>
    </Rule>
</LifecycleConfiguration>
```

## getlifecycle

Command:

`osscmd getlifecycle oss://bucket`

Obtain lifecycle rules of a bucket.

Example:

`python osscmd getlifecycle oss://mybucket`

## deletelifecycle

Command:

`osscmd deletelifecycle oss://bucket`

Delete all lifecycle rules of a bucket.

Example:

`python osscmd deletelifecycle oss://mybucket`

## putreferer

Command:

```
osscmd putreferer oss://bucket --allow_empty_referer=[true|false]
        --referer=[referer]
```

Set hotlinking protection rules. The `allow_empty_referer` parameter is required and is used to specify whether an empty Referer field is allowed. The `referer` parameter is used to set the Referer whitelist. For example, you can add www.test1.com,www.test2.com to the Referer whitelist. To add multiple domain names, separate the domain names with commas \(,\). For more information about configuration rules, see [Configure hotlinking protection](/intl.en-US/Developer Guide/Data security/Access and control/Configure hotlink protection.md).

Example:

```
python osscmd putreferer oss://mybucket --allow_empty_referer=true
          --referer="www.test1.com,www.test2.com"
```

## getreferer

Command:

`osscmd getreferer oss://bucket`

Obtain the hotlinking protection rule of the bucket.

Example:

`python osscmd getreferer oss://mybucket`

## putlogging

Command:

`osscmd putlogging oss://source_bucket oss://target_bucket/[prefix]`

source\_bucket specifies the bucket that is accessed. target\_bucket specifies the bucket that is used to store the log of access to the source bucket. You can set a prefix for the log that is generated to record access to the source bucket and facilitate log queries.

Example:

`python osscmd getlogging oss://mybucket`

## getlogging

Command:

`osscmd getlogging oss://bucket`

Obtain the access log setting rule of the bucket.

Example:

`python osscmd getlogging oss://mybucket`

