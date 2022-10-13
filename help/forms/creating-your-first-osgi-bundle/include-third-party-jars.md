---
title: 与AEM Forms创建您的第一个OSGi包
description: 使用Maven和Eclipse构建您的第一个OSGi包
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
kt: kt-11245
source-git-commit: 061077fb6cd8ac7b760aa30b884ced6d4d3c3b20
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 0%

---

# 在您的AEM项目中包含第三方包

在本文中，我们将介绍在您的AEM项目中包含第三方OSGi包所涉及的步骤。为了本文的目的，我们将包含 [jsch-0.1.55.jar](https://repo1.maven.org/maven2/com/jcraft/jsch/0.1.55/jsch-0.1.55.jar) 在我们的AEM项目中。  如果OSGi在Maven存储库包含包的依赖项中可用，则在项目的POM.xml文件中。

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

如果OSGi包位于文件系统上，则依赖项将如下所示

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

我们将此包添加到我们的AEM项目 **AEMFormsProcessStep** 居住在 **c:\aemformsbundles** 文件夹

* 打开 **filter.xml** 从C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\META-INF\vault folder of your project Make a note of the root attribute of the filter element。

* 创建以下文件夹结构C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\jcr_root\apps\AEMFormsProcessStep-vendor-packages\application\install
* 的 **apps/AEMFormsProcessStep供应商包** 是filter.xml中的根属性值
* 更新项目POM.xml的依赖项部分
* 打开命令提示符。 在我的示例中，导航到您项目的文件夹(c:\aemformsbundles\AEMFormsProcessStep)。 执行以下命令

```java
mvn clean install -pAutoInstallSinglePackage
```

如果一切正常，则包将与第三方包一起安装到您的AEM实例中。 可以使用 [felix web console](http://localhost:4502/system/console/bundles). 第三方包位于的/apps文件夹中 `crx` 存储库如下所示
![第三方](assets/custom-bundle1.png)
![第三方](assets/custom-bundle1.png)


