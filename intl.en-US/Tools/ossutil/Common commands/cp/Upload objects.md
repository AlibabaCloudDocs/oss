# Upload objects

You can use the cp command to upload objects from your local computer to OSS.

**Note:**

-   When you use the cp command to upload objects, multipart upload and resumable upload are used by default. If the upload of an object is unexpectedly interrupted before the object upload is complete, the uploaded content is stored as parts in an OSS bucket. To avoid additional storage fees, we recommend that you use the following methods to delete these parts if you no longer use these parts.
    -   Manually delete parts. For more information, see [Delete parts](/intl.en-US/Tools/ossutil/Common commands/rm.md).
    -   Use lifecycle rules to automatically delete parts. For more information, see [Configure lifecycle rules](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure lifecycle rules.md).
-   For more information about the format and supported parameters of the cp command, see [Overview](/intl.en-US/Tools/ossutil/Common commands/cp/Overview.md).

## Sample environment

In this topic, local files or folders are uploaded from your local computer to OSS in Linux. You can modify the corresponding parameters based on your operating system and environment. This topic involves the following common examples:

-   Local file: examplefile.txt \(file in the root folder\)
-   Local folder: localfolder \(folder in the root folder\)
-   OSS bucket: examplebucket
-   Folder specified in the OSS bucket: desfolder

## Examples for simple upload

ossutil allows you to use the following commands to upload local files to OSS.

-   Upload a single object

    If the name of the local file that you want to upload is not specified, the original name is used by default. If the name of the local file is specified, the object is saved in OSS based on the specified name.

    ```
    ./ossutil cp examplefile.txt oss://examplebucket/desfolder/
    ```

-   Upload a folder

    You can add the -r option to the cp command to upload the required folder to OSS.

    ```
    ./ossutil cp -r localfolder/ oss://examplebucket/desfolder/
    ```

    **Note:** If the object to upload is a symbolic link that points to a local folder, you must add a forward slash \(/\) to the symbolic link when you use the cp command to upload the object. Example:

    ```
    ./ossutil cp -r symbolic_link/ oss://examplebucket/desfolder/
    ```

-   Upload a single object and specify the --meta option.

    When you upload an object, you can use the --meta option to configure object metadata in the `header:value#header:value...` format.

    ```
    ./ossutil cp examplefile.txt oss://examplebucket/desfolder/examplefile.txt --meta=Cache-Control:no-cache#Content-Encoding:gzip
    ```

    **Note:** For more information about how to configure metadata, see [set-meta](/intl.en-US/Tools/ossutil/Common commands/set-meta.md).

-   Upload a folder without uploading existing objects

    When batch upload fails, you can specify the --update \(abbreviated as -u\) option to skip the uploaded objects for incremental upload. Command:

    ```
    ./ossutil cp -r localfolder/ oss://examplebucket/desfolder/ -u
    ```

-   Upload an object to a bucket that has pay-by-requester enabled

    ```
    ./ossutil cp localfolder/examplefile.txt oss://examplebucket/ --payer=requester
    ```

-   Upload objects only in the current folder and ignore subfolders

    ```
    ./ossutil cp localfolder/ oss://examplebucket/desfolder/ --only-current-dir -r
    ```

-   Do not generate an object for an uploaded folder

    The OSS folder is an object that is 0 KB in size and whose name ends with a forward slash \(/\). If you specify the --disable-dir-object option when you upload objects, ossutil does not generate an object for the folder to which the objects belong. However, you can view the corresponding folder structure in the OSS console. If you delete objects from the folder, the folder is also deleted.

    ```
    ./ossutil cp localfolder/ oss://examplebucket/desfolder/ --disable-dir-object -r
    ```

-   Upload objects in a subfolder to which a symbolic link points

    ```
    ./ossutil cp localfolder/ oss://examplebucket/desfolder/ --enable-symlink-dir -r
    ```

-   Ignore all objects in the subfolder to which the symbolic link points and the subfolder to which the symbolic link points during object upload

    ```
    ./ossutil cp localfolder/ oss://examplebucket/desfolder/ -r --disable-all-symlink
    ```


## Set the maximum upload speed for an object when you upload the object

When you upload an object or a folder, you can use the --maxupspeed option to set the maximum upload speed in KB/s. The default value is 0, which indicates that the maximum upload speed is unlimited. Commands:

-   Set the maximum upload speed to 1 MB/s for a folder when you upload the folder

    ```
    ./ossutil cp examplefile.txt oss://examplebucket/desfolder/ --maxupspeed 1024
    ```

-   Set the maximum upload speed to 1 MB/s for a folder when you upload the folder

    ```
    ./ossutil cp -r localfolder/ oss://examplebucket/desfolder/ --maxupspeed 1024
    ```


## Configure tagging for an object when you upload the object

You can add the --tagging option to configure object tags when you upload objects. Separate multiple tags with ampersands \(&\). Commands:

```
./ossutil cp examplefile.txt oss://examplebucket/desfolder/ --tagging "abc=1&bcd=2&……"
```

For more information about object tagging, see [object-tagging](/intl.en-US/Tools/ossutil/Common commands/object-tagging.md).

## Set the storage class of an object when you upload the object

When you upload an object, you can use the --meta option to set the storage class of the object. Valid values:

-   Standard
-   IA: infrequent access
-   Archive

If the storage class is not specified when the object is uploaded, the storage class of the bucket prevails. For more information, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md). Commands:

-   Upload an object and set its storage class to IA

    ```
    ./ossutil cp examplefile.txt oss://examplebucket/desfolder/ --meta X-oss-Storage-Class:IA
    ```

-   Upload a folder and set the storage class of all objects in the specified folder to Standard

    ```
    ./ossutil cp localfolder/ oss://examplebucket/desfolder/ --meta X-oss-Storage-Class:Standard -r
    ```


## Specify an object ACL when you upload the object

You can use the --meta option to set ACL for an object when you upload the object. Valid values:

-   default: ACL inherited from the bucket
-   private
-   public-read
-   public-read-write

Commands:

-   Set ACL to private for an object when you upload the object

    ```
    ./ossutil cp examplefile.txt oss://examplebucket/desfolder/ --meta x-oss-object-acl:private
    ```

-   Set ACL to public read for an object when you upload the object

    ```
    ./ossutil cp localfolder/ oss://examplebucket/desfolder/ --meta x-oss-object-acl:public-read  -r
    ```


## Specify an encryption method for an object when you upload the object

You can specify a server-side encryption method when you upload an object and store the encrypted object in a bucket. For more information about server-side encryption, see [Server-side encryption](/intl.en-US/Developer Guide/Data security/Data encryption/Server-side encryption.md). Commands:

-   Set the server-side encryption method to OSS-managed and encryption algorithm to AES-256 for an object when you upload the object

    ```
    ./ossutil cp examplefile.txt oss://examplebucket/desfolder/ --meta=x-oss-server-side-encryption:AES256
    ```

-   Set the server-side encryption method to Key Management Service \(KMS\)without specifying a customer master key \(CMK\) ID for an object when you upload the object

    ```
    ./ossutil cp examplefile.txt oss://examplebucket/desfolder/ --meta=x-oss-server-side-encryption:KMS
    ```

    When KMS is used for encryption, a small amount of KMS key usage fees are incurred. For more information, see [KMS billing standards](/intl.en-US/Pricing/Billing.md).

-   Set the server-side encryption method to KMS and specify a CMK ID for an object when you upload the object

    ```
    ./ossutil cp examplefile.txt oss://examplebucket/desfolder/ --meta=x-oss-server-side-encryption:KMS#x-oss-server-side-encryption-key-id:7bd6e2fe-cd0e-483e-acb0-f4b9e1******
    ```


## Generate snapshots for an object when you upload the object

If you specify the --snapshot-path option when you upload multiple objects at a time, ossutil generates snapshots for the objects in the specified folder. This folder records the last modified time on the local computer for uploaded and download objects. When you upload or download these objects next time, the last modified time of these objects is used to determine whether to skip same objects. When you use this option, make sure that other users do not modify the corresponding objects in OSS during the upload and download processes. The --snapshot-path option is applicable to scenarios where the simultaneous incremental upload or download of objects is accelerated. This option is not applicable when you copy objects. Command:

```
./ossutil cp -r localfolder/ oss://examplebucket/desfolder/ --snapshot-path=path                                
```

**Note:**

-   ossutil does not proactively delete snapshot information in the folder specified by snapshot-path. To avoid consuming excessive storage, delete snapshots that you no longer use from the folder specified by snapshot-path on a regular basis.
-   Additional overheads are generated when you read and write snapshot information. We recommend that you do not use this option in the following scenarios: The number of objects to upload or download is small. The network is properly connected. Other users must perform operations on those objects. You can use the --update option to perform incremental upload or download.
-   The --update and --snapshot-path options can be used together. ossutil first uses the --snapshot-path option to determine whether to skip the upload or download of an object. If the upload or download is not skipped by --snapshot-path, ossutil uses the --update option to determine whether to skip the upload or download of the object.

## Upload multiple objects that meet specified conditions

If you specify the --include and --exclude options when you upload multiple objects at a time, ossutil uploads objects that meet the specified conditions at a time.

The --include and --exclude options support the following formats:

-   Wildcard asterisk \(\*\): matches all characters. For example, \*.txt specifies TXT objects.
-   Wildcard question mark \(?\): matches a single character. For example, abc?.jpg specifies all JPG objects whose names begin with abc and are followed by a single character such as abc1.jpg.
-   \[sequence\]: matches characters in a sequence. For example, abc\[1-5\].jpg specifies the objects whose names begin with abc and are followed by a number contained in sequence \[1-5\]. The objects include abc1.jpg, abc2.jpg, abc3.jpg, abc4.jpg, and abc5.jpg.
-   \[! sequence\]: matches characters that are not in a sequence. For example, abc\[! 0-7\].jpg indicates that objects abc0.jpg, abc1.jpg, abc2.jpg, abc3.jpg, abc4.jpg, and abc5.jpg, abc6.jpg, and abc7.jpg are not matched.

One rule can contain multiple conditions specified by include and exclude. If these conditions are set, ossutil reads each rule from left to right to obtain the final matching results. If the test.txt object exists in a valid folder, results are generated based on different matching rules.

-   Rule 1: `--include "*test*" --exclude "*.txt"`. When ossutil reads the `--include "*test*"` condition, the test.txt object matches the condition. When ossutil reads the `--exclude "*.txt"` condition, the test.txt object is excluded because the object name is in the TXT format. The final matching results exclude the test.txt object.
-   Rule 2: `--exclude "*.txt" --include "*test*"`. When ossutil reads the `--exclude "*.txt"` condition, the test.txt object is excluded. When ossutil reads the `--include "*test*"` condition, the test.txt object matches the condition because its name contains test. The final matching results include the test.txt object.
-   Rule 3: `--include "*test*" --exclude "*.txt" --include "te? t.txt"`. When ossutil reads the `--include "*test*"` condition, the test.txt object matches the condition. When ossutil reads the `--exclude "*.txt"`condition, the test.txt object is excluded because the object name is in the TXT format. When ossutil reads the `--include "te? t.txt"` condition, the test.txt object matches the condition. The final matching results include the test.txt object.

**Note:** Names that include folder names such as `--include "/usr/test/.jpg"` are not supported when you set the condition.

Commands:

-   Upload all objects that are in the TXT format

    ```
    ./ossutil cp localfolder/ oss://examplebucket/desfolder/ --include "*.txt" -r
    ```

-   Upload all objects that contain abc in their names and are not in the JPG or TXT format

    ```
    ./ossutil cp localfolder/ oss://examplebucket/desfolder/ --include "*abc*" --exclude "*.jpg" --exclude "*.txt" -r
    ```


