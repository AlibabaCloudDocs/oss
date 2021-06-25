# Installation

This topic describes how to install OSS SDK for Android.

**Note:** We recommend that you specify the version of OSS SDK for Android when you use OSS SDK for Android.

## Prerequisites

-   Android 2.3 or later is used.
-   OSS is activated.

## Download OSS SDK for Android

-   OSS SDK for Android package

    Add the following dependency in Android Studio:

    ```
    dependencies { compile 'com.aliyun.dpa:oss-android-sdk:2.9.5' }
    ```

-   Download OSS SDK for Android at [aliyun-oss-android-sdk](https://github.com/aliyun/aliyun-oss-android-sdk)

## Install OSS SDK for Android

You can use the following two methods to install OSS SDK for Android:

-   Method 1 \(recommended\): Add the following dependency to your Maven project.

    ```
    dependencies {
        compile 'com.aliyun.dpa:oss-android-sdk:2.9.5'
    }                    
    ```

-   Method 2: Compile the source code to generate a JAR package and import JAR packages to the libs directory.
    1.  Clone the source code of the project and run the gradle command to generate a JAR package:

        ```
        # Clone the source code of the project.
        $ git clone https://github.com/aliyun/aliyun-oss-android-sdk.git
        
        # Enter the directory of the project.
        $ cd aliyun-oss-android-sdk/oss-android-sdk/
        
        # Run the script to package the source code. JDK 1.7 or later is required.
        $ ../gradlew releaseJar
        
        # Enter the directory where the generated JAR package is stored.
        $ cd build/libs && ls                            
        ```

    2.  Import the generated JAR package.

        Import the following three JAR packages to the libs directory: aliyun-oss-sdk-android-2.9.5.jar, [okhttp-3.x.x.jar](https://square.github.io/okhttp/#download), and [okio-1.x.x.jar](https://search.maven.org/remote_content?g=com.squareup.okio&a=okio&v=LATEST).


## Related settings

This section describes the related settings during the installation of OSS SDK for Android.

-   Permission settings

    The following permissions are required to use OSS SDK for Android:

    **Note:** Ensure the permissions are set in the AndroidManifest.xml file for the normal operating of OSS SDK for Android.

    ```
    <uses-permission android:name="android.permission.INTERNET"></uses-permission>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"></uses-permission>                   
    ```

-   ProGuard settings

    Add the following code to the ProGuard settings:

    ```
    -keep class com.alibaba.sdk.android.oss.** { *; }
    -dontwarn okio.**
    -dontwarn org.apache.commons.codec.binary. **                    
    ```


