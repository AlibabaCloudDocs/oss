# Object naming conventions

Object Storage Service \(OSS\) uses a flat structure instead of a hierarchical structure used by traditional file systems to store objects. All elements in OSS are stored as objects in buckets. Objects are the basic unit for data operations in OSS. Objects are also known as files. OSS uses a key \(name\) to uniquely identify an object.

## Naming conventions

The name of an object must comply with the following conventions:

-   The name can contain only UTF-8 characters.
-   The name must be 1 to 1,023 bytes in length.
-   The name cannot start with a forward slash \(/\) or a backslash \(\\\).

## Examples

The key of an object varies with the location in which the object is stored. The following table describes the keys of two objects that are stored in different locations of a bucket.

|Object|Key|
|------|---|
|An object that is named exampleobject.txt and is stored in the root directory of a bucket named examplebucket|exampleobject.txt|
|An object that is named exampleobject.jpg and is stored in the destdir directory within the root directory of a bucket named examplebucket|destdir/exampleobject.jpg|

