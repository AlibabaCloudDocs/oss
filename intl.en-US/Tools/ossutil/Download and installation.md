# Download and installation

ossutil supports the following operating systems: Windows, Linux, and macOS. You can download and install the ossutil version that best suits your requirements.

## Version and runtime environment

-   Current version: 1.7.0
-   Source code: [ossutil](https://github.com/aliyun/ossutil)
-   Runtime environment
    -   Windows/Linux/macOS
    -   Supported architectures: x86 \(32-bit and 64-bit\) and ARM \(32-bit and 64-bit\)

## Download URLs

-   [Linux x86 32bit](https://gosspublic.alicdn.com/ossutil/1.7.0/ossutil32)
-   [Linux x86 64bit](https://gosspublic.alicdn.com/ossutil/1.7.0/ossutil64)

    **Note:** When you copy the URLs to the wget command to download ossutil, delete the `? spm=xxxx` section from the URLs.

-   [Windows x86 32bit](https://gosspublic.alicdn.com/ossutil/1.7.0/ossutil32.zip)
-   [Windows x86 64bit](https://gosspublic.alicdn.com/ossutil/1.7.0/ossutil64.zip)
-   [macOS x86 32bit](https://gosspublic.alicdn.com/ossutil/1.7.0/ossutilmac32)
-   [macOS x86 64bit](https://gosspublic.alicdn.com/ossutil/1.7.0/ossutilmac64)
-   [ARM 32bit](https://gosspublic.alicdn.com/ossutil/1.7.0/ossutilarm32)
-   [ARM 64bit](https://gosspublic.alicdn.com/ossutil/1.7.0/ossutilarm64)

## Installation

Download the package based on your operating system and run the corresponding binary file.

-   Install ossutil on Linux \(a 64-bit Linux system is used as an example\)
    1.  Download the ossutil installation package.

        ```
        wget http://gosspublic.alicdn.com/ossutil/1.7.0/ossutil64                           
        ```

    2.  Modify the execution permissions of the file.

        ```
        chmod 755 ossutil64
        ```

    3.  Generate a configuration file in interactive mode.

        1.  Run the following command:

            ```
            ./ossutil64 config
            ```

        2.  Configure the path of the configuration file as prompted.

            We recommend that you use the default path for the configuration file by pressing the Enter key.

            ```
            Please enter the config file name, the file name can include path (default C:\\Users\Administrator\.ossutilconfig, carriage return will use the default file.
            If you specified this option to other file, you should specify --config-file option to the file when you use other commands): 
            ```

            **Note:** ossutil uses the default configuration file /home/user/.ossutilconfig if you do not specify a configuration file in the command. To force ossutil to use a custom configuration file, specify the file by adding the -c option in each command. For example, if you want to use the configuration file /home/config when you run the ls command, add the -c option in the following format:

            ```
            ./ossutil64 ls oss://examplebucket -c /home/config
            ```

        3.  Set the language of ossutil as prompted.

            ```
            Enter the language: CH or EN. The default language is CH. This parameter takes effect after the config command is run. 
            ```

        4.  Configure the parameters including endpoint, AccessKey ID, AccessKey secret, and STS token.

            ```
            Please enter endpoint: Endpoint
            
            ```

            参数说明如下：

            -   endpoint：填写Bucket所在地域的Endpoint。 各地域Endpoint详情，请参见[访问域名和数据中心](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md)。

                您也可以增加`http://`或`https://`指定ossutil访问OSS使用的协议，默认使用HTTP协议。 例如使用HTTPS协议访问杭州的Bucket，可设置为`https://oss-cn-shenzhen.aliyuncs.com`。

            -   accessKeyID、accessKeySecret：填写账号的AccessKey。
                -   使用阿里云账号或RAM用户访问时，AccessKey的获取方式，请参见[Create an AccessKey pair]()。
                -   使用STS临时授权账号访问时，AccessKey的获取方式，请参见[Access OSS with a temporary access credential provided by STS](/intl.en-US/Developer Guide/Data security/Access and control/Access OSS with a temporary access credential provided by STS.md)。
            -   stsToken：使用STS临时授权账号访问OSS时需要配置该项，否则置空即可。 stsToken生成方式参见[临时访问凭证](/intl.en-US/Developer Guide/Objects/Upload files/Authorized third-party upload.md)。
        **Note:** 配置文件更详细的说明，请参见[config](/intl.en-US/Tools/ossutil/Common commands/config.md)。

-   Windows系统（以64位系统为例）
    1.  单击下载链接下载工具。
    2.  将工具解压，并双击运行ossutil.bat文件。
    3.  生成配置文件。 配置方法，请参见[Linux系统生配置文件](#li_j6f_g28_vfx)。

        命令如下：

        ```
        D:\ossutil>ossutil64.exe config
        ```

-   macOS系统（以64位系统为例）
    1.  下载工具。

        ```
        curl -o ossutilmac64 http://gosspublic.alicdn.com/ossutil/1.7.0/ossutilmac64
        ```

    2.  修改文件执行权限。

        ```
        chmod 755 ossutilmac64
        ```

    3.  生成配置文件。 配置方法，请参见[Linux系统生配置文件](#li_j6f_g28_vfx)。

        命令如下：

        ```
        ./ossutilmac64 config
        ```


