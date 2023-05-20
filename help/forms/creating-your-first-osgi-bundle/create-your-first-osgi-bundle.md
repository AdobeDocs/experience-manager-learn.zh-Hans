---
title: 使用AEM Forms建立您的第一個OSGi套件組合
description: 使用Maven和Eclipse建立您的第一個OSGi套件組合
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
exl-id: 307cc3b2-87e5-4429-8f21-5266cf03b78f
last-substantial-update: 2021-04-23T00:00:00Z
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '672'
ht-degree: 1%

---

# 建立您的第一個OSGi套件

OSGi套件組合是一個Java™封存檔案，其中包含Java程式碼、資源以及說明套件組合及其相依性的資訊清單。 此套件組合是應用程式的部署單位。 本文章適用於想使用AEM Forms 6.4或6.5建立OSGi服務或servlet的開發人員。若要建置您的第一個OSGi套件，請遵循下列步驟：


## 安裝JDK

安裝支援的JDK版本。 我已使用JDK1.8。確定您已新增 **JAVA_HOME** ，且指向JDK安裝的根資料夾。
將%JAVA_HOME%/bin新增至路徑

![data-source](assets/java-home.JPG)

>[!NOTE]
> 請勿使用JDK 15。 AEM不支援。

### 測試您的JDK版本

開啟新的命令提示字元視窗並輸入： `java -version`. 您應該取回由所識別的JDK版本 `JAVA_HOME` 變數

![data-source](assets/java-version.JPG)

## 安裝Maven

Maven是主要用於Java專案的組建自動化工具。 請依照下列步驟在本機系統上安裝maven。

* 建立名為的資料夾 `maven` 在您的C磁碟機中
* 下載 [二進位zip封存](https://maven.apache.org/download.cgi)
* 將zip封存的內容解壓縮至 `c:\maven`
* 建立名為的環境變數 `M2_HOME` 具有值 `C:\maven\apache-maven-3.6.0`. 以我為例， **mvn** 版本是3.6.0。在撰寫本文時，最新的maven版本為3.6.3
* 新增 `%M2_HOME%\bin` 至您的路徑
* 儲存您的變更
* 開啟新的命令提示字元並輸入 `mvn -version`. 您應該會看到 **mvn** 下列熒幕擷圖所示的版本

![data-source](assets/mvn-version.JPG)


## 安裝Eclipse

安裝最新版本的 [eclipse](https://www.eclipse.org/downloads/)

## 建立您的第一個專案

Archetype是Maven專案範本工具組。 原型被定義為原始陣列或模型，其他所有同類物件都來自此原始陣列或模型。 此名稱適合我們嘗試提供的系統，提供產生Maven專案的一致方法。 Archetype可幫助作者為使用者建立Maven專案範本，並提供使用者產生這些專案範本引數化版本的方法。
若要建立您的第一個maven專案，請遵循以下步驟：

* 建立名為的新資料夾 `aemformsbundles` 在您的C磁碟機中
* 開啟命令提示字元並瀏覽至 `c:\aemformsbundles`
* 在命令提示字元中執行以下命令

```java
mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate -D archetypeGroupId=com.adobe.aem -D archetypeArtifactId=aem-project-archetype -D archetypeVersion=36 -D appTitle="My Site" -D appId="mysite" -D groupId="com.mysite" -D aemVersion=6.5.13
```

成功完成後，您應該會在命令視窗中看到建置成功訊息

## 從您的maven專案建立eclipse專案

* 將工作目錄變更為 `mysite`
* 執行 `mvn eclipse:eclipse` 命令列中的。 該命令會讀取您的pom檔案，並使用正確的中繼資料建立Eclipse專案，以便Eclipse瞭解專案型別、關係、類別路徑等。

## 將專案匯入eclipse

Launch **Eclipse**

前往 **檔案 — >匯入** 並選取 **現有Maven專案** 如下所示

![data-source](assets/import-mvn-project.JPG)

按「下一步」

按一下「 」，選取c：\aemformsbundles\mysite **瀏覽** 按鈕

![data-source](assets/mysite-eclipse-project.png)

>[!NOTE]
>您可以視需要選擇匯入適當的模組。 如果您只打算在專案中建立Java程式碼，請僅選取並匯入核心模組。

按一下 **完成** 以開始匯入程式

專案已匯入至Eclipse，您會看到許多 `mysite.xxxx` 資料夾

展開 `src/main/java` 在 `mysite.core` 資料夾。 這是您撰寫大部分程式碼的資料夾。

![data-source](assets/mysite-core-project.png)

## 包含AEMFD使用者端SDK

您必須在專案中加入AEMFD使用者端SDK，才能運用AEM Forms隨附的各種服務。 請參考 [AEMFD使用者端SDK](https://mvnrepository.com/artifact/com.adobe.aemfd/aemfd-client-sdk) 以在您的Maven專案中包含適當的使用者端SDK。 您必須在的相依性區段中包含AEM FD使用者端SDK `pom.xml` ，如下所示。

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

若要建置您的專案，請遵循下列步驟：

* 開啟 **命令提示視窗**
* 导航至 `c:\aemformsbundles\mysite\core`
* 執行命令 `mvn clean install -PautoInstallBundle`
上述命令會在執行的AEM伺服器上建置並安裝套件 `http://localhost:4502`. 檔案系統上也提供該套件，位於
   `C:\AEMFormsBundles\mysite\core\target` 並且可以部署使用 [Felix Web主控台](http://localhost:4502/system/console/bundles)

## 后续步骤

[建立OSGi服務](./create-osgi-service.md)

