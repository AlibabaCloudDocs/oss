# How do I upload and download folders to and from OSS?

OSS does not use a hierarchical structure for objects, but instead uses a flat structure. All elements are stored as objects in buckets. Therefore, OSS does not have folders and subfolders like a hierarchical file system would. However, OSS supports folders as a concept to group objects and simplify management. In the OSS console, a folder is an object whose name ends with a forward slash \(/\), which is similar to folders in Windows.

For example, a file whose path is abc/efg/123.jpg is the 123.jpg object contained in the efg subfolder of the abc folder in the OSS console.

You can use the following methods to upload or download a folder:

-   [OSS console](/intl.en-US/Console User Guide/Log on to the OSS console/Overview of the OSS console.md): a graphical management tool similar to the Windows resource manager.
    -   Upload a folder: When you upload a folder, drag the folder to the upload section. The folder structure remains. For more information, see [Upload an object](/intl.en-US/Console User Guide/Upload, download, and manage objects/Upload an object.md).
    -   Download a folder: OSS console does not support the direct download of folders. You can download multiple objects from a bucket to the specified folder that is created on the local computer. For more information, see [Download objects](/intl.en-US/Console User Guide/Upload, download, and manage objects/Download objects.md).
-   [ossbrowser](/intl.en-US/Tools/ossbrowser/Quick start.md): a graphical management tool, which is easy to use. This tool is similar to the Windows resource manager.
    -   Upload a folder: In the specified bucket or folder, click **Folder**. Select the folder you want to upload. You can also drag the folder to ossbrowser. For more information, see [Upload objects](/intl.en-US/Tools/ossbrowser/Quick start.md).
    -   Download a folder: Click Download in the Actions column corresponding to the folder. For more information, see [Download objects](/intl.en-US/Tools/ossbrowser/Quick start.md).
-   [ossutil](/intl.en-US/Tools/ossutil/Overview.md): a command line management tool that provides a wide range of simple and convenient commands to manage OSS data while high performance of operations is ensured.
    -   Upload a folder: Include the -r option when you upload a folder. For more information, see [Upload objects](/intl.en-US/Tools/ossutil/Common commands/cp/Overview.md).
    -   Download a folder: Include the -r option when you download a folder. For more information, see [Download objects](/intl.en-US/Tools/ossutil/Common commands/cp/Overview.md).
-   [SDK](/intl.en-US/SDK Reference/Introduction.md): provides OSS SDK demos in various programming languages to facilitate development.
    -   Upload a folder: OSS SDKs do not support the direct upload of folders. However, you can set the same prefix for object names and separate each folder level with a forward slash \(/\) when you upload the objects. For example, when you upload the a.txt, b.txt, and c.txt objects to the abc folder, set the ObjectName parameter to abc/a.txt, abc/b.txt, and abc/c.txt.
    -   Download a folder: OSS SDKs do not support the direct download of folders. However, you can download multiple objects to the same local folder.

