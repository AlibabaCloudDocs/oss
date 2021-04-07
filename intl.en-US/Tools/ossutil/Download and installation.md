# Download and installation

ossutil supports the following operating systems: Windows, Linux, and macOS. You can download and install the ossutil version that best suits your requirements.

## Version and runtime environment

-   Current version: 1.7.2
-   Source code: [ossutil](https://github.com/aliyun/ossutil)
-   Runtime environment
    -   Windows/Linux/macOS
    -   Supported architectures: x86 \(32-bit and 64-bit\) and ARM \(32-bit and 64-bit\)

## Download URLs

-   [Linux x86 32bit](https://gosspublic.alicdn.com/ossutil/1.7.2/ossutil32)
-   [Linux x86 64bit](https://gosspublic.alicdn.com/ossutil/1.7.2/ossutil64)
-   [Windows x86 32bit](https://gosspublic.alicdn.com/ossutil/1.7.2/ossutil32.zip)
-   [Windows x86 64bit](https://gosspublic.alicdn.com/ossutil/1.7.2/ossutil64.zip)
-   [macOS x86 32bit](https://gosspublic.alicdn.com/ossutil/1.7.2/ossutilmac32)
-   [macOS x86 64bit](https://gosspublic.alicdn.com/ossutil/1.7.2/ossutilmac64)
-   [ARM 32bit](https://gosspublic.alicdn.com/ossutil/1.7.2/ossutilarm32)
-   [ARM 64bit](https://gosspublic.alicdn.com/ossutil/1.7.2/ossutilarm64)

## Installation

Download the package based on your operating system and run the corresponding binary file.

-   Install ossutil on Linux \(a 64-bit Linux system is used as an example\)
    1.  Download the ossutil installation package.

        ```
        wget http://gosspublic.alicdn.com/ossutil/1.7.2/ossutil64                           
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

            You can configure the following parameters for ossutil:

            -   endpoint: Enter the endpoint of the region in which your bucket resides. For more information about the endpoint of each region, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).

                You can also add `http://` or `https://` to specify the protocol that ossutil uses to access OSS. The default protocol is HTTP. For example, if you want to access a bucket in the China \(Hangzhou\) region by using HTTPS, set the endpoint to `https://oss-cn-shenzhen.aliyuncs.com`.

            -   accessKeyID and accessKeySecret: Enter the AccessKey pair of your account.
                -   For more information about how to obtain the AccessKey pair of an Alibaba Cloud account or a RAM user, see [Create an AccessKey pair]().
                -   For more information about how to obtain the AccessKey pair of a temporary STS token, see [Access OSS with a temporary access credential provided by STS](/intl.en-US/Developer Guide/Data security/Access and control/Access OSS with a temporary access credential provided by STS.md).
            -   stsToken: This option is required only when you use a temporary STS token to access OSS buckets. Otherwise, you can leave this parameter empty. For more information about how to generate an STS token, see [Temporary access credential](/intl.en-US/Developer Guide/Objects/Upload files/Authorized third-party upload.md).
        **Note:** For more information about the configuration file of ossutil, see [config](/intl.en-US/Tools/ossutil/Common commands/config.md).

-   Install ossutil on Windows \(a 64-bit Windows system is used as an example\)
    1.  Click the download URL listed in the preceding section to download the installation package of ossutil.
    2.  Decompress the package. Double-click the ossutil.bat file.
    3.  Generate a configuration file. For more information about how to generate a configuration file, see [Generate a configuration file in interactive mode](#li_j6f_g28_vfx).

        You can run the following command to generate a configuration file:

        ```
        D:\ossutil>ossutil64.exe config
        ```

-   Install ossutil on macOS \(a 64-bit macOS system is used as an example\)
    1.  Download the ossutil installation package.

        ```
        curl -o ossutilmac64 http://gosspublic.alicdn.com/ossutil/1.7.2/ossutilmac64
        ```

    2.  Modify the execution permissions of the file.

        ```
        chmod 755 ossutilmac64
        ```

    3.  Generate a configuration file. For more information about how to generate a configuration file, see [Generate a configuration file in interactive mode](#li_j6f_g28_vfx).

        You can run the following command to generate a configuration file:

        ```
        ./ossutilmac64 config
        ```


