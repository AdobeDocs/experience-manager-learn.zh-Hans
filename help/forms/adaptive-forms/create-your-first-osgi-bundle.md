---
title: 使用AEM表单创建您的第一个OSGi捆绑包
description: 使用maven和eclipse构建您的第一个OSGi捆绑包
feature: 自适应表单
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
topic: 开发
role: 开发人员
level: 初学者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '835'
ht-degree: 2%

---


# 创建您的第一个OSGi捆绑包

OSGi捆绑包是一个Java™归档文件，包含Java代码、资源以及描述捆绑包及其依赖关系的清单。 捆绑包是应用程序的部署单元。 本文面向希望使用AEM Forms 6.4或6.5创建OSGi服务或servlet的开发人员。要构建您的第一个OSGi捆绑包，请遵循以下步骤：


## 安装JDK

安装支持的JDK版本。 我已使用JDK1.8。请确保已在环境变量中添加&#x200B;**JAVA_HOME**，并指向JDK安装的根文件夹。
将%JAVA_HOME%/bin添加到路径

![数据源](assets/java-home.JPG)

>[!NOTE]
> 请勿使用JDK 15。 AEM不支持它。

### 测试JDK版本

打开新的命令提示符窗口并键入：`java -version`。 应返回由`JAVA_HOME`变量标识的JDK版本

![数据源](assets/java-version.JPG)

## 安装Maven

Maven是一个主要用于Java项目的构建自动化工具。 请按照以下步骤在本地系统上安装maven。

* 在C驱动器中创建一个名为`maven`的文件夹
* 下载[二进制zip归档文件](http://maven.apache.org/download.cgi)
* 将zip存档的内容解压到`c:\maven`中
* 创建一个名为`M2_HOME`的环境变量，其值为`C:\maven\apache-maven-3.6.0`。 在我的例子中，**mvn**&#x200B;版本为3.6.0。在编写本文时，最新的maven版本为3.6.3
* 将`%M2_HOME%\bin`添加到路径
* 保存更改
* 打开新的命令提示符并键入`mvn -version`。 您应当看到&#x200B;**mvn**&#x200B;版本列出，如下屏幕截图所示

![数据源](assets/mvn-version.JPG)

## Settings.xml

Maven `settings.xml`文件定义以各种方式配置Maven执行的值。 通常，它用于定义本地存储库位置、备用远程存储库服务器以及专用存储库的身份验证信息。

导航到`C:\Users\<username>\.m2 folder`
解压[settings.zip](assets/settings.zip)文件的内容，并将其放在`.m2`文件夹中。

## 安装Eclipse

安装[eclipse](https://www.eclipse.org/downloads/)的最新版本

## 创建您的第一个项目

Archetype是Maven项目的模板工具包。 原型被定义为原始图案或模型，从中可以制造所有同类物品。 我们正在尝试提供一个系统，以提供一致的方法生成Maven项目，因此该名称是合适的。 Archetype将帮助作者为用户创建Maven项目模板，并为用户提供生成这些项目模板的参数化版本的方法。
要创建您的第一个主要项目，请执行以下步骤：

* 在C驱动器中新建一个名为`aemformsbundles`的文件夹
* 打开命令提示符并导航到`c:\aemformsbundles`
* 在命令提示符下运行以下命令
* `mvn archetype:generate  -DarchetypeGroupId=com.adobe.granite.archetypes  -DarchetypeArtifactId=aem-project-archetype -DarchetypeVersion=19`

此时将以交互方式生成主项目，并会要求您为许多属性(如

| 属性名称 | 意义 | 值 |
------------------------|---------------------------------------|---------------------
| groupId | groupId可在所有项目中唯一标识您的项目 | com.learningaemforms.adobe |
| appsFolderName | 将保存您的项目结构的文件夹的名称 | 学习Aemforms |
| artifactId | artifactId是没有版本的jar的名称。 如果您创建了它，则可以选择任何您想要的名称，并使用小写字母，而不使用奇怪的符号。 | 学习Aemforms |
| 版本 | 如果您分发它，则可以选择任何带数字和点(1.0、1.1、1.0.1、...)的典型版本。 | 1.0 |

通过按Enter键接受其他属性的默认值。
如果一切顺利，您应在命令窗口中看到生成成功消息

## 从您的主项目创建Eclipse项目

将工作目录更改为`learningaemforms`。
从命令行执行`mvn eclipse:eclipse`
上面的命令读取您的pom文件并创建包含正确元数据的Eclipse项目，以便Eclipse了解项目类型、关系、类路径等。

## 将项目导入Eclipse

启动&#x200B;**Eclipse**

转至&#x200B;**文件 — >导入**，然后选择&#x200B;**现有的Maven Projects**，如下所示

![数据源](assets/import-mvn-project.JPG)

单击“下一步”

单击&#x200B;**浏览**&#x200B;按钮选择`c:\aemformsbundles\learningaemform`

![数据源](assets/select-mvn-project.JPG)

>[!NOTE]
>您可以根据需要选择导入相应的模块。 仅当您要在项目中创建Java代码时，请选择并导入核心模块。

单击&#x200B;**完成**&#x200B;以开始导入过程

项目已导入到Eclipse中，您将看到许多`learningaemforms.xxxx`文件夹

展开`learningaemforms.core`文件夹下的`src/main/java`。 这是您将在其中编写大多数代码的文件夹。

![数据源](assets/learning-core.JPG)

## 构建项目

编写OSGi服务或servlet后，您需要构建项目以生成可使用Felix Web控制台部署的OSGi捆绑包。 请参阅[AEMFD客户端SDK](https://repo.adobe.com/nexus/content/repositories/public/com/adobe/aemfd/aemfd-client-sdk/)以在Maven项目中包含相应的客户端SDK。 您必须在核心项目`pom.xml`的“依赖项”部分包含AEM FD Client SDK，如下所示。

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

要构建您的项目，请执行以下步骤：

* 打开&#x200B;**命令提示窗口**
* 导航至 `c:\aemformsbundles\learningaemforms\core`
* 执行命令`mvn clean install`
如果一切顺利，您应在以下位置`C:\AEMFormsBundles\learningaemforms\core\target`看到捆绑包。 此捆绑包现已准备好使用Felix Web控制台部署到AEM中。
