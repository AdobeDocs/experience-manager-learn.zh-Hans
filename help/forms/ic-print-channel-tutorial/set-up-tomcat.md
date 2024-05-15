---
title: 安装和配置Tomcat视频
description: 这是创建您的第一个交互式通信文档的多步教程的第1部分。
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
thumbnail: 37815.jpg
discoiquuid: 5f68be3d-aa35-4a3f-aaea-b8ee213c87ae
topic: Development
role: Developer
level: Beginner
exl-id: faa9ca2d-6cfa-4abf-be5e-3e549202853a
duration: 237
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 0%

---

# 安装和配置Tomcat {#install-and-configure-tomcat}

在本部分中，我们安装TOMCAT并在TOMCAT中部署sampleRest.war文件。 此WAR文件公开的REST端点是我们数据源和表单数据模型的基础。

>[!VIDEO](https://video.tv.adobe.com/v/37815?quality=12&learn=on)

要设置tomcat，请按照以下说明操作：

1. 下载并安装JDK1.8。
2. 将JAVA_HOME设置为指向JDK1.8。
3. 下载 [tomcat](https://tomcat.apache.org/). 此war文件已在Tomcat版本8.5.x和9.0.x中进行测试。
4. 下载首选项的tomcat版本。 您可以在核心部分下下载64位windows zip。
5. 将内容解压缩到c：\tomcat。
6. 您应该会在C驱动器中看到类似内容 **c：\tomcat\apache-tomcat-8.5.27** 取决于您的tomcat的版本
7. 创建一个名为“CATALINA_HOME”的环境变量，并将其值设置为tomcat安装文件夹示例c：\tomcat\apache- tomcat-8.5.27
8. 将SampleRest.war文件复制到Tomcat安装的webapps文件夹中
9. 启动新的命令提示符窗口。
10. 导航到 &lt;tomcat install=&quot;&quot; folder=&quot;&quot;>\bin并运行startup.bat
11. tomcat启动后，测试由WAR文件公开的端点 [单击此处](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. 您应会在此调用中获取样本数据。

恭喜!!!。 您已设置tomcat并部署SampleRest.war文件。

以下视频介绍如何在Tomcat中部署示例应用程序
>[!VIDEO](https://video.tv.adobe.com/v/37815?quality=12&learn=on)

## 后续步骤

[创建RESTful数据源](./create-data-source.md)