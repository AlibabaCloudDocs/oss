# Use ossftp

ossftp is a FTP server tool based on Alibaba Cloud OSS. ossftp receives FTP requests and maps operations related to files and folders to OSS objects and buckets. This way, you can manage objects stored in OSS over FTP.

-   ossftp is designed for individual users. We recommended that you use OSS SDKs for production environments.
-   Python 2.6 and Python 2.7 are supported by FTP servers.
-   ossftp V1.0 does not support Transport Layer Security \(TLS\) encryption for the ease of installation and deployment. FTP transmits data in plaintext. To prevent your password from disclosure, we recommend that you run the FTP server and client on the same server and access it by using 127.0.0.1:port.

## Benefits and features

-   Benefits
    -   Compatible with different platforms: You can run ossftp in Windows, Linux, or macOS. 32-bit and 64-bit operating systems, graphical interfaces, and command lines are supported.
    -   Free of installation: You can run this tool directly after decompression.
    -   Free of configuration: You can run this tool without further configurations.
    -   Open source: The FTP tool is written in Python. You can view the complete source code. The code will be available on GitHub.
-   Functions
    -   Allows you to upload, download, and delete objects and folders. Objects cannot be renamed or moved.
    -   Supports multipart upload of large objects.
    -   Supports most FTP commands to meet routine needs.

## Download URL

-   Windows: [ossftp-1.0.3-win.zip](http://gosspublic.alicdn.com/ossftp/ossftp-1.0.3-win.zip)

    By default, Python 2.7 is not installed on Windows. Therefore, the installation package includes Python 2.7. You can run ossftp directly after decompression.

-   Linux or macOS: [ossftp-1.0.3-linux-mac.zip](http://gosspublic.alicdn.com/ossftp/ossftp-1.0.3-linux-mac.zip)

    By default, Python 2.7 or Python 2.6 is installed on Linux or macOS. Therefore, the installation package contains only the required dependency libraries.


## Run ossftp

1.  Download the installation package based on your operating system.

2.  Decompress the installation package and run the code.

    **Note:** The path to which the installation package is decompressed cannot contain Chinese characters.

    -   In Windows:

        Decompress the installation package and double-click start.vbs. If no response is returned, upgrade your Internet Explorer or set another browser to the default browser.

    -   In Linux:
        1.  Decompress the installation package:

            ```
            $ unzip ossftp-1.0.3-linux-mac.zip
            ```

        2.  Open the decompressed folder and run start.sh.

            ```
            $ cd ossftp-1.0.3-linux-mac
            $ bash start.sh
            ```

        3.  Use a computer that supports graphical interfaces to access the FTP server operation interface by using a browser. Domain name: `http://ServerIP:8192`.
    -   In macOS:

        Decompress the installation package and double-click start.command. Alternatively, run `$ bash start.command` on the terminal.

3.  Configure the FTP Server, save the configurations, and then restart the server for the configurations to take effect.

    **Note:**

    -   The preceding process starts an FTP server, which listens to port 2048 on IP address 127.0.0.1 by default. A web server is also started to listen to port 8192 on IP address 127.0.0.1 for the convenience of monitoring the FTP server status.
    -   The management control page of the FTP server may fail to be opened in Internet Explorer of earlier versions. If you use Internet Explorer of an earlier version, upgrade the browser.
    Configure the following parameters for the FTP server:

    -   **ossftp address**: Set the IP address of the client that needs to use the FTP service. If the client is running on this server, you can keep the default settings.
    -   **ossftp port**: Set the listening port of ossftp. Keep the default settings.
    -   **ossftp log level**: Set the log level of ossftp. Set this parameter as needed.
    -   **Bucket endpoints**: Enter the domain name of the bucket in the bucket\_name.endpoint format. Separate multiple domain names with commas \(,\).
    -   **Language**: Set the display language for ossftp.
4.  Download and install [FileZilla Client](https://filezilla-project.org/?spm=a2c4g.11186623.2.6.bqHidZ).

5.  After the OSS access information is configured, you can click **Quickconnect**.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6224459951/p2520.png)

    **Note:** A server can have only a single connection at a time. When a new connection is created, the previous connection to the server is ended.

    -   **Host**: Set the server IP address. If the server and client are on the same device, use the default address 127.0.0.1.
    -   **Username**: Enter a bucket name and the AccessKey ID of the account that has permissions to access the bucket. The format is AccessKeyID/bucket\_name. Example: `tSxyi******wPMEp/test-hz-jh-002`. For more information about how to obtain an AccessKey ID, see [Create an AccessKey pair]().
    -   **Password**: Enter the AccessKey secret of the account that has permissions to access the bucket. For more information about how to obtain an AccessKey secret, see [Create an AccessKey pair]().
    -   **Port**: Enter the listening port configured on the server. The default value is 2048.

