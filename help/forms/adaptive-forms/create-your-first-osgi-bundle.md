---
title: 创建您的第一个OSGi包和AEM表单
description: 使用maven和eclipse构建您的第一个OSGi包
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2021-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '820'
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
* 下载 [二进制zip存档](http://maven.apache.org/download.cgi)
* 将zip存档的内容解压缩到 `c:\maven`
* 创建一个名为 `M2_HOME` 值为 `C:\maven\apache-maven-3.6.0`. 就我而言， **mvn** 版本为3.6.0。在编写本文时，最新的maven版本为3.6.3
* 添加 `%M2_HOME%\bin` 你的路
* 保存更改
* 打开新的命令提示符并键入 `mvn -version`. 您应会看到 **mvn** 列出的版本，如以下屏幕截图所示

![数据源](assets/mvn-version.JPG)

## Settings.xml

玛文 `settings.xml` 文件定义了用各种方式配置Maven执行的值。 通常，它用于定义本地存储库位置、备用远程存储库服务器以及专用存储库的身份验证信息。

导航到 `C:\Users\<username>\.m2 folder`
提取的内容 [settings.zip](assets/settings.zip) 文件并放入 `.m2` 文件夹。

## 安装Eclipse

安装最新版本的 [eclipe](https://www.eclipse.org/downloads/)

## 创建您的第一个项目

原型是Maven项目模板工具包。 原型被定义为原始模式或模型，从中可以制造所有同类的其它事物。 此名称适合我们尝试提供一个系统，以提供一致的Maven项目生成方法。 原型可帮助作者为用户创建Maven项目模板，并为用户提供方法来生成这些项目模板的参数化版本。
要创建您的第一个Maven项目，请执行以下步骤：

* 创建一个名为 `aemformsbundles` 在C驱动器中
* 打开命令提示符并导航到 `c:\aemformsbundles`
* 在命令提示符下运行以下命令
* `mvn archetype:generate  -DarchetypeGroupId=com.adobe.granite.archetypes  -DarchetypeArtifactId=aem-project-archetype -DarchetypeVersion=19`

Maven项目是以交互方式生成的，系统会要求您为许多属性提供值，例如

| 属性名称 | 显着性 | 值 |
|------------------------|---------------------------------------|---------------------|
| groupId | groupId可在所有项目中唯一标识您的项目 | com.learningaemforms.adobe |
| appsFolderName | 保存项目结构的文件夹的名称 | 学习Aemforms |
| artifactId | artifactId是不带版本的jar的名称。 如果您创建了该名称，则可以选择您想要的任何名称，并使用小写字母而不使用奇怪的符号。 | 学习Aemforms |
| 版本 | 如果您分发它，则可以选择任何带数字和点(1.0、1.1、1.0.1、...)的典型版本。 | 1.0 |

通过按Enter键接受其他属性的默认值。
如果一切正常，您应会在命令窗口中看到生成成功消息

## 从maven项目创建Eclipse项目

将工作目录更改为 `learningaemforms`.
正在执行 `mvn eclipse:eclipse` 从命令行上述命令读取您的pom文件，并使用正确的元数据创建Eclipse项目，以便Eclipse了解项目类型、关系、类路径等。

## 将项目导入Eclipse

Launch **Eclipse**

转到 **文件 — >导入** 选择 **现有Maven项目** 如下所示

![数据源](assets/import-mvn-project.JPG)

单击下一步

选择 `c:\aemformsbundles\learningaemform`通过单击 **浏览** 按钮

![数据源](assets/select-mvn-project.JPG)

>[!NOTE]
>您可以根据需要选择导入相应的模块。 仅当您只想在项目中创建Java代码时，才选择并导入核心模块。

单击 **完成** 启动导入流程

项目已导入到Eclipse中，您会看到 `learningaemforms.xxxx` 文件夹

展开 `src/main/java` 下 `learningaemforms.core` 文件夹。 这是您在其中编写大多数代码的文件夹。

![数据源](assets/learning-core.JPG)

## 构建项目

编写OSGi服务或Servlet后，您需要构建项目以生成可使用Felix Web控制台部署的OSGi包。 请参阅 [AEMFD客户端SDK](https://repo.adobe.com/nexus/content/repositories/public/com/adobe/aemfd/aemfd-client-sdk/) 以在您的Maven项目中包含相应的客户端SDK。 必须在的依赖项部分中包含AEM FD客户端SDK `pom.xml` 的子项目，如下所示。

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

要构建项目，请执行以下步骤：

* 打开 **命令提示符窗口**
* 导航至 `c:\aemformsbundles\learningaemforms\core`
* 执行命令 `mvn clean install`
如果一切正常，您应会在以下位置看到包 `C:\AEMFormsBundles\learningaemforms\core\target`. 此包现已准备就绪，可使用Felix Web控制台部署到AEM中。
