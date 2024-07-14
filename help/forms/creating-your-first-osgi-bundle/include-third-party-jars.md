---
title: 包括第三方jar
description: 了解如何在您的AEM项目中使用第三方jar文件
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
jira: KT-11245
last-substantial-update: 2022-10-15T00:00:00Z
thumbnail: third-party.jpg
exl-id: e8841c63-3159-4f13-89a1-d8592af514e3
duration: 53
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 0%

---

# 在您的AEM项目中包含第三方包

在本文中，我们将介绍在您的AEM项目中包含第三方OSGi包所涉及的步骤。出于本文的目的，我们将在AEM项目中包含该[jsch-0.1.55.jar](https://repo1.maven.org/maven2/com/jcraft/jsch/0.1.55/jsch-0.1.55.jar)。  如果OSGi在maven存储库中可用，请在项目的POM.xml文件中包含捆绑包的依赖项。

>[!NOTE]
> 假定第三方jar是OSGi捆绑包

```java
<!-- https://mvnrepository.com/artifact/com.jcraft/jsch -->
<dependency>
    <groupId>com.jcraft</groupId>
    <artifactId>jsch</artifactId>
    <version>0.1.55</version>
</dependency>
```

如果OSGi包位于您的文件系统上，请在项目的基目录(C:\aemformsbundles\AEMFormsProcessStep\localjar)下创建一个名为&#x200B;**localjar**&#x200B;的文件夹，则依赖关系将如下所示

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

我们正在将此捆绑包添加到位于&#x200B;**c：\aemformsbundles**&#x200B;文件夹中的AEM项目&#x200B;**AEMFormsProcessStep**

* 从项目的C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\META-INF\vault文件夹中打开&#x200B;**filter.xml**
记下过滤器元素的根属性。

* 创建以下文件夹结构C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\jcr_root\apps\AEMFormsProcessStep-vendor-packages\application\install
* **apps/AEMFormsProcessStep-vendor-packages**&#x200B;是filter.xml中的根属性值
* 更新项目POM.xml的依赖关系部分
* 打开命令提示符。 在本例中，请导航到项目的文件夹(c：\aemformsbundles\AEMFormsProcessStep)。 执行以下命令

```java
mvn clean install -PautoInstallSinglePackage
```

如果一切进展顺利，则会将软件包与第三方捆绑包一起安装到您的AEM实例中。 您可以使用[felix Web控制台](http://localhost:4502/system/console/bundles)检查捆绑包。 第三方包在`crx`存储库的/apps文件夹中可用，如下所示
![第三方](assets/custom-bundle1.png)
