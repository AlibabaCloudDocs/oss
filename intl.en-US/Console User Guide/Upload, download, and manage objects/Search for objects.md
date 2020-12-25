# Search for objects

If a large number of objects are stored in your bucket, you can search for objects by specifying a prefix.

## Usage notes

-   Search rule

    You can search for objects by prefix. The string used to search for objects is case-sensitive and cannot contain forward slashes \(/\).

-   Search results

    When you specify a prefix to search for objects in the root folder or a specified folder of a bucket, only the objects or subfolders whose names contain the specified prefix are returned. For more information, see [List objects](/intl.en-US/Developer Guide/Objects/Manage files/View the object list.md).


## Procedures

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket to which you want to upload objects.

3.  In the left-side navigation pane, click **Files**.

4.  Search for objects.

    -   Search for specific objects or folders within the root folder of the bucket

        Enter a prefix in the search box in the upper-right corner. Press the Enter key or click the ![search](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1967549951/p59253.png) icon. Objects and folders whose names contain the specified prefix within the root folder are displayed.

        The following example shows the search result when you specify Example as the prefix to search for objects and folders within the root folder of the bucket named TestBucket.

        ```
        Folder structure             Specified prefix          Search result
        TestBucket                         Example               Examplesrcfolder1                          
            └── Examplesrcfolder1                                Exampledestfolder.png                           
                   ├── test.txt                                
                   ├── abc.jpg
            └── Exampledestfolder.png
            └── example.txt
        ```

    -   Search for specific objects or subfolders within a folder of the bucket

        Click the folder that includes the objects or subfolders you want to search. Enter a prefix in the search box in the upper-right corner. Press the Enter key or click the ![search](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1967549951/p59253.png) icon. Objects and subfolders whose names contain the specified prefix within the folder are displayed.

        The following example shows the search result when you specify Project as the prefix to search for objects and subfolders within the folder named Examplesrcfolder1.

        ```
        Folder structure             Specified prefix          Search result
        Examplesrcfolder1              Project                 Projectfolder
            └── Projectfolder                                  ProjectA.jpg
                  ├── a.txt                                    ProjectB.doc
                  ├── b.txt
            └── ProjectA.jpg
            └── ProjectB.doc
            └── projectC.doc
        ```


