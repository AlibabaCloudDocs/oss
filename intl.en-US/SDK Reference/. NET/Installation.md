# Installation

This topic describes how to install OSS SDK for .NET.

## Prerequisites

-   Windows
    -   .NET 2.0 or later is required.
    -   Visual Studio 2010 or later is required.
-   Linux/macOS

    Mono 3.12 or later is required.


## Download OSS SDK for .NET

-   [Download SDK installation package](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/32085/cn_zh/1515493045734/aliyun_oss_dotnet_sdk_2_8_0.zip)
-   [Download from GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk.git?spm=a2c4g.11186623.2.15.5fce4144QhQZy7&file=aliyun-oss-csharp-sdk.git)
-   [Download previous versions](https://github.com/aliyun/aliyun-oss-csharp-sdk/releases)

## Install OSS SDK for .NET

-   Install OSS SDK for .NET in Windows
    -   Install by using NuGet
        1.  If NuGet is not installed for your Visual Studio , you must first install NuGet. For more information, visit [Install NuGet client tools](http://docs.nuget.org/docs/start-here/installing-nuget).
        2.  After you install NuGet, create a project or open an existing project in Visual Studio, and then choose **Tools** \> **NuGet Package Manager** \> **Manage NuGet Packages for Solution**.
        3.  Enter aliyun.oss.sdk in the search box and click Search. Find Aliyun.OSS.SDK \(applies to .NET Framework\) or Aliyun.OSS.SDK.NetCore \(applies to .Net Core\) in the search results. Select the latest version, and click **Install**.
    -   Install by using a DLL file
        1.  Download and decompress the installation package of OSS SDK for .NET.
        2.  In Visual Studio, go to Solution Explorer and find your project. Right-click the project name and choose **Reference** \> **Add Reference**. In the dialog box that appears, select **Browse**.
        3.  Find the directory to which the installation package is decompressed and select the Aliyun.OSS.dll in the bin folder. Click **OK**.
    -   Install by importing a project

        If you download the SDK installation package or download the source code from GitHub and use the source code to install the SDK, perform the following operations:

        1.  In Visual Studio, right-click and select **Solution** from the menu that appears. Select Add \> **Existing projects**.
        2.  In the dialog box that appears, select the aliyun-oss-sdk.csproj file. Click **Open**.
        3.  Right-click the name of your project and choose **Reference** \> **Add Reference**. In the dialog box that appears, click the **Projects** tab, select the aliyun-oss-sdk project, and click **OK**.
-   Install OSS SDK for .NET in Unix and macOS
    -   Install by using NuGet
        1.  In Xamarin, create a project or open an existing project. Select **Add NuGet** from Tools.
        2.  Search and find Aliyun.OSS.SDK or Aliyun.OSS.SDK.NetCore. Select the latest version, and click **Add Package** to add the package to the project application.
    -   Install by using a DLL file
        1.  Download and decompress the installation package of OSS SDK for .NET.
        2.  In Xamarin, select your project in **Solution**. Right click and select **Reference**. In the menu that appears, select **Edit References**.
        3.  In the Edit References dialog box, select .Net Assembly and click **Browse**. Find the directory to which the installation package is decompressed and select Aliyun.OSS.dll in the bin folder. Click **Open**.

