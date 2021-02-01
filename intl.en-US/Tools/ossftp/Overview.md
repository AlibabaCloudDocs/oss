# Overview

ossftp is an FTP server tool based on Alibaba Cloud OSS. ossftp maps operations related to files and folders to those on OSS objects and folders. This way, you can manage objects stored in OSS over FTP.

## Limits

-   ossftp is provided for individual tests. To manage your OSS, we recommend that you use tools such as the [OSS console](/intl.en-US/Console User Guide/Log on to the OSS console/Use Alibaba Cloud accounts to log on to the OSS console.md), [ossutil](/intl.en-US/Tools/ossutil/Overview.md), [ossbrowser](/intl.en-US/Tools/ossbrowser/Quick start.md), and [SDK](/intl.en-US/SDK Reference/Introduction.md) in production environments.
-   The FTP protocol transmits data in plaintext. To prevent password leaks, we recommend that you run ossftp and the client on the same machine and access data by using `127.0.0.1:port`.

## Runtime environment

-   Supported operating systems: Windows, Linux, and macOS
-   Supported architectures: x86 \(32-bit and 64-bit\)
-   Runtime environment: Python2.6 and 2.7

## Features

-   Upload, download, and delete objects and folders.
-   Use multipart upload to upload large objects.
-   Support most FTP commands.

