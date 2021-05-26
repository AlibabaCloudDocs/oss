# Object tagging and lifecycle rules

Lifecycle rules can take effect on objects configured with specified prefixes or tags. You can also specify a combination of prefixes and tags as conditions of lifecycle rules at the same time.

**Note:** If you configure tag conditions, the rule applies only to objects that meet the tag key and value conditions. If a prefix and multiple object tags are configured in a rule, the rule applies only to objects that match the prefix and object tag conditions.

## Add tagging configurations to a lifecycle rule

The following code provides an example on how to add tagging configurations to a lifecycle rule:

```
# -*- coding: utf-8 -*-
import oss2
import datetime
from oss2.models import (LifecycleExpiration, LifecycleRule, 
                        BucketLifecycle,AbortMultipartUpload, 
                        TaggingRule, Tagging, StorageTransition)

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to Object Storage Service (OSS) because the account has permissions on all API operations. We recommend that you use a Resource Access Management (RAM) user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
auth = oss2.Auth('yourAccessKeyId', 'yourAccessKeySecret')
# Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
# Specify the bucket name. 
bucket = oss2.Bucket(auth, 'yourEndpoint', 'examplebucket')

# Specify that objects expire three days after they are last modified. 
# Set the name of the expiration rule and the prefix to match the objects. 
rule1 = LifecycleRule('rule1', 'tests/',
                      # Enable the expiration rule. 
                      status=LifecycleRule.ENABLED,
                      # Set the validity period to three days after the last modified date. 
                      expiration=LifecycleExpiration(days=3))

# Specify that the objects last modified before the specified date expire. 
# Set the name of the expiration rule and the prefix to match the objects. 
rule2 = LifecycleRule('rule2', 'logging-',
                      # Disable the expiration rule. 
                      status=LifecycleRule.DISABLED,
                      # Specify that the objects last modified before the specified date expire. 
                      expiration=LifecycleExpiration(created_before_date=datetime.date(2018, 12, 12)))

# Specify that parts expire three days after they are last modified. 
rule3 = LifecycleRule('rule3', 'tests1/',
                      status=LifecycleRule.ENABLED,
                      abort_multipart_upload=AbortMultipartUpload(days=3))

# Specify that the parts last modified before the specified date expire. 
rule4 = LifecycleRule('rule4', 'logging1-',
                      status=LifecycleRule.DISABLED,
                      abort_multipart_upload = AbortMultipartUpload(created_before_date=datetime.date(2018, 12, 12)))

# Configure tags to match objects. 
tagging_rule = TaggingRule()
tagging_rule.add('key1', 'value1')
tagging_rule.add('key2', 'value2')
tagging = Tagging(tagging_rule)

# Configure the rule to convert the storage class of objects. Specify that the storage class of objects is converted to Archive 365 days after the objects are last modified.  
# Tags to match objects are specified in rule5. The rule applies only to objects that match tag conditions of key1=value1 and key2=value2. 
rule5 = LifecycleRule('rule5', 'logging2-',
                      status=LifecycleRule.ENABLED,
                      storage_transitions=[StorageTransition(days=365, storage_class=oss2.BUCKET_STORAGE_CLASS_ARCHIVE)],
                      tagging = tagging)

lifecycle = BucketLifecycle([rule1, rule2, rule3, rule4, rule5])

bucket.put_bucket_lifecycle(lifecycle)
```

## View the tagging configurations of a lifecycle rule

The following code provides an example on how to view the tagging configurations of a lifecycle rule:

```
# -*- coding: utf-8 -*-
import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
auth = oss2.Auth('yourAccessKeyId', 'yourAccessKeySecret')
# Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
# Specify the bucket name. 
bucket = oss2.Bucket(auth, 'yourEndpoint', 'examplebucket')

# View the lifecycle rules. 
lifecycle = bucket.get_bucket_lifecycle()

for rule in lifecycle.rules:
    # View the part expiration rules. 
    if rule.abort_multipart_upload is not None:
        print('id={0}, prefix={1}, tagging={2}, status={3}, days={4}, created_before_date={5}'
                .format(rule.id, rule.prefix, rule.tagging, rule.status,
                    rule.abort_multipart_upload.days,
                    rule.abort_multipart_upload.created_before_date))

    # View the object expiration rules. 
    if rule.expiration is not None:
        print('id={0}, prefix={1}, tagging={2}, status={3}, days={4}, created_before_date={5}'
                .format(rule.id, rule.prefix, rule.tagging, rule.status,
                    rule.expiration.days,
                    rule.expiration.created_before_date))
    # View the rules to convert the storage class. 
    if len(rule.storage_transitions) > 0: 
        storage_trans_info = ''
        for storage_rule in rule.storage_transitions:
            storage_trans_info += 'days={0}, created_before_date={1}, storage_class={2} **** '.format(
                storage_rule.days, storage_rule.created_before_date, storage_rule.storage_class)

        print('id={0}, prefix={1}, tagging={2}, status={3},, StorageTransition={4}'
                .format(rule.id, rule.prefix, rule.tagging, rule.status, storage_trans_info))
```

