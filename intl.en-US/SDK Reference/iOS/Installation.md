# Installation

This topic describes how to install the OSS iOS SDK.

## Environment requirements

-   iOS: 8.0 or later
-   MacOS: 10.10 or later

## Directly import the framework

For more information about how to obtain the OSS iOS SDK framework, see [GitHub](https://github.com/aliyun/aliyun-oss-ios-sdk/blob/master/README-CN.md).

In Xcode, drag the framework and drop it to your target area. In the dialog box that appears, select **Copy items if needed**.

## Pod dependency

If your project uses Cocoapods to manage dependencies, add the following dependency to the Podfile. In this case, you do not need to import the OSS iOS SDK framework.

```
pod 'AliyunOSSiOS'
            
```

**Note:** You can choose either of the preceding methods.

## Import the header file to your project

```
#import <AliyunOSSiOS/OSSService.h>
            
```

**Note:** After you import the framework, add `-ObjC` to `Other Linker Flags` of `Build Settings` in your project. If the `-force_load` option is configured for your project, add `-force_load <framework path>/AliyunOSSiOS`.

## Compatible with IPv6-only networks

Domain name resolution in wireless networks is subject to hijackings. To address this issue, the OSS mobile SDK imports HTTPDNS for domain name resolution and directly uses IP addresses to request the OSS server. In an IPv6-only network, compatibility issues may occur. Apple has officially issued the review requirements for applications, requiring applications to be IPv6-only network compatible. To meet this requirement, the SDK starts to be compatible from V2.5.0. In the later version, aside from -ObjC settings, two system libraries must be imported:

```
libresolv.tbd
CoreTelephony.framework
SystemConfiguration.framework
            
```

## About the ATS policy of Apple

During WWDC 2016, Apple announced that all the applications in Apple App Store must implement App Transport Security \(ATS\) from January 1, 2017. In other words, all newly submitted applications are not allowed to use `NSAllowsArbitraryLoads` to bypass the ATS limitation. Additionally, the application submitters must ensure that all network requests from the application are encrypted through HTTPS. Otherwise, the application may fail the review.

The OSS iOS SDK V`2.6.0` and later provide the support for this ATS policy. The SDK sends only HTTPS requests. The SDK supports the prefix of `https://` in the `Endpoint` field value. You need only to set the correct HTTPS `Endpoint` field. In this way, all network requests can meet the requirements.

**Note:**

-   When you set the `Endpoint` field, you must use the URL that is prefixed with `https://`.
-   Ensure that the application initiates only HTTPS requests when implementing callbacks such as signing and obtaining STS tokens.

