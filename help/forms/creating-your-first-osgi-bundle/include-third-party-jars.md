---
title: 包含協力廠商jar
description: 瞭解如何在您的AEM專案中使用協力廠商jar檔案
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

# 在您的AEM專案中包含協力廠商套件組合

在本文章中，我們將逐步說明在您的AEM專案中包含第三方OSGi套件組合的相關步驟。為了撰寫本文檔，我們將包含 [jsch-0.1.55.jar](https://repo1.maven.org/maven2/com/jcraft/jsch/0.1.55/jsch-0.1.55.jar) 加入我們的AEM專案。  如果OSGi可在maven存放庫中取得，請在專案的POM.xml檔案中包含套件組合的相依性。

>[!NOTE]
> 假設第三方jar是OSGi套件

```java
<!-- https://mvnrepository.com/artifact/com.jcraft/jsch -->
<dependency>
    <groupId>com.jcraft</groupId>
    <artifactId>jsch</artifactId>
    <version>0.1.55</version>
</dependency>
```

如果您的OSGi套件組合位於您的檔案系統上，請建立名為的資料夾 **localjar** 在您的專案基底目錄(C:\aemformsbundles\AEMFormsProcessStep\localjar)下，相依性看起來會像這樣

```java
<dependency>
    <groupId>jsch</groupId>
    <artifactId>jsch</artifactId>
    <version>1.0</version>
    <scope>system</scope>
    <systemPath>${project.basedir}/localjar/jsch-0.1.55-bundle.jar</systemPath>
</dependency>
```

## 建立資料夾結構

我們正在將此套件組合新增至我們的AEM專案 **AEMFormsProcessStep** 位於 **c：\aemformsbundles** 資料夾

* 開啟 **filter.xml** 從專案的C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\META-INF\vault資料夾記下篩選元素的根屬性。

* 建立下列資料夾結構C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\jcr_root\apps\AEMFormsProcessStep-vendor-packages\application\install
* 此 **apps/AEMFormsProcessStep-vendor-packages** 是filter.xml中的根屬性值
* 更新專案POM.xml的相依性區段
* 開啟命令提示。 以我的案例瀏覽至專案的資料夾(c：\aemformsbundles\AEMFormsProcessStep)。 執行以下命令

```java
mvn clean install -PautoInstallSinglePackage
```

如果一切順利，套件會與協力廠商套件一起安裝在您的AEM執行個體中。 您可以使用來檢查束 [felix web主控台](http://localhost:4502/system/console/bundles). 協力廠商套件組合可在 `crx` 存放庫，如下所示
![協力廠商](assets/custom-bundle1.png)
