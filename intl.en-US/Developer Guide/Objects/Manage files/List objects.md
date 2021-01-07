# List objects

You can call the GetBucket \(ListObjects\) or GetBucketV2 \(ListObjectsV2\) operation to list objects in a bucket.

**Note:** The GetBucket \(ListObjects\) operation has been updated to GetBucketV2 \(ListObjectsV2\). We recommend that you use GetBucketV2 \(ListObjectsV2\) when you develop your applications. To provide backward compatibility, OSS continues to support the GetBucket \(ListObjects\) operation. For more information about GetBucketV2 \(ListObjectsV2\), see [GetBucketV2 \(ListObjectsV2\)](/intl.en-US/API Reference/Bucket operations/Basic operations/GetBucketV2 (ListObjectsV2).md).

## Implementation modes

|Implementation mode|Description|
|-------------------|-----------|
|[Console](https://oss.console.aliyun.com/overview)|A web application that lists the objects in a bucket on the Files page of the bucket|
|[ossbrowser](/intl.en-US/Tools/ossbrowser/Quick start.md)|An easy-to-operate graphical tool|
|[ossutil](/intl.en-US/Tools/ossutil/Common commands/ls.md)|A high-performance command-line tool|
|[Java SDK](/intl.en-US/SDK Reference/Java/Manage objects/List objects.md)|SDK demos for various programming languages|
|[Python SDK](/intl.en-US/SDK Reference/Python/Manage objects/List objects.md)|
|[PHP SDK](/intl.en-US/SDK Reference/PHP/Manage objects/List objects.md)|
|[Go SDK](/intl.en-US/SDK Reference/Go/Manage objects/List objects.md)|
|[C SDK](/intl.en-US/SDK Reference/C/Manage objects/List objects.md)|
|[.NET SDK](/intl.en-US/SDK Reference/. NET/Manage objects/List objects.md)|
| |
|[Node.js SDK](/intl.en-US/SDK Reference/Node. js/Manage objects/List all objects in a bucket.md)|
|[Browser.js SDK](/intl.en-US/SDK Reference/Browser.js/Manage objects.md)|
|[Ruby SDK](/intl.en-US/SDK Reference/Ruby/Manage objects.md)|

## Usage notes

You can call the GetBucketV2 \(ListObjectsV2\) or GetBucket \(ListObjects\) operation to list up to 1,000 objects in a bucket at a time. The following tables describe the parameters that you can use to list objects in various ways.

-   GetBucketV2 \(ListObjectsV2\)

    |Parameter|Description|
    |---------|-----------|
    |Delimiter|The character used to group objects by name. If you specify the Delimiter parameter in the request, the response contains the CommonPrefixes element. Objects whose names contain the same string from the prefix and the next occurrence of the delimiter are grouped as a single result element in CommonPrefixes.|
    |Start-after|Specifies the Start-after value from which to start the list. The names of objects are returned in alphabetical order. The Start-after parameter is used to list the returned objects by page. This parameter value must be smaller than 1,024 bytes in length.

Even if the list excludes the object name specified by Star-after during a conditional query, the names of objects from which to start the list are returned in alphabetical order. |
    |Continuation-token|Specifies the token from which to start the list. If the number of objects exceeds the Max-keys value, the response includes `<NextContinuationToken>` as the condition token for next listing.|
    |Max-keys|The maximum number of objects to return. Valid values: 0 to 1000.

Default value: 100. |
    |Prefix|The prefix that the returned object names must contain. If Prefix is set to a folder name in the request, the objects whose names contain this prefix are listed, including all objects and subfolders in the folder.

If you specify Prefix and set Delimiter to a forward slash \(/\), only the objects in the folder are listed. The subfolders are grouped together as a single result in CommonPrefixes. Objects and folders in the subfolders are not listed.

For example, a bucket contains the following objects: fun/test.jpg, fun/movie/001.avi, and fun/movie/007.avi. If Prefix is set to fun/, the three objects are returned. If Prefix is set to fun/ and Delimiter is set to a forward slash \(/\), fun/test.jpg and fun/movie/are returned.

**Note:**

    -   The parameter value must be smaller than 1,024 bytes in length.
    -   If you specify a prefix to query objects, the names of the returned objects still contain the prefix. |
    |Fetch-owner|Specifies whether to include the owner information in the response. Valid values: true and false.

    -   true: The response includes the owner information.
    -   false: The response excludes the owner information. |

-   GetBucket \(ListObjects\)

    |Parameter|Description|
    |---------|-----------|
    |Delimiter|The character used to group objects by name. If you specify the Delimiter parameter in the request, the response contains the CommonPrefixes element. Objects whose names contain the same string from the prefix and the next occurrence of the delimiter are grouped as a single result element in CommonPrefixes.|
    |Marker|The name of the object after which the list begins. If this parameter is specified, objects whose names are alphabetically greater than the Marker parameter value are returned. The Marker parameter is used to list the returned objects by page, and its value must be smaller than 1,024 bytes in length.

Even if the specified marker does not exist in the list during a conditional query, the list starts from the object whose name is alphabetically greater than the Marker parameter value. |
    |Max-keys|The maximum number of objects to return. Valid values: 0 to 1000.

Default value: 100. |
    |Prefix|The prefix that the returned object names must contain. If Prefix is set to a folder name in the request, the objects whose names contain this prefix are listed, including all objects and subfolders in the folder.

If you specify Prefix and set Delimiter to a forward slash \(/\), only the objects in the folder are listed. The subfolders are grouped together as a single result in CommonPrefixes. Objects and folders in the subfolders are not listed.

**Note:**

    -   The parameter value must be smaller than 1,024 bytes in length.
    -   If you specify a prefix to query objects, the names of the returned objects still contain the prefix. |


## Folder simulation

OSS does not use a hierarchical structure for objects, but instead uses a flat structure. All elements are stored as objects in buckets. However, OSS supports folders as a concept to group objects and simplify management. A folder is an object whose name ends with a forward slash \(/\) and whose size is 0. This object can be uploaded and downloaded. The console shows any object whose name ends with a forward slash \(/\) as a folder.

You can use the Delimiter and Prefix parameters together to simulate folders in following ways:

-   If Prefix is set to a folder name in the request, the objects whose names contain this prefix are listed, including all objects and subfolders in the folder. The names are listed in the Contents element.
-   If you also set the Delimiter parameter to a forward slash \(/\) in the request, the objects whose names start with the specified prefix and subfolders \(directories\) in the folder are listed. The subfolders \(directories\) are listed in the CommonPrefixes element, excluding recursive objects and folders in these subfolders.

Example:

The oss-sample bucket contains the following objects:

```
Object D
Directory A/Object C
Directory A/Object D
Directory A/Directory B/Object B
Directory A/Directory B/Directory C/Object A
Directory A/Directory C/Object A
Directory A/Directory D/Object B
Directory B/Object A
```

-   List level-1 directories and objects

    Leave the Prefix parameter empty and set the Delimiter parameter to a forward slash \(/\). The following result is returned:

    ```
    <? xml version="1.0" encoding="UTF-8"? >
    <ListBucketResult>
      <Name>oss-sample</Name>
      <Prefix></Prefix>
      <Marker></Marker>
      <MaxKeys>1000</MaxKeys>
      <Delimiter>/</Delimiter>
      <IsTruncated>false</IsTruncated>
      <Contents>
        <Key>Object D</Key>
        <LastModified>2015-11-06T10:07:11.000Z</LastModified>
        <ETag>"8110930DA5E04B1ED5D84D6CC4DC9080"</ETag>
        <Type>Normal</Type>
        <Size>3340</Size>
        <StorageClass>Standard</StorageClass>
        <Owner>
          <ID>oss</ID>
          <DisplayName>oss</DisplayName>
        </Owner>
      </Contents>
      <CommonPrefixes>
        <Prefix>Directory A/</Prefix>
      </CommonPrefixes>
      <CommonPrefixes>
        <Prefix>Directory B/</Prefix>
      </CommonPrefixes>
    </ListBucketResult>
    ```

    The Contents element contains the level-1 object: Object D. The CommonPrefixes element contains the level-1 directories: Directory A/ and Directory B/, but does not list the objects in these directories.

-   List level-2 directories and objects in Directory A

    Set the Prefix parameter to Directory A and the Delimiter parameter to a forward slash \(/\). The following result is returned:

    ```
    <? xml version="1.0" encoding="UTF-8"? >
    <ListBucketResult>
      <Name>oss-sample</Name>
      <Prefix>Directory A/</Prefix>
      <Marker></Marker>
      <MaxKeys>1000</MaxKeys>
      <Delimiter>/</Delimiter>
      <IsTruncated>false</IsTruncated>
      <Contents>
        <Key>Directory A/Object C</Key>
        <LastModified>2015-11-06T09:36:00.000Z</LastModified>
        <ETag>"B026324C6904B2A9CB4B88D6D61C81D1"</ETag>
        <Type>Normal</Type>
        <Size>2</Size>
        <StorageClass>Standard</StorageClass>
        <Owner>
          <ID>oss</ID>
          <DisplayName>oss</DisplayName>
        </Owner>
      </Contents>
      <Contents>
        <Key>Directory A/Object D</Key>
        <LastModified>2015-11-06T09:36:00.000Z</LastModified>
        <ETag>"B026324C6904B2A9CB4B88D6D61C81D1"</ETag>
        <Type>Normal</Type>
        <Size>2</Size>
        <StorageClass>Standard</StorageClass>
        <Owner>
          <ID>oss</ID>
          <DisplayName>oss</DisplayName>
        </Owner>
      </Contents>
      <CommonPrefixes>
        <Prefix>Directory A/Directory B/</Prefix>
      </CommonPrefixes>
      <CommonPrefixes>
        <Prefix>Directory A/Directory C/</Prefix>
      </CommonPrefixes>
      <CommonPrefixes>
        <Prefix>Directory A/Directory D/</Prefix>
      </CommonPrefixes>
    </ListBucketResult>
    ```

    The Contents element contains the level-2 objects: Directory A/Object C and Directory A/Object D. The CommonPrefixes element contains the level-2 directories: Directory A/Directory B/, Directory A/Directory C/, and Directory A/Directory D/, but does not list the objects in these directories.


