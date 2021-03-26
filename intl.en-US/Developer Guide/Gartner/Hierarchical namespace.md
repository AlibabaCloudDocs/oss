# Hierarchical namespace

The hierarchical namespace feature allows you to manage objects in a multi-level hierarchical structure based on directories. This feature allows you to perform a single atomic operation to rename a directory or an object. This way, you do not need to list or process all objects whose names contain a prefix. Traditionally, OSS uses a flat namespace to store objects in buckets and simulates directories by using objects whose names end with a forward slash \(/\). Compared with this method, the hierarchical namespace feature greatly improves the performance of operations that are related to directories.

## Benefits

-   Atomic operations on directories

    If a bucket uses a flat namespace in which real directories are not supported, applications may need to process millions of objects to complete a task at the directory level. In contrast, if a bucket uses the hierarchical namespace feature, applications can update a parent directory in a single atomic operation to complete multiple directory-level tasks at the same time.

-   Optimized performance

    Compared with a flat namespace, a hierarchical namespace eliminates the need to replicate or convert data before the data is analyzed. This improves the performance of directory management. The hierarchical namespace feature is especially important for big data analytics frameworks. For example, Hive or Spark usually writes output results to a temporary directory during a task and renames the directory after the task is completed. In a flat hierarchical namespace, the time that is consumed to rename the directory is usually longer than the time that is consumed to perform the task.


## Usage notes

When you perform directory-level operations, take note of the following items:

-   Create a directory

    A directory name cannot contain consecutive forward slashes \(/\).

    Data cannot be written to directories. The Content-Length of a directory can be set only to 0.

    The Content-Type of a directory can be set only to`application/x-directory`.

-   Rename a directory or object

    The name of a renamed directory or object cannot be the same as the name of an existing directory or object in the same bucket.

    The parent directory that is included in the name of a renamed directory or object must exist. For example, if you rename a directory `destfolder/examplefolder/test`, the parent directory `destfolder/examplefolder` must exist in the bucket.

-   Delete a directory
    -   Recursive delete

        Recursive delete is used to delete a directory and all the objects and subdirectories within the directory. To use recursive delete, you must have the DeleteObject permission on the directory and the objects and subdirectories within the directory that you want to delete.

        For example, if you want to use recursive delete to delete the dest/testfolder directory and the objects in the directory, you must have the DeleteObject permission on `dest/testfolder` and `dest/testfolder/*`.

        **Note:** If a concurrent request is sent to write data to a directory when you use recursive delete to delete the directory, the directory may fail to be deleted.

    -   Non-recursive delete

        Non-recursive delete is used to delete empty directories. To use non-recursive delete, you must have the DeleteObject permission on the directory that you want to delete.


## Unsupported features

The following table describes the features that are not supported by buckets for which the hierarchical namespace feature is enabled.

|Category|Feature|
|--------|-------|
|Bucket features|-   Cross-region replication \(CRR\)
-   Versioning
-   Bucket inventory
-   Cross-origin resource sharing \(CORS\)
-   Static website hosting
-   Lifecycle management
-   Retention policies |
|Object features|-   Symbolic links
-   Append upload
-   Access control list \(ACL\) and tagging
-   Image processing \(IMG\)
-   Archive and Cold Archive objects
-   Callback
-   The RestoreObject operation that is used to restore Archive and Cold Archive objects
-   The `x-oss-forbid-overwrite` parameter that is used to forbid a parameter from overwritting another parameter that has the same name
-   The `response-content-*` parameter in GetObject requests that are sent to query directories
-   Operations that are related to LiveChannel
-   The DeleteMultipleObjects operation that is used to delete objects in batches
-   The SelectObject operation that is used to select content from objects |

