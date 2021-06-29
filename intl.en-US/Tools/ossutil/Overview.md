# Overview

ossutil allows you to use command lines to manage OSS data. ossutil provides a variety of simple commands for you to manage buckets and objects. The following operating systems support ossutil: Windows, Linux, and macOS.

ossutil allows you to perform the following operations:

-   Manage buckets, such as create, list, and delete buckets.
-   Manage objects, such as upload, download, list, copy, and delete objects.
-   Manage parts, such as list and delete parts.

## Install ossutil

For more information about how to download and install ossutil, see [Download and installation](/intl.en-US/Tools/ossutil/Download and installation.md).

## Common commands

The following table describes common commands that are supported by ossutil.

|Command|Description|
|-------|-----------|
|[appendfromfile \(append upload\)](/intl.en-US/Tools/ossutil/Common commands/appendfromfile (append upload).md)|Uploads a local file to an append object in OSS by using append upload.|
|[bucket-encryption \(server-side encryption\)](/intl.en-US/Tools/ossutil/Common commands/bucket-encryption (server-side encryption).md)|Adds, modifies, queries, or deletes encryption configurations for a bucket.|
|[bucket-policy \(manage bucket policies\)](/intl.en-US/Tools/ossutil/Common commands/bucket-policy (manage bucket policies).md)|Adds, modifies, queries, or deletes bucket policy configurations for a bucket.|
|[bucket-tagging \(manage bucket tags\)](/intl.en-US/Tools/ossutil/Common commands/bucket-tagging (manage bucket tags).md)|Adds, modifies, queries, or deletes tagging configurations for a bucket.|
|[bucket-versioning \(configure or query bucket versioning status\)](/intl.en-US/Tools/ossutil/Common commands/bucket-versioning (configure or query bucket versioning status).md)|Adds or queries versioning configurations for a bucket.|
|[cat](/intl.en-US/Tools/ossutil/Common commands/cat.md)|Exports object content to ossutil.|
|[config](/intl.en-US/Tools/ossutil/Common commands/config.md)|Generates a configuration file to store OSS access information.|
|[cors](/intl.en-US/Tools/ossutil/Common commands/cors.md)|Adds, modifies, queries, or deletes cross-origin resource sharing \(CORS\) configurations for a bucket.|
|[cors-options](/intl.en-US/Tools/ossutil/Common commands/cors-options.md)|Tests whether a bucket allows a specified cross-region request.|
|[Overview](/intl.en-US/Tools/ossutil/Common commands/cp/Overview.md)|Uploads, downloads, or copies objects.|
|[create-symlink \(create a symbolic link\)](/intl.en-US/Tools/ossutil/Common commands/create-symlink (create a symbolic link).md)|Creates a symbolic link.|
|[du \(query object sizes\)](/intl.en-US/Tools/ossutil/Common commands/du (query object sizes).md)|Queries the storage usage of a specified bucket, object, or folder.|
|[getallpartsize](/intl.en-US/Tools/ossutil/Common commands/getallpartsize.md)|Queries the size of each part in a bucket that has not been uploaded and the total size of all of those parts.|
|[hash](/intl.en-US/Tools/ossutil/Common commands/hash.md)|Calculates the CRC-64 value or MD5 hash of a local file.|
|[help](/intl.en-US/Tools/ossutil/Common commands/help.md)|Queries help information about a command. We recommend that you use the help command to query information about a specified command.|
|[inventory \(configure bucket inventories\)](/intl.en-US/Tools/ossutil/Common commands/inventory (configure bucket inventories).md)|Adds, queries, lists, or deletes inventory configurations for a bucket.|
|[lifecycle \(manage lifecycle rules\)](/intl.en-US/Tools/ossutil/Common commands/lifecycle (manage lifecycle rules).md)|Adds, modifies, queries, or deletes lifecycle configurations for a bucket.|
|[listpart](/intl.en-US/Tools/ossutil/Common commands/listpart.md)|Lists the parts that have not been uploaded for an object.|
|[logging](/intl.en-US/Tools/ossutil/Common commands/logging.md)|Adds, modifies, queries, or deletes logging configurations for a bucket.|
|[ls \(list buckets, objects, or parts\)](/intl.en-US/Tools/ossutil/Common commands/ls (list buckets, objects, or parts).md)|Lists buckets, objects, or parts.|
|[mb \(create buckets\)](/intl.en-US/Tools/ossutil/Common commands/mb (create buckets).md)|Creates a bucket.|
|[mkdir \(create directories\)](/intl.en-US/Tools/ossutil/Common commands/mkdir (create directories).md)|Creates a folder in a bucket.|
|[object-tagging \(add, modify, query, and delete object tags\)](/intl.en-US/Tools/ossutil/Common commands/object-tagging (add, modify, query, and delete object tags).md)|Adds, modifies, queries, or deletes tagging configurations for an object.|
|[probe](/intl.en-US/Tools/ossutil/Common commands/probe.md)|Monitors access to OSS. This command can also be used to troubleshoot issues that are caused by network faults or incorrect parameter settings during the upload and download process.|
|[read-symlink \(query a symbolic link\)](/intl.en-US/Tools/ossutil/Common commands/read-symlink (query a symbolic link).md)|Reads the description of a symbolic link object.|
|[referer](/intl.en-US/Tools/ossutil/Common commands/referer.md)|Adds, modifies, queries, or deletes hotlink protection configurations for a bucket.|
|[restore \(restore objects\)](/intl.en-US/Tools/ossutil/Common commands/restore (restore objects).md)|Restores an object from the frozen state to the readable state.|
|[request-payment \(configure pay-by-requester\)](/intl.en-US/Tools/ossutil/Common commands/request-payment (configure pay-by-requester).md)|Configures or queries pay-by-requester configurations for a bucket.|
|[revert-versioning](/intl.en-US/Tools/ossutil/Common commands/revert-versioning.md)|Removes delete markers whose IS\_LATEST are set to true. This way, the latest previous version becomes the current version.|
|[rm \(delete objects, parts, or buckets\)](/intl.en-US/Tools/ossutil/Common commands/rm (delete objects, parts, or buckets).md)|Deletes buckets, objects, or parts.|
|[set-acl \(configure or modify object or bucket ACLs\)](/intl.en-US/Tools/ossutil/Common commands/set-acl (configure or modify object or bucket ACLs).md)|Configures the ACL for a bucket or an object.|
|[set-meta](/intl.en-US/Tools/ossutil/Common commands/set-meta.md)|Configures the metadata for an uploaded object.|
|[sign](/intl.en-US/Tools/ossutil/Common commands/sign.md)|Generates signed URLs for third-party users to access objects in a bucket.|
|[stat](/intl.en-US/Tools/ossutil/Common commands/stat.md)|Obtains the description of a specified bucket or object.|
|[update](/intl.en-US/Tools/ossutil/Common commands/update.md)|Updates the ossutil version.|
|[website](/intl.en-US/Tools/ossutil/Common commands/website.md)|Adds, modifies, queries, or deletes static website hosting and back-to-origin configurations for a bucket.|

