# Upload objects

The cp command is used to upload local files or directories to Object Storage Service \(OSS\).

## Usage notes

-   Sample command lines in this topic are based on the 64-bit Linux system. For other systems, replace ./ossutil64 in the commands with the corresponding binary name. For more information, see [ossutil](/intl.en-US/Quick Start/ossutil.md).
-   By default, when you use the cp command to upload objects, multipart upload and resumable upload are used. If the upload is interrupted before the object is completely uploaded, the uploaded portion is stored as parts in an OSS bucket. To avoid additional storage fees, we recommend that you use the following methods to delete these parts if you no longer need them.
    -   Manually delete parts. For more information, see [Delete parts](/intl.en-US/Tools/ossutil/Common commands/rm (delete objects, parts, or buckets).md).
    -   Configure lifecycle rules to automatically delete parts. For more information, see [Configure lifecycle rules](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure lifecycle rules.md).

For more information about the format of the cp command and the parameters that is suppported, see [Overview](/intl.en-US/Tools/ossutil/Common commands/cp/Overview.md).

## Sample environment

In this topic, local files or directories are uploaded from a Linux system to OSS. You can modify the parameters in the examples based on your operating system and environment. This topic involves the following common examples:

-   Local file: examplefile.txt \(a file in the root directory\)
-   Local directory: localfolder \(a directory in the root directory\)
-   OSS bucket: examplebucket
-   Specified directory in the OSS bucket: desfolder

## Simple upload

You can use ossutil to upload local files to OSS.

-   Upload a single local file

    If you do not specify the name of the uploaded object, the name of the local file is used as the object name.

    ```
    ./ossutil cp examplefile.txt oss://examplebucket/desfolder/
    ```

-   Upload the files in a local directory

    You can add the -r parameter to the cp command to upload files in the specified local directory to OSS.

    ```
    ./ossutil cp -r localfolder/ oss://examplebucket/desfolder/
    ```

-   Upload a local directory and the files inside

    You can add the -r parameter to the cp command to upload a local directory and the files inside to OSS.

    ```
    ./ossutil cp -r localfolder/ oss://examplebucket/desfolder/localfolder/
    ```

-   Upload a single file and specify the --meta parameter.

    When you run the cp command to upload a local file, you can add the --meta parameter to the command and configure the metadata of the file in the following format: `header:value#header:value...`.

    ```
    ./ossutil cp examplefile.txt oss://examplebucket/desfolder/examplefile.txt --meta=Cache-Control:no-cache#Content-Encoding:gzip
    ```

-   Upload a directory without uploading existing objects

    When you resume a failed batch upload task, you can specify the --update \(abbreviated as -u\) parameter to skip uploaded files. This way, you can upload only incremental data to OSS.

    ```
    ./ossutil cp -r localfolder/ oss://examplebucket/desfolder/ -u
    ```

-   Upload files to a bucket for which pay-by-requester is enabled

    ```
    ./ossutil cp localfolder/examplefile.txt oss://examplebucket/ --payer=requester
    ```

-   Upload files only in the current directory

    ```
    ./ossutil cp localfolder/ oss://examplebucket/desfolder/ --only-current-dir -r
    ```

-   Upload a directory without generating an object for the uploaded directory

    In OSS, a directory is an object that is 0 KB in size and has a name that ends with a forward slash \(/\). If you specify the --disable-dir-object parameter in the cp command to upload a directory, OSS does not generate an object for the uploaded directory. However, you can view the directory in the OSS console. If you delete all objects in the directory, the directory is also deleted.

    ```
    ./ossutil cp localfolder/ oss://examplebucket/desfolder/ --disable-dir-object -r
    ```

-   Upload files including those in subdirectories to which symbolic links point

    ```
    ./ossutil cp localfolder/ oss://examplebucket/desfolder/ --enable-symlink-dir -r
    ```

-   Upload files and ignore files and subdirectories to which symbolic links point

    ```
    ./ossutil cp localfolder/ oss://examplebucket/desfolder/ -r --disable-all-symlink
    ```


## Set the maximum upload speed

When you run the cp command to upload a file, you can specify the --maxupspeed parameter to set the maximum upload speed in KB/s. The default value of the parameter is 0, which indicates that the upload speed is not limited. The following examples show how to set the maximum upload speed when you upload files and directories:

-   Upload a file to OSS and set the maximum upload speed to 1 MB/s.

    ```
    ./ossutil cp examplefile.txt oss://examplebucket/desfolder/ --maxupspeed 1024
    ```

-   Upload a directory to OSS and set the maximum upload speed to 1 MB/s.

    ```
    ./ossutil cp -r localfolder/ oss://examplebucket/desfolder/ --maxupspeed 1024
    ```


## Configure tags for files to upload

When you run the cp command to upload a file, you can specify the --tagging parameter to configure tags for the files to upload. Multiple tags are separated by ampersands \(&\).

```
./ossutil cp examplefile.txt oss://examplebucket/desfolder/ --tagging "abc=1&bcd=2&……"
```

For more information about object tagging, see [object-tagging \(add, modify, query, and delete object tags\)](/intl.en-US/Tools/ossutil/Common commands/object-tagging (add, modify, query, and delete object tags).md).

## Specify the storage classes of uploaded objects

When you run the cp command to upload a local file to an object, you can specify the --meta parameter to specify the storage class of the object. OSS supports the following storage classes:

-   Standard
-   Infrequent Access \(IA\)
-   Archive

If you do not specify the storage class in the command, the storage class of the uploaded object is the same as that of the bucket in which the object is stored. For more information, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md). The following examples show how to specify the storage class of uploaded objects:

-   Upload an object and set the storage class of the object to IA.

    ```
    ./ossutil cp examplefile.txt oss://examplebucket/desfolder/ --meta X-oss-Storage-Class:IA
    ```

-   Upload objects in a directory and set the storage class of the objects to Standard.

    ```
    ./ossutil cp localfolder/ oss://examplebucket/desfolder/ --meta X-oss-Storage-Class:Standard -r
    ```


## Specify the ACL of uploaded objects

When you run the cp command to upload objects, you can specify the --meta parameter to set the access control list \(ACL\) of the objects. OSS supports the following object ACLs:

-   default: The ACL of an object is the same as that of the bucket in which the object is stored.
-   private
-   public-read
-   public-read-write

The following examples show how to specify the ACL of uploaded objects:

-   Upload an object and set the ACL of the object to private.

    ```
    ./ossutil cp examplefile.txt oss://examplebucket/desfolder/ --meta x-oss-object-acl:private
    ```

-   Upload objects in a directory and set the ACL of the objects to public-read.

    ```
    ./ossutil cp localfolder/ oss://examplebucket/desfolder/ --meta x-oss-object-acl:public-read  -r
    ```


## Specify the encryption method of uploaded objects

When you run the cp command to upload a file to an object in a bucket, you can specify the method used to encrypt the object on the OSS server. For more information about server-side encryption, see [Server-side encryption](/intl.en-US/Developer Guide/Data security/Data encryption/Server-side encryption.md). The following examples show how to specify the encryption method of uploaded objects:

-   Upload an object and set the encryption method of the object to SSE-OSS and the encryption algorithm to AES256.

    ```
    ./ossutil cp examplefile.txt oss://examplebucket/desfolder/ --meta=x-oss-server-side-encryption:AES256
    ```

-   Upload an object and set the encryption method of the object to SSE-KMSwithout specifying a customer master key \(CMK\) ID.

    ```
    ./ossutil cp examplefile.txt oss://examplebucket/desfolder/ --meta=x-oss-server-side-encryption:KMS
    ```

    When you set the encryption method to SSE-KMS, you are charged for the usage of CMK IDs. For more information, see [KMS billing methods](/intl.en-US/Pricing/Billing.md).

-   Upload an object, set the encryption method of the object to SSE-KMS, and specify a CMK ID.

    ```
    ./ossutil cp examplefile.txt oss://examplebucket/desfolder/ --meta=x-oss-server-side-encryption:KMS#x-oss-server-side-encryption-key-id:7bd6e2fe-cd0e-483e-acb0-f4b9e1******
    ```


## Generate snapshots for uploaded objects

When you run the cp command to batch upload files and specify the --snapshot-path parameter in the command, ossutil creates the snapshots of the uploaded objects in the specified directory to record the last modified time of the objects. In further upload tasks, ossutil determines whether to skip existing objects based on their last modified time. If you want to specify the --snapshot-path parameter in the command, make sure that the objects in OSS are not modified by other users since the last time when the objects are uploaded. The --snapshot-path parameter is used to accelerate incremental batch uploads or downloads and is not supported when you run the cp command to copy objects. The following example shows how to upload files in a directory to OSS and generate snapshots for uploaded objects:

```
./ossutil cp -r localfolder/ oss://examplebucket/desfolder/ --snapshot-path=path                                
```

**Note:**

-   ossutil does not delete snapshots stored in the directory specified by the snapshot-path parameter. To prevent snapshots from consuming too much storage space, delete snapshots that you no longer need from the directory specified by the snapshot-path parameter.
-   Additional overheads are generated when you read and write snapshot information. We recommend that you do not specify this parameter in the following scenarios: 1. The number of objects to upload or download is small. 2. The network is properly connected. 3. Other users must perform operations on those objects. In these scenarios, you can specify the --update parameter in the command for incremental uploads or downloads.
-   You can specify both the --update and --snapshot-path parameters in a command. ossutil determines whether to skip a file in uploads or downloads first based on the snapshots stored in the directory specified by --snapshot-path. If no snapshots are generated for the file, ossutil determines whether to skip the file based on the --update parameter.

## Batch upload files that meet specified conditions

When you run the cp command to batch upload files and specify the --include and --exclude parameters in the command, ossutil batch uploads files that meet the specified conditions.

The --include and --exclude parameters support the following formats:

-   Asterisk \(\*\): matches all characters. For example, \*.txt indicates all TXT files.
-   Question mark \(?\): matches a single character. For example, abc?.jpg specifies all JPG objects whose names begin with abc and are followed by a single character such as abc1.jpg.
-   \[sequence\]: matches characters in a sequence. For example, abc\[1-5\].jpg specifies the objects whose names begin with abc and are followed by a number contained in sequence \[1-5\]. The objects include abc1.jpg, abc2.jpg, abc3.jpg, abc4.jpg, and abc5.jpg.
-   \[!sequence\]: matches characters that are not in a sequence. For example, abc\[!0-7\].jpg indicates that objects abc0.jpg, abc1.jpg, abc2.jpg, abc3.jpg, abc4.jpg, and abc5.jpg, abc6.jpg, and abc7.jpg are not matched.

A rule can contain multiple conditions specified by --include and --exclude. After these conditions are configured, ossutil reads each rule from left to right to obtain the final matching results. If the test.txt object exists in a valid directory, results are generated based on different matching rules.

-   Rule 1: `--include "*test*" --exclude "*.txt"`. When ossutil reads the `--include "*test*"` condition, the test.txt object matches the condition. When ossutil reads the `--exclude "*.txt"` condition, the test.txt object is excluded because the object name is in the TXT format. The final matching results exclude the test.txt object.
-   Rule 2: `--exclude "*.txt" --include "*test*"`. When ossutil reads the `--exclude "*.txt"` condition, the test.txt object is excluded. When ossutil reads the `--include "*test*"` condition, the test.txt object matches the condition because its name contains test. The final matching results include the test.txt object.
-   Rule 3: `--include "*test*" --exclude "*.txt" --include "te?t.txt"`. When ossutil reads the `--include "*test*"` condition, the test.txt object matches the condition. When ossutil reads the `--exclude "*.txt"`condition, the test.txt object is excluded because the object name is in the TXT format. When ossutil reads the `--include "te?t.txt"` condition, the test.txt object matches the condition. The final matching results include the test.txt object.

**Note:** Conditions that include directory names such as `--include "/usr/test/.jpg"` are not supported.

The following examples show how to upload files that meet the specified conditions:

-   Upload all files that are in the TXT format.

    ```
    ./ossutil cp localfolder/ oss://examplebucket/desfolder/ --include "*.txt" -r
    ```

-   Upload all files that contain abc in their names and are not in the JPG or TXT format.

    ```
    ./ossutil cp localfolder/ oss://examplebucket/desfolder/ --include "*abc*" --exclude "*.jpg" --exclude "*.txt" -r
    ```


