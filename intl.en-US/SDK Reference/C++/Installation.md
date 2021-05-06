# Installation

This topic describes how to install OSS SDK for C++ and configure compilation options in different operating systems.

## Prerequisites

-   A compiler that supports C++ V11 and later is installed.
-   Visual Studio 2013 or later is installed.
-   GCC 4.8 or later is installed.
-   Clang 3.3 or later is installed.

## Download the SDK

[Download directly](https://gosspublic.alicdn.com/doc/c-sdk/aliyun-oss-cpp-sdk-master.zip)

[Download from GitHub](https://github.com/aliyun/aliyun-oss-cpp-sdk.git)

## Install the SDK

You can install OSS SDK for C++ in Linux, Windows, Android, and Mac.

-   Linux:

    1.  Install CMake

        Install CMake 3.1 and later. Open the SDK installation package and use CMake to compile and generate the required files.

        The compilation commands are as follows:

        ```
        cd <path/to/aliyun-oss-cpp-sdk>
        mkdir build
        cd build
        cmake ..
        ```

    2.  Install libcurl and openssl

        RedHat or CentOS:

        ```
        yum –y install libcurl-devel openssl-devel
        ```

        Fedora:

        ```
        sudo dnf install libcurl-devel openssl-devel
        ```

    3.  Install the SDK

        ```
        make && make install
        ```

    **Note:** RTTI is disabled in OSS SDK for C++ by default. Therefore, add the following options when you use g++ to install the SDK: -std=c++11, -fno-rtti, -lalibabacloud-oss-cpp-sdk, -lcurl, -lcrypto, and -lpthread.

    Example:

    ```
    g++ test.cpp -std=c++11 -fno-rtti -lalibabacloud-oss-cpp-sdk -lcurl -lcrypto -lpthread -o test.bin
    ```

-   Windows:
    1.  Install CMake Open the Command Prompt and go to the directory that stores the SDK files. Create a folder named build, and run cmake .. to generate required files, as shown in the following figure:

        ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7286498951/p38252.png)

        **Note:**

        -   The SDK package does not include the project file alibabacloud-oss-cpp-sdk.sln. You must run the cmake command to generate required project files.
        -   To build a x64 structure, run the cmake -A x64 .. command.
    2.  Run VS developer command prompt as an administrator. Go to the build folder and run the following command to compile and install the SDK:

        ```
        msbuild ALL_BUILD.vcxproj
        msbuild INSTALL.vcxproj
        ```

        You can also use Visual Studio to open the project file alibabacloud-oss-cpp-sdk.sln to generate a solution.

-   Android:

    Build a project based on the android-ndk-r16 tool chain in Linux. Install third-party libraries libcurl and libopenssl in $ANDROID\_NDK/sysroot and run the following command to compile and install the SDK:

    ```
    cmake -DCMAKE_TOOLCHAIN_FILE=$ANDROID_NDK/build/cmake/android.toolchain.cmake  \
          -DANDROID_NDK=$ANDROID_NDK    \
          -DANDROID_ABI=armeabi-v7a     \
          -DANDROID_TOOLCHAIN=clang     \
          -DANDROID_PLATFORM=android-21 \
          -DANDROID_STL=c++_shared ..
    make
    ```

-   Mac:

    Specify the installation path of OpenSSL in Mac.

    For example, if OpenSSL is installed in /usr/local/Cellar/openssl/1.0.2p, run the following command to compile and install the SDK:

    ```
    cmake -DOPENSSL_ROOT_DIR=/usr/local/Cellar/openssl/1.0.2p  \
          -DOPENSSL_LIBRARIES=/usr/local/Cellar/openssl/1.0.2p/lib  \
          -DOPENSSL_INCLUDE_DIRS=/usr/local/Cellar/openssl/1.0.2p/include/ ..
    make
    ```


## Compilation options

You can configure different compilation options in different scenarios in the following format: cmake -D$\{OptionName\}=ON\|OFF ..

The following table describes the compilation options that you can configure.

|Option|Description|
|------|-----------|
|BUILD\_SHARED\_LIBS|Build dynamic libraries. Default value: OFF. If you set this option to ON, dynamic libraries and static libraries are built at the same time, and the -static suffix is added to the name of static libraries. To build dynamic libraries in the compilation, run the cmake -DBUILD\_SHARED\_LIBS=ON .. command. |
|ENABLE\_RTTI|Supports Run-Time Type Information. Default value: ON.|
|OSS\_DISABLE\_BUCKET|Does not build interfaces related to buckets. Default value: OFF. You can set this option to ON to reduce code size.|
|OSS\_DISABLE\_LIVECHANNEL|Does not build interfaces related to LivaChannels. Default value: OFF. You can set this option to ON to reduce code size.|
|OSS\_DISABLE\_RESUAMABLE|Does not build interfaces related to resumble upload and download. Default value: OFF. You can set this option to ON to reduce code size.|
|OSS\_DISABLE\_ENCRYPTION|Does not build interfaces related to client-side encryption. Default value: OFF. You can set this option to ON to reduce code size.|

