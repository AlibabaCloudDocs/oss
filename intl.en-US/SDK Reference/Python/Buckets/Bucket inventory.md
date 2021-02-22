# Bucket inventory

This topic describes how to add, view, list, and delete the inventory configurations of a bucket.

**Note:** Before you can perform these operations, you must have the appropriate permissions on the bucket. By default, these permissions are available only to the bucket owner. If you do not have the permissions to perform these operations, apply for the permissions from the bucket owner.

## Add inventory configurations

**Note:**

-   You can have up to 1,000 inventory configurations per bucket.
-   The inventory list must be stored in a bucket that is in the same region as the bucket that you want to inventory.

The following code provides an example on how to add an inventory configuration for a bucket:

```
# -*- coding: utf-8 -*-

import oss2
from oss2.models import (InventoryConfiguration,
                        InventoryFilter, 
                        InventorySchedule, 
                        InventoryDestination, 
                        InventoryBucketDestination, 
                        InventoryServerSideEncryptionKMS,
                        InventoryServerSideEncryptionOSS,
                        INVENTORY_INCLUDED_OBJECT_VERSIONS_CURRENT,
                        INVENTORY_INCLUDED_OBJECT_VERSIONS_ALL,
                        INVENTORY_FREQUENCY_DAILY,
                        INVENTORY_FREQUENCY_WEEKLY,
                        INVENTORY_FORMAT_CSV,
                        FIELD_SIZE,
                        FIELD_LAST_MODIFIED_DATE,
                        FIELD_STORAG_CLASS,
                        FIELD_ETAG,
                        FIELD_IS_MULTIPART_UPLOADED,
                        FIELD_ENCRYPTION_STATUS)

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

account_id = '<yourtBucketDestinationAccountId>'
role_arn = '<yourBucketDestinationRoleArn>'
dest_bucket_name = '<yourDestinationBucketName>'

# Specify the inventory ID.
inventory_id = "test-config-id"

# Specify the object attributes to include in the inventory list.
optional_fields = [FIELD_SIZE, FIELD_LAST_MODIFIED_DATE, FIELD_STORAG_CLASS,
        FIELD_ETAG, FIELD_IS_MULTIPART_UPLOADED, FIELD_ENCRYPTION_STATUS]

# Specify the destination bucket to contain the inventory results.
bucket_destination = InventoryBucketDestination(
        # Specify the ID of the account that owns the destination bucket.
        account_id=account_id, 
        # Specify the role ARN of the destination bucket.
        role_arn=role_arn,
        # Specify the name of the destination bucket.
        bucket=dest_bucket_name,
        # Specify the output format of the inventory results.
        inventory_format=INVENTORY_FORMAT_CSV,
        # Specify the prefix of the inventory list object.
        prefix='destination-prefix',
        # The following code provides an example on how to encrypt the inventory list object by using CMKs hosted in KMS.
        # sse_kms_encryption=InventoryServerSideEncryptionKMS("test-kms-id"),
        # The following code provides an example on how to encrypt the inventory list object on the OSS server.
        # sse_oss_encryption=InventoryServerSideEncryptionOSS()
        )

# Create an inventory configuration.
inventory_configuration = InventoryConfiguration(
        # Specify the inventory ID.
        inventory_id=inventory_id,
        # Specify whether the inventory is enabled. True indicates that the inventory list is generated, while false indicates that no inventory list is generated.
        is_enabled=True, 
        # Specify whether to generate the inventory on a daily or weekly basis. The following code provides an example to generate an inventory on a daily basis. WEEKLY indicates once a week. DAILY indicates once a day.
        inventory_schedule=InventorySchedule(frequency=INVENTORY_FREQUENCY_DAILY),
        # Set the inventory list to include only the current version. If you set this parameter to INVENTORY_INCLUDED_OBJECT_VERSIONS_ALL, all versions are inventoried. Versioning must be enabled for this configuration to take effect.
        included_object_versions=INVENTORY_INCLUDED_OBJECT_VERSIONS_CURRENT,
        # Specify the prefix of object names to filter objects to include in the inventory list.
        inventory_filter=InventoryFilter(prefix="obj-prefix"),
        # Specify the object attributes to include in the inventory list.
        optional_fields=optional_fields,
        # Specify the destination for the inventory results.
        inventory_destination=InventoryDestination(bucket_destination=bucket_destination))

# Upload the inventory configuration.
bucket.put_bucket_inventory_configuration(inventory_configuration)
```

For more information about how to add an inventory configuration for a bucket, see [PutBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/PutBucketInventory.md).

## View inventory configurations

The following code provides an example on how to view the inventory configurations of a bucket:

```
# -*- coding: utf-8 -*-

import oss2
import os

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Specify the inventory ID.
inventory_id = "test-config-id"

# Obtain the inventory configuration.
result = bucket.get_bucket_inventory_configuration(inventory_id=inventory_id);

# Obtain the information about the inventory configuration.
print('======inventory configuration======')
print('inventory_id', result.inventory_id)
print('is_enabled', result.is_enabled)
print('frequency', result.inventory_schedule.frequency)
print('included_object_versions', result.included_object_versions)
print('inventory_filter prefix', result.inventory_filter.prefix)
print('fields', result.optional_fields)
bucket_destin = result.inventory_destination.bucket_destination
print('===bucket destination===')
print('account_id', bucket_destin.account_id)
print('role_arn', bucket_destin.role_arn)
print('bucket', bucket_destin.bucket)
print('format', bucket_destin.inventory_format)
print('prefix', bucket_destin.prefix)
if bucket_destin.sse_kms_encryption is not None:
    print('server side encryption by kms, key id:', bucket_destin.sse_kms_encryption.key_id)
elif bucket_destin.sse_oss_encryption is not None:
    print('server side encryption by oss.')
```

For more information about how to view inventory configurations, see [GetBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/GetBucketInventory.md).

## List multiple inventory configurations at a time

**Note:** You can obtain up to 100 inventory configurations per request. To obtain more than 100 inventory configurations, you must send multiple requests. Each subsequent request must use the token returned in the previous request.

The following code provides an example on how to list all inventory configurations of a bucket at a time:

```
# -*- coding: utf-8 -*-

import oss2
import os

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Obtain the information about the inventory configuration.
def print_inventory_configuration(configuration):
    print('======inventory configuration======')
    print('inventory_id', configuration.inventory_id)
    print('is_enabled', configuration.is_enabled)
    print('frequency', configuration.inventory_schedule.frequency)
    print('included_object_versions', configuration.included_object_versions)
    print('inventory_filter prefix', configuration.inventory_filter.prefix)
    print('fields', configuration.optional_fields)
    bucket_destin = configuration.inventory_destination.bucket_destination
    print('===bucket destination===')
    print('account_id', bucket_destin.account_id)
    print('role_arn', bucket_destin.role_arn)
    print('bucket', bucket_destin.bucket)
    print('format', bucket_destin.inventory_format)
    print('prefix', bucket_destin.prefix)
    if bucket_destin.sse_kms_encryption is not None:
        print('server side encryption by kms, key id:', bucket_destin.sse_kms_encryption.key_id)
    elif bucket_destin.sse_oss_encryption is not None:
        print('server side encryption by oss.')

# List all inventory configurations.
# If the results are more than 100, the results are returned in pages. The pagination information is stored in the <oss2.models.ListInventoryConfigurationResult> class.
continuation_token = None
while 1:
    result = bucket.list_bucket_inventory_configurations(continuation_token=continuation_token)
    # Obtain whether the results are listed by page.
    print('is truncated', result.is_truncated)
    # Obtain the token to carry in this list.
    print('continuaiton_token', result.continuaiton_token)
    # Obtain the token to carry in the subsequent list.
    print('next_continuation_token', result.next_continuation_token)

    # Obtain the information about the inventory configuration.
    for inventory_config in result.inventory_configurations:
        print_inventory_configuration(inventory_config)

    # If the results are listed by page, continue the listing. The subsequent list must carry the continuation token.
    if result.is_truncated:
        continuation_token = result.next_continuation_token
    else:
         break
```

For more information about how to list multiple inventory configurations at a time, see [ListBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/ListBucketInventory.md).

## Delete inventory configurations

The following code provides an example on how to delete the inventory configurations of a bucket:

```
# -*- coding: utf-8 -*-

import oss2
import os

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Specify the inventory ID.
inventory_id = "test-config-id"

# Delete the inventory configuration specified by the inventory ID.
bucket.delete_bucket_inventory_configuration(inventory_id)
```

For more information about how to delete the inventory configurations of a bucket, see [DeleteBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/DeleteBucketInventory.md).

