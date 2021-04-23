# Download and installation

ossutil supports the following operating systems: Windows, Linux, and macOS. You can download and install the ossutil version that best suits your requirements.

## Version and runtime environment

-   Current version: 1.7.3
-   Source code: [ossutil](https://github.com/aliyun/ossutil)
-   Runtime environment
    -   Windows/Linux/macOS
    -   Supported architectures: x86 \(32-bit and 64-bit\) and ARM \(32-bit and 64-bit\)

## Download URLs

-   [Linux x86 32bit](https://gosspublic.alicdn.com/ossutil/1.7.3/ossutil32)
-   [Linux x86 64bit](https://gosspublic.alicdn.com/ossutil/1.7.3/ossutil64)
-   [Windows x86 32bit](https://gosspublic.alicdn.com/ossutil/1.7.3/ossutil32.zip)
-   [Windows x86 64bit](https://gosspublic.alicdn.com/ossutil/1.7.3/ossutil64.zip)
-   [macOS x86 32bit](https://gosspublic.alicdn.com/ossutil/1.7.3/ossutilmac32)
-   [macOS x86 64bit](https://gosspublic.alicdn.com/ossutil/1.7.3/ossutilmac64)
-   [ARM 32bit](https://gosspublic.alicdn.com/ossutil/1.7.3/ossutilarm32)
-   [ARM 64bit](https://gosspublic.alicdn.com/ossutil/1.7.3/ossutilarm64)

Download the package based on your operating system and run the corresponding binary file. In the topic, ossutil is installed on 64-bit operating systems.

## Install ossutil on Linux

1.  Run the following command to download the ossutil installation package:

    ```
    wget http://gosspublic.alicdn.com/ossutil/1.7.3/ossutil64                           
    ```

    **Note:** When you use the copied URL in the wget command to download ossutil, delete the`?spm=xxxx` part from the URL.

2.  Run the following command to modify the execution permissions of the file:

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

        By default, ossutil uses the configuration file /home/user/.ossutilconfig if you do not specify a configuration file in the command. To force ossutil to use a configuration file that you configure, add the -c option in each command to specify the configuration file. For example, if you want to use the configuration file /home/config when you run the ls command, add the -c option in the following format:

        ```
        ./ossutil64 ls oss://examplebucket -c /home/config
        ```

    3.  Set the language of ossutil as prompted.

        ```
        Enter the language: CH or EN. The default language is CH. This parameter takes effect after the config command is run. 
        ```

    4.  Configure the parameters, including endpoint, AccessKey ID, AccessKey secret, and Security Token Service \(STS\) token.

        You can configure the following parameters for ossutil:

        -   endpoint: Enter the endpoint of the region in which your bucket is located. For more information about the endpoint of each region, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).

            You can also add `http://` or `https://` to specify the protocol that ossutil uses to access OSS. The default protocol is HTTP. For example, if you want to access a bucket in the China \(Shenzhen\) region by using HTTPS, set the endpoint to `https://oss-cn-shenzhen.aliyuncs.com`.

        -   accessKeyID and accessKeySecret: Enter the AccessKey pair of your account.
            -   For more information about how to obtain the AccessKey pair of an Alibaba Cloud account or a RAM user, see [Create an AccessKey pair]().
            -   For more information about how to obtain the AccessKey pair of a temporary STS token, see [Access OSS with a temporary access credential provided by STS](/intl.en-US/Developer Guide/Data security/Access and control/Access OSS with a temporary access credential provided by STS.md).
        -   stsToken: This option is required only when you use a temporary STS token to access an OSS bucket. Otherwise, you can leave this parameter empty. For more information about how to generate an STS token, see [Access OSS with a temporary access credential provided by STS](/intl.en-US/Developer Guide/Objects/Upload files/Authorized third-party upload.md).
    **Note:** For more information about configuration files, see [config](/intl.en-US/Tools/ossutil/Common commands/config.md).


## Install ossutil on Windows

1.  Click the download URL listed in the preceding section to download the installation package of ossutil.
2.  Decompress the downloaded installation package. Run the ossutil.bat file.
3.  Run the following command to generate a configuration file:

    ```
    D:\ossutil>ossutil64.exe config
    ```

4.  Configure the configuration file as prompted. You can configure the configuration file in the same way as you generate configuration files in Linux. For more information, see [Generate a configuration file in interactive mode](#linux).

## Install ossutil on macOS

1.  Run the following command to download the ossutil installation package:

    ```
    curl -o ossutilmac64 http://gosspublic.alicdn.com/ossutil/1.7.3/ossutilmac64
    ```

2.  Run the following command to modify the execution permissions of the file:

    ```
    chmod 755 ossutilmac64
    ```

3.  Run the following command to generate a configuration file:

    ```
    ./ossutilmac64 config
    ```

4.  Configure the configuration file as prompted. You can configure the configuration file in the same way as you generate configuration files in Linux. For more information, see [Generate a configuration file in interactive mode](#linux).

## Install ossutil on ARM systems

1.  Run the following command to download the ossutil installation package:

    ```
    wget http://gosspublic.alicdn.com/ossutil/1.7.3/ossutilarm64
    ```

2.  Run the following command to modify the execution permissions of the file:

    ```
    chmod 755 ossutilarm64
    ```

3.  Run the following command to generate a configuration file:

    ```
    ./ossutilarm64 config
    ```

4.  Configure the configuration file as prompted. You can configure the configuration file in the same way as you generate configuration files in Linux. For more information, see [Generate a configuration file in interactive mode](#linux).

