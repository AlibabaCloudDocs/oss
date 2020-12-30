# ossutil

This topic describes how to use ossutil to create a bucket, upload a local file to the bucket, download the uploaded object, and generate a signed URL for the uploaded object to share the object with third parties for download or preview.

## Prerequisites

ossutil is installed. For more information, see [Download and installation](/intl.en-US/Tools/ossutil/Download and installation.md).

## Create a bucket

-   Command syntax

    ```
    ./ossutil mb oss://bucket
    ```

-   Examples

    Create a bucket named examplebucket.

    ```
    ./ossutil mb oss://examplebucket
    ```

    If the following results are displayed, the bucket named examplebucket is created.

    ```
    0.668238(s) elapsed
    ```


For more examples of creating buckets, see [mb](/intl.en-US/Tools/ossutil/Common commands/mb.md).

## Upload an object

-   Command syntax

    ```
    ./ossutil cp local\_file oss://bucket
    ```

-   Examples

    -   Upload a file named examplefile.txt to the examplebucket bucket.

        ```
        ./ossutil cp examplefile.txt oss://examplebucket
        ```

    -   Upload a file named examplefile.txt to the examplebucket bucket and rename the file as exampleobject.txt.

        ```
        ./ossutil cp examplefile.txt oss://examplebucket/exampleobject.txt
        ```

    If the following results are displayed, the file is uploaded to the bucket.

    ```
    0.720812(s) elapsed
    ```


For more examples of uploading objects, see [Upload objects](/intl.en-US/Tools/ossutil/Common commands/cp/Upload objects.md).

## Download an object

-   Command syntax

    ```
    ./ossutil cp cloud\_url local\_file
    ```

-   Examples

    Download the object examplefile.txt from the examplebucket bucket to the local folder named localfolder.

    ```
    ./ossutil cp oss://examplebucket/examplefile.txt localfolder/
    ```

    Download the object examplefile.txt from the examplebucket bucket to the local folder named localfolder and rename the object as exampleobject.txt.

    ```
    ./ossutil cp oss://examplebucket/examplefile.txt localfolder/exampleobject.txt
    ```

    If the following results are displayed, the object is downloaded to the local folder.

    ```
    0.720812(s) elapsed
    ```


For more examples of downloading objects, see [Download objects](/intl.en-US/Tools/ossutil/Common commands/cp/Download objects.md).

## Generate a signed URL for an object

-   Command syntax

    ```
    ./ossutil sign cloud\_url --timeout t
    ```

-   Examples

    Generate a signed URL with a validity period of 3,600 seconds for the object `oss://examplebucket/exampleobject.txt`.

    ```
    ./ossutil sign oss://examplebucket/exampleobject.txt --timeout 3600 
    ```

    If the following results are displayed, the signed URL is generated.

    ```
    https://examplebucket.ss-cn-hangzhou.aliyuncs.com/exampleobject.txt?Expires=1608282224&OSSAccessKeyId=LTAI4G33piUmgRN1DXx9****&Signature=jo4%2FGykfuc1A4fvyvKRpRyymYH****
    ```


For more examples of generating signed URLs, see [sign](/intl.en-US/Tools/ossutil/Common commands/sign.md).

