---
title: 安装和配置Tomcat
description: 这是创建您的第一个交互式通信文档的多步教程的第1部分。在本部分中，我们将安装TOMCAT并在TOMCAT中部署sampleRest.war文件。
feature: Interactive Communication
doc-type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
discoiquuid: 4f400c22-6c96-4018-851c-70d988ce7c6c
topic: Development
role: Developer
level: Beginner
exl-id: f0f19838-1ade-4eda-b736-a9703a3916c2
duration: 44
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 0%

---

# 安装和配置Tomcat {#install-and-configure-tomcat}

在本部分中，我们安装TOMCAT并在TOMCAT中部署sampleRest.war文件。 此WAR文件公开的REST端点是我们数据Source和表单数据模型的基础。

要设置tomcat，请按照以下说明操作：

1. 下载并安装JDK1.8。
2. 将JAVA_HOME设置为指向JDK1.8。
3. 下载[tomcat](https://tomcat.apache.org/)。 此war文件已在Tomcat版本8.5.x和9.0.x中进行测试。
4. 下载首选项的tomcat版本。 您可以在核心部分下下载64位windows zip。
5. 将内容解压缩到c：\tomcat。
6. 在c驱动器&#x200B;**c：\tomcat\apache-tomcat-8.5.27**&#x200B;中应该会看到类似这样的内容，具体取决于您的tomcat版本
7. 创建一个名为“CATALINA_HOME”的环境变量，并将其值设置为tomcat安装文件夹示例c：\tomcat\apache- tomcat-8.5.27
8. 将SampleRest.war文件复制到Tomcat安装的webapps文件夹中
9. 启动新的命令提示符窗口。
10. 导航到&lt;tomcat install folder>\bin并运行startup.bat
11. tomcat启动后，通过[单击此处](http://localhost:8080/SampleRest/webapi/getStatement/9586)来测试由WAR文件公开的端点
12. 您应会在此调用中获取样本数据。

恭喜!!!。 您已设置tomcat并部署SampleRest.war文件。

## 后续步骤

[配置RESTful数据源](./parttwo.md)
