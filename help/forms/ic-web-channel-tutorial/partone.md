---
title: 安装和配置Tomcat
seo-title: Install and Configure Tomcat
description: 这是创建您的第一个交互式通信文档的多步教程的第1部分。在本部分中，我们将安装TOMCAT并在TOMCAT中部署sampleRest.war文件。
uuid: c6d4c74c-ea16-4c63-92c9-182d087fd88c
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 4f400c22-6c96-4018-851c-70d988ce7c6c
topic: Development
role: Developer
level: Beginner
exl-id: f0f19838-1ade-4eda-b736-a9703a3916c2
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '236'
ht-degree: 0%

---

# 安装和配置Tomcat {#install-and-configure-tomcat}

在本部分中，我们安装了TOMCAT，并在TOMCAT中部署了sampleRest.war文件。 此WAR文件公开的REST端点是我们的数据源和表单数据模型的基础。

要设置tomcat，请按照以下说明操作：

1. 下载并安装JDK1.8。
2. 将JAVA_HOME设置为指向JDK1.8。
3. 下载 [tomcat](https://tomcat.apache.org/). 已使用Tomcat版本8.5.x和9.0.x对此War文件进行测试。
4. 下载首选项的tomcat版本。 您可以下载核心部分下方的64位windows zip文件。
5. 将内容解压缩到c:\tomcat文件夹。
6. 您应该在c驱动器中看到类似的内容 **c:\tomcat\apache-tomcat-8.5.27** 根据tomcat的版本
7. 创建一个名为“CATALINA_HOME”的环境变量，并将其值设置为tomcat安装文件夹示例c:\tomcat\apache- tomcat-8.5.27
8. 将SampleRest.war文件复制到Tomcat安装的Webapps文件夹中
9. 启动新的命令提示窗口。
10. 导航到 &lt;tomcat install=&quot;&quot; folder=&quot;&quot;>\bin并运行startup.bat
11. Tomcat启动后，测试由WAR File公开的端点(由 [单击此处](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. 您应该会获得此调用的示例数据。

恭喜!!!。 您已设置tomcat并部署了SampleRest.war文件。
