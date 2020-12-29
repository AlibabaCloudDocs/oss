# Installation

Before you perform operations on OSS, you must install OSS SDK for Java. This topic describes how to install OSS SDK for Java in various scenarios. Install OSS SDK for Java based on your requirements.

## Prerequisites

Java 7 or later is installed.

You can run the java -version command to check the Java version.

## Download OSS SDK for Java

-   [Download SDK installation package](https://gosspublic.alicdn.com/sdks/java/aliyun_java_sdk_3.10.2.zip)
-   [Download from GitHub](https://github.com/aliyun/aliyun-oss-java-sdk)
-   [Download previous versions](https://github.com/aliyun/aliyun-oss-java-sdk/releases)

## Install OSS SDK for Java

You can use one of the following method to install OSS SDK for Java:

-   Method 1: \(Recommended\) Add dependencies to your Maven project

    To use OSS SDK for Java in Maven, you only need to add the corresponding dependencies to the pom.xml file. For OSS SDK for Java V3.10.2, add the following content to <dependencies\>:

    ```
    <dependency>
        <groupId>com.aliyun.oss</groupId>
        <artifactId>aliyun-sdk-oss</artifactId>
        <version>3.10.2</version>
    </dependency>
    ```

    If you use Java 9 or later versions, you must add dependencies on jaxb. Add the following content to <dependencies\>:

    ```
    <dependency>
        <groupId>javax.xml.bind</groupId>
        <artifactId>jaxb-api</artifactId>
        <version>2.3.1</version>
    </dependency>
    <dependency>
        <groupId>javax.activation</groupId>
        <artifactId>activation</artifactId>
        <version>1.1.1</version>
    </dependency>
    <! -- no more than 2.3.3-->
    <dependency>
        <groupId>org.glassfish.jaxb</groupId>
        <artifactId>jaxb-runtime</artifactId>
        <version>2.3.3</version>
    </dependency>
    ```

-   Method 2: Import the JAR package to your Eclipse project

    For OSS SDK for Java V3.10.2, perform the following operations to import the JAR package to your Eclipse project:

    1.  Download the [OSS SDK for Java package](https://gosspublic.alicdn.com/sdks/java/aliyun_java_sdk_3.10.2.zip).
    2.  Decompress the package.
    3.  Copy the aliyun-sdk-oss-3.10.2.jar file and all files in the lib folder to your project.
    4.  Right-click your project name in Eclipse and choose **Properties** \> **Java Build Path** \> **Add JARs**.
    5.  Select all JAR files that you copy and import them to Libraries.
-   Method 3: Import the JAR package to IntelliJ IDEA

    For OSS SDK for Java V3.10.2, perform the following operations to import the JAR package to IntelliJ IDEA:

    1.  Download the [OSS SDK for Java package](https://gosspublic.alicdn.com/sdks/java/aliyun_java_sdk_3.10.2.zip).
    2.  Decompress the package.
    3.  Copy the aliyun-sdk-oss-3.10.2.jar file and all JAR files in the lib folder to your project.
    4.  In IntelliJ IDEA, select your project and choose **File** \> **Project Structure** \> **Modules** \> **Dependencies** \> **+** \> **JARs or directories** in the right-click menu.
    5.  Select all JAR files that you copy and import them to External Libraries.

