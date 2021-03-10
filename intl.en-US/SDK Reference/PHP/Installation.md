# Installation

## Preparation

OSS PHP SDK is applicable to PHP 5.3 and later versions. PHP 5.6.22 is used in this topic as an example.

-   Installation environment

    You must install PHP cURL extensions:

    -   In Windows, Compile and use Alibaba Cloud OSS PHP SDK in Windows to install PHP cURL extension. In Windows, if you cannot find the required module, specify the extension\_dir to `C:/Windows/System32/` in php.ini.
    -   In Ubuntu, you can use the apt-get package manager to install the PHP cURL extension by running `sudo apt-get install php-curl`.
    -   In CentOS, you can use the yum package manager to install the PHP cURL extension by running `sudo yum install php-curl`.
-   Check PHP version
    -   Run `php -v` to check the current PHP version.
    -   Run `php -m` to check whether cURL extension is installed.

## Download SDK

-   [Download from GitHub](https://github.com/aliyun/aliyun-oss-php-sdk)
-   [Download previous versions](https://github.com/aliyun/aliyun-oss-php-sdk/releases)

For more information, see [OSS API documents](http://gosspublic.alicdn.com/AliyunPHPSDK/latest/apidocs/index.html).

**Note:** We recommend that you use the latest version of the SDK. You can download documents of OSS PHP SDK 2.0.0 and earlier versions from [here](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/internal/oss/0.0.4/assets/pdf/oss_sdk_php20150819.pdf).

## Install SDK

You can install the SDK in three methods:

-   Composer

    You can install the SDK through composer as follows:

    1.  Run `composer require aliyuncs/oss-sdk-php` in the root directory of your project, or declare the dependency on Alibaba Cloud OSS SDK for PHP in `composer.json` as follows:

        ```
        
        "require": {
            "aliyuncs/oss-sdk-php": "~2.4"
        }
                                    
        ```

    2.  Run `composer install` to install the dependency. After the dependency is installed, check whether your directory structure complies with the following structure:

        ```
                .
                ├── app.php
                ├── composer.json
                ├── composer.lock
                └── vendor
                                    
        ```

        The `app.php` is a user application. The `vendor/` directory contains the dependent library. You must introduce the dependency in `app.php`:

        ```
        require_once __DIR__ . '/vendor/autoload.php';
                                    
        ```

    **Note:**

    -   If your project references `autoload.php`, you do not need to introduce the file again after adding the SDK dependency.
    -   If a network error occurs when you use the Composer Dependency Manager, you can use the [Image Source](http://pkg.phpcomposer.com/) in the Composer China region by running the following command: `composer config -g repositories.packagist composer http://packagist.phpcomposer.com`.
-   Phar

    You can install the SDK through phar as follows:

    1.  Select the required version and download the Phar package on [GitHub](https://github.com/aliyun/aliyun-oss-php-sdk/releases).
    2.  Introduce the file in your code:

        ```
        require_once '/path/to/oss-sdk-php.phar';
                                    
        ```


## Source code

You can install the SDK through source code as follows:

1.  Select the required version and download the zip package on [GitHub](https://github.com/aliyun/aliyun-oss-php-sdk/releases).
2.  Find the `autoload.php` file in the root directory of the unzipped package, and introduce the file in your code:

    ```
    require_once '/path/to/oss-sdk/autoload.php';
                        
    ```


