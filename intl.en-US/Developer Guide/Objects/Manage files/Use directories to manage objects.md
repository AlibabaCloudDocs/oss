# Use directories to manage objects

Object Storage Service \(OSS\) uses a flat structure instead of a hierarchical structure used by traditional file systems to store objects. All elements in OSS are stored as objects in buckets. You can create simulated directories in OSS to help you categorize objects and control access to your objects in a simplified manner.

## Structure

OSS uses objects whose names end with a forward slash \(/\) to simulate directories. The following example shows the structure of a bucket named examplebucket:

```
examplebucket
    └── log/
       ├── date1.txt
       ├── date2.txt
       ├── date3.txt
    └── destfolder/
       └── 2021/
          ├── photo.jpg
```

In the preceding structure:

-   The following three objects have the log prefix in their names: log/date1.txt, log/date2.txt, and log/date3.txt. In the OSS console, a directory named log is displayed. Three objects named date1.txt, date2.txt, and date3.txt are stored within the directory.
-   The following object has the destfolder prefix in its name: destfolder/2021/photo.jpg. In the OSS console, a directory named destfolder is displayed, which contains a subdirectory named 2021. An object named photo.jpg is stored in the 2021 subdirectory.

## Access control based on directories

The following examples show how to grant third-party users different permissions to access the specified directories and objects in examplebucket described in the preceding section:

-   The following objects within the log directory store the OSS access logs of a user in the last three days: log/date1.txt, log/date2.txt, and log/date3.txt. Technical support needs to view the logs stored in the three objects to troubleshoot the issues, such as slow access and object upload failures, reported by the user. In this case, you can configure a bucket policy to grant the technical support permissions to access the log directory and objects within the directory. For more information about how to configure a bucket policy, see [Configure bucket policies to authorize other users to access OSS resources](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure bucket policies to authorize other users to access OSS resources.md).
-   An object named destfolder/2021/photo.jpg in examplebucket is a group photo of all your employees that was taken on a 2021 spring outing. You want all your employees to have access to the object. In this case, you can set the access control list \(ACL\) of the object to public read. For more information, see [Configure ACL for objects](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure ACL for objects.md).

## Implementation methods

You can create a directory by using only the OSS console, ossbrowser, and ossutil. After you create a directory, you can upload objects to the directory. You can delete directories that you no longer need by using the OSS console, ossbrowser, and ossutil.

|Operation|Implementation method|
|---------|---------------------|
|Create directories|[Console](/intl.en-US/Console User Guide/Upload, download, and manage objects/Create directories.md)|
|[ossutil](/intl.en-US/Tools/ossutil/Common commands/mkdir (create directories).md)|
|[ossbrowser](/intl.en-US/Tools/ossbrowser/Use ossbrowser.md)|
|Delete directories|[Console](/intl.en-US/Console User Guide/Upload, download, and manage objects/Delete directories.md)|
|[ossutil](/intl.en-US/Tools/ossutil/Common commands/rm (delete objects, parts, or buckets).md)|
|[ossbrowser](/intl.en-US/Tools/ossbrowser/Use ossbrowser.md)|

Directories cannot be created or deleted by calling API operations. However, you can use OSS SDKs for various programming languages to create or delete directories by using the following methods:

-   When you upload an object to OSS, you can add a directory name that ends with a forward slash \(/\) to the object name \(key\) to create a directory for the object. For example, when you upload a local file named localfile.txt to a bucket named examplebucket, you can set the name of the uploaded object to destfolder/localfile.txt. In this case, a directory named destfolder is created in examplebucket, and the uploaded object named localfile.txt is stored in destfolder. In this example, the destfolder directory is simulated by an object whose name is destfolder/ and whose size is 0. For more information about how to use OSS SDK for Java to create directories, see [Upload objects](/intl.en-US/SDK Reference/Java/Upload objects/Simple upload.md).
-   When you delete objects, you can specify a prefix that is contained in the names of all objects you want to delete. In this case, the directory whose name is the specified prefix and all objects within the directory are deleted. For example, if you specify a prefix "log", the directory named log and all objects within the directory are deleted. For more information about how to use OSS SDK for Java to delete directories, see [Delete objects](/intl.en-US/SDK Reference/Java/Manage objects/Delete objects.md).

