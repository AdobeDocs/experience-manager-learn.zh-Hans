---
title: 安装和配置Tomcat
seo-title: 安装和配置Tomcat
description: 这是创建您的第一个交互式通信文档的多步教程的第一部分。在本部分中，我们将安装TOMCAT并在TOMCAT中部署sampleRest.war文件。 此WAR文件公开的REST端点将作为数据源和表单数据模型的基础。
seo-description: 这是创建您的第一个交互式通信文档的多步教程的第一部分。在本部分中，我们将安装TOMCAT并在TOMCAT中部署sampleRest.war文件。 此WAR文件公开的REST端点将作为数据源和表单数据模型的基础。
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
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 1%

---


# 安装和配置Tomcat {#install-and-configure-tomcat}

在本部分中，我们将安装TOMCAT并在TOMCAT中部署sampleRest.war文件。 此WAR文件公开的REST端点将作为数据源和表单数据模型的基础。

>[!VIDEO](https://video.tv.adobe.com/v/37815/?quality=9&learn=on)

要设置tomcat，请按照以下说明操作：

1. 下载并安装JDK1.8。
2. 将JAVA_HOME设置为指向JDK1.8。
3. 下载[tomcat](https://tomcat.apache.org/)。 此war文件已使用Tomcat版本8.5.x和9.0.x进行测试。
4. 下载您的首选项的tomcat版本。 您可以下载核心部分下的64位windows zip。
5. 将内容解压缩到您的c:\tomcat文件夹。
6. 您应该在c驱动器&#x200B;**c:\tomcat\apache-tomcat-8.5.27**&#x200B;中看到类似的内容，具体取决于tomcat的版本
7. 创建一个名为“CATALINA_HOME”的环境变量，并将其值设置为tomcat安装文件夹示例c:\tomcat\apache- tomcat-8.5.27
8. 将SampleRest.war文件复制到Tomcat安装的webapps文件夹中
9. 开始新命令提示窗口。
10. 导航到&lt;tomcat install folder>\bin并运行startup.bat
11. Tomcat启动后，请通过单击此处](http://localhost:8080/SampleRest/webapi/getStatement/9586)测试WAR文件公开的端点[
12. 您应该通过此调用获得示例数据。

祝贺你！!!. 您已设置tomcat并部署了SampleRest.war文件。

以下视频介绍了示例应用程序在Tomcat中的部署
>[!VIDEO](https://video.tv.adobe.com/v/37815)