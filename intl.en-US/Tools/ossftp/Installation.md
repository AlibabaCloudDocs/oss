# Installation

You can run ossftp on operating systems such as Windows, Linux, and macOS. This topic describes how to install and use ossftp.

## Download URL

You can download the installation package of ossftp based on your system environment:

-   Windows: [ossftp-1.1.0-win.zip](https://gosspublic.alicdn.com/ossftp/ossftp-1.1.0-win.zip)

    By default, Python 2.7 is not installed on Windows. The installation package includes Python 2.7. You can run ossftp directly after the package is decompressed.

-   Linux and macOS: [ossftp-1.1.0-linux-mac.zip](https://gosspublic.alicdn.com/ossftp/ossftp-1.1.0-linux-mac.zip)

    By default, Python 2.7 or Python 2.6 is installed on Linux or macOS. The installation package contains only the required dependent libraries.


## Procedure

1.  Decompress the downloaded installation package.

    The path to which the installation package is decompressed cannot contain Chinese characters.

2.  Run ossftp.

    After ossftp is run, TCP ports 2048 and 8192 of the local computer are enabled by default. Port 2048 is used as the port of the FTP service to receive FTP requests. Port 8192 is used as the port of the web service to open the graphical management interface of ossftp. To provide services for other users, enable the two ports in firewall configurations. The method to run ossftp varies with operating systems.

    -   In Windows:

        Decompress the installation package and double-click start.vbs. If no response is returned, upgrade the installed version of Internet Explorer, or set the default browser to a different browser.

    -   In Linux:
        1.  Run the following command to decompress the downloaded file:

            ```
            unzip ossftp-1.0.3-linux-mac.zip
            ```

        2.  Open the decompressed folder. Run start.sh. Commands:

            ```
            cd ossftp-1.0.3-linux-mac
            bash start.sh
            ```

        3.  Use a browser to access the graphical management interface of ossftp. The endpoint is `http://127.0.0.1:8192`.

            If the local machine does not provide a graphical management interface, you can use other computers to access the graphical management interface of ossftp. The endpoint is `http://Linux server IP address:8192`.

    -   In macOS:

        Decompress the installation package and double-click start.command. Alternatively, run $ bash start.command on the command line.

3.  The following table describes the parameters that you can configure on the graphical management interface of ossftp.

    |Parameter|Description|
    |---------|-----------|
    |**ossftp address**|Enter the IP address of the client that needs to use the FTP service.If the client is running on the local computer, you can keep the default settings. |
    |**ossftp port**|Set the port that receives requests for ossftp. If the default port does not conflict with other ports, keep the default port.|
    |**ossftp passive ports start**|Set the starting port number of the range to respond to requests for ossftp. If the default port does not conflict with other ports, keep the default port.|
    |**ossftp passive ports end**|Set the ending port number of the range to respond to requests for ossftp. If the default port does not conflict with other ports, keep the default port.|
    |**ossftp log level**|Set the log output level of ossftp. Valid values:    -   DEBUG: records information events in detail. DEBUG logs are used to debug programs.
    -   INFO: records events that occur when software properly runs.
    -   WARNING: records abnormal events that do not affect the system.
    -   ERROR: records abnormal events that affect the system but do not affect the system reliability.
    -   CRITICAL: records events that cause the system fail to work. |
    |**Bucket endpoints**|Enter the endpoint of the bucket. The format is `BucketName.Endpoint`. Separate multiple endpoints with commas \(,\).Example: `examplebucket.oss-cn-hangzhou.aliyuncs.com`. |
    |**Language**|Select a display language for ossftp.|

4.  After settings are complete, click **Save config**. Then, click **Restart**.

    Do not click **Exit**. Otherwise, ossftp stops running.

5.  Install the FTP client.

    The FileZilla software is used in the example. For more information about the address to download the software, visit [FileZilla](https://filezilla-project.org/?spm=a2c4g.11186623.2.6.bqHidZ).

6.  Open FileZilla. Configure the OSS access information. Then, click **Quickconnect**.

    |Parameter|Description|
    |---------|-----------|
    |**Host**|Configure the IP addresses of the server.If the server and client reside on the same device, use default address 127.0.0.1. |
    |**Username** and **Password**|Enter the username and password to connect to ossftp. You can use the following methods to obtain the username and password:    -   AccessKey

        -   When you use an AccessKey pair to connect to ossftp, the username consists of a bucket name and an AccessKey ID that has the access permissions on the bucket. The format is `AccessKey ID/Bucket name`. Example: `Y6IoUOZReouXvWaXuwjvDch9******/examplebucket`.
        -   The password is the AccessKey secret that is paired with the AccessKey ID.
For more information about how to obtain an AccessKey pair, see [Create an AccessKey pair]().

    -   Custom username

You can generate a custom username on the ossftp server for the client. For more information about configuration methods, see [Appendix: Create a custom logon account](#section_nx1_u1x_lp7). |
    |**Port**|Enter the listening port configured for ossftp.|

    **Note:** The ossftp server can be connected to only one client at a time. Subsequent connection requests cause the existing connection to disconnect from the client.

7.  Upload and download objects.

    In the FileZilla console, drag an object from the local site on the left side to the remote site on the right side. This way, the object is uploaded. To download an object, drag the object from the remote site to the local site.


## Appendix: Create a custom logon account

Open the config.json file in the installation directory of ossftp. Modify parameters in `accounts`. Keep default settings for other parameters. Configuration example:

```
{
  "modules":{
    "accounts":[
      {
        // Specify the AccessKey ID and AccessKey secret that have the permissions to access the required bucket.
        "access_id":"LTAI4FrfJPUSoKm4JH******",
        "access_secret":"Y6IoUOZReouXvWaXuwjvDch9******",
        // Specify the name of the bucket.
        "bucket_name":"examplebucket",
        // Specify the URLs of the objects in the bucket. After the preceding information is specified, the account can access the objects whose URLs are specified. If no URLs are specified, all objects in the bucket can be accessed.
        "home_dir":"examplefolder/",
        // Customize a password for logons.
        "login_password":"password1",
        // Customize a username for logons.
        "login_username":"user1"
      },
      {
        "access_id":"LTAI4FrfJPUSoKm4JH******",
        "access_secret":"Y6IoUOZReouXvWaXuwjvDch9******",
        "bucket_name":"examplebucket",
        "home_dir":"",
        "login_password":"password2",
        "login_username":"user2"
      }
    ],
    "launcher":{
      "auto_start":0,
      "control_port":8192,
      "language":"cn",
      "popup_webui":1,
      "show_systray":1
    },
    "ossftp":{
      "address":"127.0.0.1",
      "bucket_endpoints":"",
      "log_level":"INFO",
      "passive_ports_start":51000,
      "passive_ports_end":53000,
      "port":2048
    }
  }
}
```

After the configuration file is saved, you must restart the ossftp service on the graphical management interface of ossftp. Otherwise, the configured account does not take effect.

