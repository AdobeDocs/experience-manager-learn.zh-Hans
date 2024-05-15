---
title: 使用AEM Forms创建您的第一个OSGi捆绑包
description: 使用Maven和Eclipse构建您的第一个OSGi捆绑包
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
exl-id: 307cc3b2-87e5-4429-8f21-5266cf03b78f
last-substantial-update: 2021-04-23T00:00:00Z
duration: 145
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '665'
ht-degree: 0%

---

# 创建您的第一个OSGi捆绑包

OSGi捆绑包是一个Java™存档文件，其中包含Java代码、资源以及描述捆绑包及其依赖关系的清单。 捆绑是应用程序的部署单元。 本文适用于希望使用AEM Forms 6.4或6.5创建OSGi服务或servlet的开发人员。要构建您的第一个OSGi捆绑包，请执行以下步骤：


## 安装JDK

安装支持的JDK版本。 我使用的是JDK1.8。确保您已添加 **JAVA_HOME** 环境变量中的且指向JDK安装的根文件夹。
将%JAVA_HOME%/bin添加到路径中

![数据源](assets/java-home.JPG)

>[!NOTE]
> 请勿使用JDK 15。 AEM不支持它。

### 测试您的JDK版本

打开新的命令提示符窗口并键入： `java -version`. 您应该获取由标识的JDK版本 `JAVA_HOME` 变量

![数据源](assets/java-version.JPG)

## 安装Maven

Maven是一种构建自动化工具，主要用于Java项目。 请按照以下步骤在本地系统上安装maven。

* 创建名为的文件夹 `maven` 在C驱动器中
* 下载 [二进制zip存档](https://maven.apache.org/download.cgi)
* 将zip存档的内容提取到 `c:\maven`
* 创建一个名为的环境变量 `M2_HOME` 值为 `C:\maven\apache-maven-3.6.0`. 以我为例， **mvn** 版本为3.6.0。在撰写本文时，最新的maven版本是3.6.3
* 添加 `%M2_HOME%\bin` 到您的路径
* 保存更改
* 打开新的命令提示符并键入 `mvn -version`. 您应会看到 **mvn** 如下面的屏幕快照所示列出的版本

![数据源](assets/mvn-version.JPG)


## 安装Eclipse

安装最新版本的 [eclipse](https://www.eclipse.org/downloads/)

## 创建您的第一个项目

Archetype是一个Maven项目模板工具包。 原型被定义为原始阵列或模型，所有同类的其他事物都通过它来制造。 此名称适合我们尝试提供的系统，该系统提供生成Maven项目的一致方法。 Archetype可帮助作者为用户创建Maven项目模板，并为用户提供生成这些项目模板的参数化版本的方法。
要创建您的第一个maven项目，请执行以下步骤：

* 创建一个名为的新文件夹 `aemformsbundles` 在C驱动器中
* 打开命令提示符并导航至 `c:\aemformsbundles`
* 在命令提示符下运行以下命令

```java
mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate -D archetypeGroupId=com.adobe.aem -D archetypeArtifactId=aem-project-archetype -D archetypeVersion=36 -D appTitle="My Site" -D appId="mysite" -D groupId="com.mysite" -D aemVersion=6.5.13
```

成功完成后，您应在命令窗口中看到生成成功消息

## 从maven项目创建eclipse项目

* 将工作目录更改为 `mysite`
* 执行 `mvn eclipse:eclipse` 命令行。 该命令可读取您的pom文件并使用正确的元数据创建Eclipse项目，以便Eclipse了解项目类型、关系、类路径等。

## 将项目导入eclipse

Launch **Eclipse**

转到 **文件 — >导入** 并选择 **现有Maven项目** 如下所示

![数据源](assets/import-mvn-project.JPG)

单击“下一步”

通过单击 **浏览** 按钮

![数据源](assets/mysite-eclipse-project.png)

>[!NOTE]
>您可以根据需要选择导入相应的模块。 仅当要在项目中创建Java代码时，才选择并导入核心模块。

单击 **完成** 启动导入流程

项目已导入到Eclipse中，并且您会看到许多 `mysite.xxxx` 文件夹

展开 `src/main/java` 在 `mysite.core` 文件夹。 这是在其中编写大部分代码的文件夹。

![数据源](assets/mysite-core-project.png)

## 包含AEMFD客户端SDK

您需要在项目中包含AEMFD客户端SDK，以利用AEM Forms随附的各种服务。 请参考 [AEMFD客户端SDK](https://mvnrepository.com/artifact/com.adobe.aemfd/aemfd-client-sdk) 以在您的Maven项目中包含相应的客户端SDK。 您必须在的依赖关系部分包含AEM FD客户端SDK `pom.xml` ，如下所示。

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

要构建项目，请执行以下步骤：

* 打开 **命令提示符窗口**
* 导航到 `c:\aemformsbundles\mysite\core`
* 执行命令 `mvn clean install -PautoInstallBundle`
上述命令会在运行的AEM服务器中构建并安装捆绑包 `http://localhost:4502`. 该捆绑包还可在文件系统上获得，网址为
  `C:\AEMFormsBundles\mysite\core\target` 并且可以使用部署 [Felix Web控制台](http://localhost:4502/system/console/bundles)

## 后续步骤

[创建OSGi服务](./create-osgi-service.md)

