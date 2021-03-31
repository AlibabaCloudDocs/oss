# ossutil

This topic describes how to use ossutil to create a bucket, upload a local file to the bucket, download an object to the local computer, and generate a signed URL to share an object with third parties for downloads or previews.

## Prerequisites

ossutil is installed. For more information, see [Download and installation](/intl.en-US/Tools/ossutil/Download and installation.md).

## Usage notes

Each sample command line in this topic is based on the 64-bit Linux system. For other systems, replace ./ossutil64 of the command with the corresponding binary name. For example, for the 64-bit Windows system, replace ./ossutil64 with ossutil64.exe. The following table lists the binary names corresponding to each system.

|System|Binary name|
|------|-----------|
|64-bit Linux|./ossutil64|
|32-bit Linux|./ossutil32|
|64-bit Windows|ossutil64.exe|
|32-bit Windows|ossutil32.exe|
|64-bit macOS|./ossutilmac64|
|32-bit macOS|./ossutilmac32|
|64-bit ARM|./ossutilarm64|
|32-bit ARM|./ossutilarm32|

## Create a bucket

-   Command syntax

    ```
    ./ossutil64 mb oss://bucket
    ```

-   Examples

    Create a bucket named examplebucket.

    ```
    ./ossutil64 mb oss://examplebucket
    ```

    If a similar result is displayed, the bucket named examplebucket is created.

    ```
    0.668238(s) elapsed
    ```


For more information about how to create buckets, see [mb](/intl.en-US/Tools/ossutil/Common commands/mb.md).

## Upload an object

-   Command syntax

    ```
    ./ossutil64 cp local\_file oss://bucket
    ```

-   Examples

    -   Upload aan object named examplefile.txt to the examplebucket bucket.

        ```
        ./ossutil64 cp examplefile.txt oss://examplebucket
        ```

    -   Upload an object named examplefile.txt to the examplebucket bucket and rename the object-exampleobject.txt.

        ```
        ./ossutil64 cp examplefile.txt oss://examplebucket/exampleobject.txt
        ```

    If the following results are displayed, the object is uploaded to the bucket.

    ```
    0.720812(s) elapsed
    ```


For more information about how to upload objects, see [Upload objects](/intl.en-US/Tools/ossutil/Common commands/cp/Upload objects.md).

## Download an object

-   Command syntax

    ```
    ./ossutil64 cp cloud\_url local\_file
    ```

-   Examples

    Download the object examplefile.txt from the examplebucket bucket to the local folder named localfolder.

    ```
    ./ossutil64 cp oss://examplebucket/examplefile.txt localfolder/
    ```

    Download the object examplefile.txt from the examplebucket bucket to the local folder named localfolder and rename the object exampleobject.txt.

    ```
    ./ossutil64 cp oss://examplebucket/examplefile.txt localfolder/exampleobject.txt
    ```

    If the following results are displayed, the object is downloaded to the local folder.

    ```
    0.720812(s) elapsed
    ```


For more information about how to download objects, see [Download objects](/intl.en-US/Tools/ossutil/Common commands/cp/Download objects.md).

## Generate a signed URL for an object

-   Command syntax

    ```
    ./ossutil64 sign cloud\_url [--timeout <value>]
    ```

-   Examples

    Generate a signed URL where the validity period is set to 3,600 seconds for the object `oss://examplebucket/exampleobject.txt`.

    ```
    ./ossutil64 sign oss://examplebucket/exampleobject.txt --timeout 3600 
    ```

    If the following results are displayed, the signed URL is generated.

    ```
    https://examplebucket.ss-cn-hangzhou.aliyuncs.com/exampleobject.txt?Expires=1608282224&OSSAccessKeyId=LTAI4G33piUmgRN1DXx9****&Signature=jo4%2FGykfuc1A4fvyvKRpRyymYH****
    0.368676(s) elapsed
    ```


For more information about how to generate signed URLs, see [sign](/intl.en-US/Tools/ossutil/Common commands/sign.md).

