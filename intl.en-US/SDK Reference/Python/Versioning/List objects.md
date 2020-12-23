# List objects

This topic describes how to list all objects, a specified number of objects, and objects whose names contain a specified prefix in a versioned bucket.

## Lists the versions of all objects in a bucket

The following code provides an example on how to list the versions of all objects including delete markers in a bucket:

```
# -*- coding: utf-8 -*-
import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Use the list_object_versions operation to list the versions of objects in a versioned bucket.
# List the versions of all objects including delete markers in a bucket.
result = bucket.list_object_versions()

# List the versions of all objects in the bucket.
next_key_marker = None
next_versionid_marker = None
while True:
    result = bucket.list_object_versions(key_marker=next_key_marker, versionid_marker=next_versionid_marker)

    # Display the version information about the listed objects.
    for version_info in result.versions:
        print('version_info.versionid:', version_info.versionid)
        print('version_info.key:', version_info.key)
        print('version_info.is_latest:', version_info.is_latest)

    # Display the version information of the listed delete markers.
    for del_maker_Info in result.delete_marker:
        print('del_maker.key:', del_maker_Info.key)
        print('del_maker.versionid:', del_maker_Info.versionid)
        print('del_maker.is_latest:', version_info.is_latest)

    is_truncated = result.is_truncated

    # Check whether the versions of all objects are listed. If not the versions of all objects are listed, continue the list operation. If the versions of all objects are listed, the list operation ends.
    if is_truncated:
        next_key_marker = result.next_key_marker
        next_versionid_marker = result.next_versionid_marker
    else:
        break
```

## List the versions of objects whose names contain the specified prefix

The following code provides an example on how to list the versions of objects whose names contain the specified prefix:

```
# -*- coding: utf-8 -*-
import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Use the list_object_versions operation to list the versions of objects in a versioned bucket.
# List the versions of all objects including delete markers in a bucket.
result = bucket.list_object_versions()

# List the versions of objects whose names contain the test- prefix.
prefix = 'test-'
next_key_marker = None
next_versionid_marker = None
while True:
    result = bucket.list_object_versions(prefix=prefix, key_marker=next_key_marker, versionid_marker=next_versionid_marker)

    # Display the version information of the listed objects.
    for version_info in result.versions:
        print('version_info.versionid:', version_info.versionid)
        print('version_info.key:', version_info.key)
        print('version_info.is_latest:', version_info.is_latest)

    # Display the version information of the listed delete markers.
    for del_maker_Info in result.delete_marker:
        print('del_maker.key:', del_maker_Info.key)
        print('del_maker.versionid:', del_maker_Info.versionid)
        print('del_maker.is_latest:', version_info.is_latest)

    is_truncated = result.is_truncated

    # Check whether the versions of all objects whose names contain the test- prefix are listed. If not the versions of all objects whose names contain the test- prefix are listed, continue the list operation. If the versions of all objects whose names contain the test- prefix are listed, the list operation ends.
    if is_truncated:
        next_key_marker = result.next_key_marker
        next_versionid_marker = result.next_versionid_marker
    else:
        break
```

## List a specific number of object versions

The following code provides an example on how to list a specific numbers of object versions:

```
# -*- coding: utf-8 -*-
import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Use the list_object_versions operation to list the versions of objects in a versioned bucket.
# List the versions of all objects including delete markers in a bucket.
result = bucket.list_object_versions()

# List up to 200 object versions.
max_keys = 200

result = bucket.list_object_versions(max_keys=max_keys)

# Display the version information of the listed objects.
for version_info in result.versions:
    print('version_info.versionid:', version_info.versionid)
    print('version_info.key:', version_info.key)
    print('version_info.is_latest:', version_info.is_latest)

# Display the version information of the listed delete markers.
for del_maker_Info in result.delete_marker:
    print('del_maker.key:', del_maker_Info.key)
    print('del_maker.versionid:', del_maker_Info.versionid)
    print('del_maker.is_latest:', version_info.is_latest)

# Check whether the listed result is truncated.
# If the number of objects in the bucket is larger than 200, the value of is_truncated is True, which indicates that the listed result is truncated. If the number of objects in the bucket is smaller than 200, the value of is_truncated is False, which indicates that the listed result is not truncated.
print('is truncated', result.is_truncated)
```

## List objects by folder

OSS does not use a hierarchical structure for objects, but instead uses a flat structure. All elements are stored as objects in buckets. A folder is an object 0 KB in size whose name ends with a forward slash \(/\). You can upload and download this object. By default, objects whose names end with a forward slash \(/\) are displayed as folders in the OSS console.

You can use the delimiter and prefix parameters to list objects by folder.

-   If you set prefix to a folder name in the request, objects and subfolders whose names contain the prefix are listed.
-   If you also set delimiter to a forward slash \(/\) in the request, the objects and subfolders whose names start with the specified prefix in the folder are listed. Each subfolder is listed as a single result element in commonPrefixes. The objects and folders in these subfolders are not listed.

Example: A bucket named examplebucket contains the following objects: oss.jpg, fun/test.jpg, fun/movie/001.avi, and fun/movie/007.txt. The forward slash \(/\) is used as the folder delimiter. The following figure shows the structure of the bucket named examplbucket.

```
examplebucket           
 └── oss.jpg
 └── fun               
      └── test.jpg
      └── movie
           └── 001.avi
           └── 007.txt
```

The following examples describe how to list objects by simulating folders.

-   List the versions of objects in the root folder

    The following code provides an example on how to list the versions of objects in the root folder:

    ```
    # -*- coding: utf-8 -*-
    import oss2
    
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    # The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
    
    # Use the list_object_versions operation to list the versions of objects in a versioned bucket.
    # List the versions of all objects including delete markers in a bucket.
    result = bucket.list_object_versions()
    
    # Specify the delimiter as a forward slash (/).
    delimiter = "/"
    next_key_marker = None
    next_versionid_marker = None
    while True:
        result = bucket.list_object_versions(delimiter=delimiter, key_marker=next_key_marker, versionid_marker=next_versionid_marker)
    
        # Display the version information of the listed objects.
        for version_info in result.versions:
            print('version_info.versionid:', version_info.versionid)
            print('version_info.key:', version_info.key)
            print('version_info.is_latest:', version_info.is_latest)
    
        # Display the version information of the listed delete markers.
        for del_maker_Info in result.delete_marker:
            print('del_maker.key:', del_maker_Info.key)
            print('del_maker.versionid:', del_maker_Info.versionid)
            print('del_maker.is_latest:', version_info.is_latest)
    
        # Display the folders whose names end with a forward slash (/).
        for common_prefix in result.common_prefix:
            print("common_prefix:", common_prefix)
    
        is_truncated = result.is_truncated
    
        # Check whether the versions of all objects in the root folder are listed. If not the versions of all objects in the root folder are listed, continue the list operation. If the versions of all objects in the root folder are listed, the list operation ends.
        if is_truncated:
            next_key_marker = result.next_key_marker
            next_versionid_marker = result.next_versionid_marker
        else:
            break
    ```

    Returned results:

    ```
    ('version_info.versionid:', 'CAEQEhiBgMCw8Y7FqBciIGIzMDE3MTEzOWRiMDRmZmFhMmRlMjljZWI0MWU4****')
    ('version_info.key:', 'oss.jpg')
    ('version_info.is_latest:', True)
    ('common_prefix:', 'fun/')
    ```

-   List objects and subfolders within a folder

    The following code provides an example on how to list objects and subfolders within a folder in a bucket:

    ```
    # -*- coding: utf-8 -*-
    import oss2
    
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    # The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
    
    # Use the list_object_versions operation to list the versions of objects in a versioned bucket.
    # List the versions of all objects including delete markers in a bucket.
    result = bucket.list_object_versions()
    
    # Specify the delimiter as a forward slash (/) and prefix as fun/.
    prefix = "fun/"
    delimiter = "/"
    next_key_marker = None
    next_versionid_marker = None
    while True:
        result = bucket.list_object_versions(prefix=prefix, delimiter=delimiter, key_marker=next_key_marker, versionid_marker=next_versionid_marker)
    
        # Display the version information of the listed objects.
        for version_info in result.versions:
            print('version_info.versionid:', version_info.versionid)
            print('version_info.key:', version_info.key)
            print('version_info.is_latest:', version_info.is_latest)
    
        # Display the version information of the listed delete markers.
        for del_maker_Info in result.delete_marker:
            print('del_maker.key:', del_maker_Info.key)
            print('del_maker.versionid:', del_maker_Info.versionid)
            print('del_maker.is_latest:', version_info.is_latest)
    
        # Display the folders whose names end with a forward slash (/).
        for common_prefix in result.common_prefix:
            print("common_prefix:", common_prefix)
    
        is_truncated = result.is_truncated
    
        # Check whether all objects and subfolders within the folder are listed. If not all objects and subfolders within the folder are listed, continue the list operation. If all objects and subfolders within the folder are listed, the list operation ends.
        if is_truncated:
            next_key_marker = result.next_key_marker
            next_versionid_marker = result.next_versionid_marker
        else:
            break
    ```

    Returned results:

    ```
    ('version_info.versionid:', 'CAEQFRiBgMCh9JDkrxciIGE3OTNkYzFhYTc2YzQzOTQ4Y2MzYjg2YjQ4ODg*****')
    ('version_info.key:', 'fun/test.jpg')
    ('version_info.is_latest:', True)
    ('commonPrefix:', 'fun/movie/')
    ```


