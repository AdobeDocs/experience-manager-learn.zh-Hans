---
title: 包括第三方jar
description: 了解如何在AEM项目中使用第三方jar文件
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
kt: 11245
last-substantial-update: 2022-10-15T00:00:00Z
thumbnail: third-party.jpg
exl-id: e8841c63-3159-4f13-89a1-d8592af514e3
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 0%

---

# 在您的AEM项目中包含第三方包

在本文中，我们将介绍在您的AEM项目中包含第三方OSGi捆绑包所涉及的步骤。出于本文的目的，我们将包含 [jsch-0.1.55.jar](https://repo1.maven.org/maven2/com/jcraft/jsch/0.1.55/jsch-0.1.55.jar) 在我们的AEM项目中。  如果OSGi在maven存储库中可用，请在项目的POM.xml文件中包含捆绑包的依赖项。

>[!NOTE]
> 假定第三方jar是OSGi包

```java
<!-- https://mvnrepository.com/artifact/com.jcraft/jsch -->
<dependency>
    <groupId>com.jcraft</groupId>
    <artifactId>jsch</artifactId>
    <version>0.1.55</version>
</dependency>
```

如果您的OSGi捆绑包位于您的文件系统上，请创建一个名为的文件夹 **localjar** 在项目的基目录(C:\aemformsbundles\AEMFormsProcessStep\localjar)下，依赖关系如下所示

```java
<dependency>
    <groupId>jsch</groupId>
    <artifactId>jsch</artifactId>
    <version>1.0</version>
    <scope>system</scope>
    <systemPath>${project.basedir}/localjar/jsch-0.1.55-bundle.jar</systemPath>
</dependency>
```

## 创建文件夹结构

我们正在将此捆绑包添加到我们的AEM项目 **AEMFormsProcessStep** 居住于 **c：\aemformsbundles** 文件夹

* 打开 **filter.xml** 从项目的C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\META-INF\vault文件夹中，记下过滤器元素的根属性。

* 创建以下文件夹结构C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\jcr_root\apps\AEMFormsProcessStep-vendor-packages\application\install
* 此 **apps/AEMFormsProcessStep-vendor-packages** 是filter.xml中的根属性值
* 更新项目POM.xml的依赖关系部分
* 打开命令提示符。 在我的示例中，导航到项目的文件夹(c：\aemformsbundles\AEMFormsProcessStep)。 执行以下命令

```java
mvn clean install -PautoInstallSinglePackage
```

如果一切顺利，则会将软件包与第三方捆绑包一起安装到您的AEM实例中。 可使用检查捆绑包 [felix web控制台](http://localhost:4502/system/console/bundles). 第三方捆绑包位于 `crx` 存储库，如下所示
![第三方](assets/custom-bundle1.png)
