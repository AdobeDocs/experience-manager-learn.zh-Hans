---
title: 与AEM Forms创建您的第一个OSGi包
description: 使用Maven和Eclipse构建您的第一个OSGi包
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
exl-id: 307cc3b2-87e5-4429-8f21-5266cf03b78f
last-substantial-update: 2021-04-23T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '667'
ht-degree: 1%

---

# 创建您的第一个OSGi包

OSGi包是一个Java™存档文件，其中包含Java代码、资源以及描述包及其依赖项的清单。 包是应用程序的部署单元。 本文面向希望使用AEM Forms 6.4或6.5创建OSGi服务或Servlet的开发人员。要构建您的第一个OSGi包，请执行以下步骤：


## 安装JDK

安装支持的JDK版本。 我已使用JDK1.8。请确保已添加 **JAVA_HOME** ，并指向JDK安装的根文件夹。
将%JAVA_HOME%/bin添加到路径中

![数据源](assets/java-home.JPG)

>[!NOTE]
> 请勿使用JDK 15。 AEM不支持此功能。

### 测试JDK版本

打开新的命令提示窗口并键入： `java -version`. 您应返回由 `JAVA_HOME` 变量

![数据源](assets/java-version.JPG)

## 安装Maven

Maven是一款主要用于Java项目的内部版本自动化工具。 请按照以下步骤在本地系统上安装maven。

* 创建名为 `maven` 在C驱动器中
* 下载 [二进制zip存档](https://maven.apache.org/download.cgi)
* 将zip存档的内容解压缩到 `c:\maven`
* 创建一个名为 `M2_HOME` 值为 `C:\maven\apache-maven-3.6.0`. 就我而言， **mvn** 版本为3.6.0。在编写本文时，最新的maven版本为3.6.3
* 添加 `%M2_HOME%\bin` 你的路
* 保存更改
* 打开新的命令提示符并键入 `mvn -version`. 您应会看到 **mvn** 列出的版本，如以下屏幕截图所示

![数据源](assets/mvn-version.JPG)


## 安装Eclipse

安装最新版本的 [eclipe](https://www.eclipse.org/downloads/)

## 创建您的第一个项目

原型是Maven项目模板工具包。 原型被定义为原始模式或模型，从中可以制造所有同类的其它事物。 此名称适合我们尝试提供一个系统，以提供一致的Maven项目生成方法。 原型可帮助作者为用户创建Maven项目模板，并为用户提供方法来生成这些项目模板的参数化版本。
要创建您的第一个Maven项目，请执行以下步骤：

* 创建一个名为 `aemformsbundles` 在C驱动器中
* 打开命令提示符并导航到 `c:\aemformsbundles`
* 在命令提示符下运行以下命令

```java
mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate -D archetypeGroupId=com.adobe.aem -D archetypeArtifactId=aem-project-archetype -D archetypeVersion=36 -D appTitle="My Site" -D appId="mysite" -D groupId="com.mysite" -D aemVersion=6.5.14
```

成功完成后，您应会在命令窗口中看到生成成功消息

## 从maven项目创建Eclipse项目

* 将工作目录更改为 `mysite`
* 执行 `mvn eclipse:eclipse` 命令行中。 该命令会读取您的pom文件并使用正确的元数据创建Eclipse项目，以便Eclipse了解项目类型、关系、类路径等。

## 将项目导入Eclipse

Launch **Eclipse**

转到 **文件 — >导入** 选择 **现有Maven项目** 如下所示

![数据源](assets/import-mvn-project.JPG)

单击下一步

选择c:\aemformsbundles\mysite by clicking the **浏览** 按钮

![数据源](assets/mysite-eclipse-project.png)

>[!NOTE]
>您可以根据需要选择导入相应的模块。 仅当您只想在项目中创建Java代码时，才选择并导入核心模块。

单击 **完成** 启动导入流程

项目已导入到Eclipse中，您会看到 `mysite.xxxx` 文件夹

展开 `src/main/java` 下 `mysite.core` 文件夹。 这是您在其中编写大多数代码的文件夹。

![数据源](assets/mysite-core-project.png)

## 包括AEMFD客户端SDK

您需要在项目中包含AEMFD客户端sdk，以利用AEM Forms附带的各种服务。 请参阅 [AEMFD客户端SDK](https://mvnrepository.com/artifact/com.adobe.aemfd/aemfd-client-sdk) 以在您的Maven项目中包含相应的客户端SDK。 您必须在的依赖项部分中包含AEM FD客户端SDK `pom.xml` 的子项目，如下所示。

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

要构建项目，请执行以下步骤：

* 打开 **命令提示符窗口**
* 导航至 `c:\aemformsbundles\mysite\core`
* 执行命令 `mvn clean install -PautoInstallBundle`
上述命令在运行的AEM服务器中生成并安装包 `http://localhost:4502`. 该包也可在位于的文件系统上使用
   `C:\AEMFormsBundles\mysite\core\target` 和可以使用 [Felix Web控制台](http://localhost:4502/system/console/bundles)
