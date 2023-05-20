---
title: 安裝及設定Tomcat視訊
seo-title: Install and Configure Tomcat
description: 這是建立第一個互動式通訊檔案的多步驟教學課程的第1部分。
uuid: 835e2342-82b6-4f0c-9a6b-467bbbd8527a
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
thumbnail: 37815.jpg
discoiquuid: 5f68be3d-aa35-4a3f-aaea-b8ee213c87ae
topic: Development
role: Developer
level: Beginner
exl-id: faa9ca2d-6cfa-4abf-be5e-3e549202853a
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '241'
ht-degree: 0%

---

# 安裝及設定Tomcat {#install-and-configure-tomcat}

在本部分中，我們會安裝TOMCAT並在TOMCAT中部署sampleRest.war檔案。 此WAR檔案公開的REST端點是我們資料來源和表單資料模型的基礎。

>[!VIDEO](https://video.tv.adobe.com/v/37815?quality=12&learn=on)

若要設定tomcat，請遵循下列指示：

1. 下載並安裝JDK1.8。
2. 將JAVA_HOME設定為指向JDK1.8。
3. 下載 [tomcat](https://tomcat.apache.org/). 此war檔案已通過Tomcat 8.5.x版和9.0.x版的測試。
4. 下載您偏好設定的tomcat版本。 您可以在核心區段底下下載64位元windows zip。
5. 將內容解壓縮至您的c：\tomcat。
6. 您應該會在C磁碟中看到類似這樣的內容 **c：\tomcat\apache-tomcat-8.5.27** 視您的tomcat版本而定
7. 建立名為「CATALINA_HOME」的環境變數，並將其值設定為tomcat安裝資料夾範例c：\tomcat\apache- tomcat-8.5.27
8. 將SampleRest.war檔案複製到Tomcat安裝的webapps資料夾
9. 啟動新的命令提示字元視窗。
10. 導覽至 &lt;tomcat install=&quot;&quot; folder=&quot;&quot;>\bin並執行startup.bat
11. 在您的tomcat啟動後，請以下列方式測試WAR檔案公開的端點 [按一下這裡](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. 您應該會因此呼叫而取得範例資料。

恭喜!!!。 您已設定tomcat並部署SampleRest.war檔案。

以下影片說明如何在Tomcat中部署範例應用程式
>[!VIDEO](https://video.tv.adobe.com/v/37815?quality=12&learn=on)

## 后续步骤

[建立RESTful資料來源](./create-data-source.md)