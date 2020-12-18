# 命令行工具ossutil快速入门

本文旨在引导您通过命令行工具ossutil快速创建目标存储空间（Bucket），然后将本地文件上传至Bucket。上传完成后，将文件（Object）下载至本地或者通过生成签名URL的方式将文件分享给第三方，供其下载或预览。

## 前提条件

已安装ossutil。详情请参见[下载和安装](/cn.zh-CN/常用工具/命令行工具ossutil/下载和安装.md)。

## 创建Bucket

-   命令格式

    ```
    ./ossutil mb oss://bucket
    ```

-   使用示例

    创建名为examplebucket的存储空间。

    ```
    ./ossutil mb oss://examplebucket
    ```

    以下输出结果表明已成功创建examplebucket。

    ```
    0.668238(s) elapsed
    ```


有关创建Bucket的更多示例，请参见[mb](/cn.zh-CN/常用工具/命令行工具ossutil/常用命令/mb.md)。

## 上传文件

-   命令格式

    ```
    ./ossutil cp local\_file oss://bucket
    ```

-   使用示例

    -   上传单个文件examplefile.txt至目标存储空间examplebucket。

        ```
        ./ossutil cp examplefile.txt oss://examplebucket
        ```

    -   上传单个文件examplefile.txt至目标存储空间examplebucket，并将文件重命名为exampleobject.txt。

        ```
        ./ossutil cp examplefile.txt oss://examplebucket/exampleobject.txt
        ```

    以下输出结果表明文件已成功上传至目标Bucket。

    ```
    0.720812(s) elapsed
    ```


有关上传文件的更多示例，请参见[cp（上传文件）](/cn.zh-CN/常用工具/命令行工具ossutil/常用命令/cp/上传文件.md)。

## 下载文件

-   命令格式

    ```
    ./ossutil cp cloud\_url local\_file
    ```

-   使用示例

    将文件examplefile.txt从目标存储空间examplebucket下载至本地localfolder文件夹下。

    ```
    ./ossutil cp oss://examplebucket/examplefile.txt localfolder/
    ```

    将文件examplefile.txt从目标存储空间examplebucket下载至本地localfolder文件夹下，并将文件重命名为exampleobject.txt。

    ```
    ./ossutil cp oss://examplebucket/examplefile.txt localfolder/exampleobject.txt
    ```

    以下输出结果表明文件已成功下载至本地目标文件夹。

    ```
    0.720812(s) elapsed
    ```


有关下载文件的更多示例，请参见[cp（下载文件）](/cn.zh-CN/常用工具/命令行工具ossutil/常用命令/cp/下载文件.md)。

## 生成签名URL

-   命令格式

    ```
    ./ossutil sign cloud\_url --timeout t
    ```

-   使用示例

    对目标文件`oss://examplebucket/exampleobject.txt`生成超时时间为3600秒的文件签名URL。

    ```
    ./ossutil sign oss://examplebucket/exampleobject.txt --timeout 3600 
    ```

    以下输出结果表明已成功生成文件签名URL。

    ```
    https://examplebucket.ss-cn-hangzhou.aliyuncs.com/exampleobject.txt?Expires=1608282224&OSSAccessKeyId=LTAI4G33piUmgRN1DXx9****&Signature=jo4%2FGykfuc1A4fvyvKRpRyymYH****
    0.368676(s) elapsed
    ```


有关生成签名URL的更多示例，请参见[sign](/cn.zh-CN/常用工具/命令行工具ossutil/常用命令/sign.md)。

