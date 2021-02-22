# Installation

This topic describes how to install OSS .NET SDK.

## Preparation

-   Windows
    -   Applicable to `.NET 2.0` and later.
    -   Applicable to `Visual Studio 2010` and later.
-   Linux Mac

    Applicable to `Mono 3.12` and later.


## Download SDK

-   [Direct download](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/32085/cn_zh/1515493045734/aliyun_oss_dotnet_sdk_2_8_0.zip)
-   [Download from GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk.git?spm=a2c4g.11186623.2.15.5fce4144QhQZy7&file=aliyun-oss-csharp-sdk.git)
-   [Download previous versions](https://github.com/aliyun/aliyun-oss-csharp-sdk/releases)

## Install SDK

-   Install OSS .NET SDK in Windows
    -   Install SDK through NuGet
        1.  If your Visual Studio does not have NuGet installed, install [NuGet](http://docs.nuget.org/docs/start-here/installing-nuget) first.
        2.  Create a project or open an existing project in Visual Studio, select **Tools** \> **NuGet Package Manager** \> **Management Solution NuGet Package**.
        3.  Search for aliyun.oss.sdk and find Aliyun.OSS.SDK or Aliyun.OSS.SDK.NetCore in the search results. Select the latest version, and click **Install**.
    -   Install SDK through DLL reference
        1.  Download and unzip the .NET SDK package.
        2.  In the Visual Studio, access Solution Resource Manager, select your project, right click Project Name. Select **Reference** \> **Add Reference**, and then select **Browse** in the prompt dialog box.
        3.  Find thedirectory that the SDK package is unzipped to, select Aliyun.OSS.dll in the bin directory, and click **OK**.
    -   Install SDK through project introduction

        If you download SDK package or the source code from GitHub and you want to install SDK package using the source code, you can

        1.  right click **Solution** in Visual Studio and select **Add** in the dialog box menu to add an existing project.
        2.  In the prompt dialog box, select aliyun-oss-sdk.csproj, and click **Open**.
        3.  Right click Your Projects and select **Reference** \> **Add Reference**. In the prompt dialog box, click the **Project** tab, select the aliyun-oss-sdk project, and then click **OK**.
-   Install OSS .NET SDK in Windows
    -   Install SDK through NuGet
        1.  In Xamarin, create a project or open an existing project, and select **Add NuGet Packages** from Tool.
        2.  Search and find aliyun.oss.sdk or or Aliyun.OSS.SDK.NetCore, select the latest version, and click **Add Package** to add the package to project application.
    -   Install SDK through DLL reference
        1.  Download and unzip the .NET SDK package.
        2.  In Xamarin, select your project in **Solution**, right click **Reference**, and then select **Edit References** in the prompt menu.
        3.  In the Edit References dialog box, select .Net Assembly, click **Browse**. Find the directory that SDK is unzipped to, select Aliyun.OSS.dll in the bin directory, and then click **Open**.

