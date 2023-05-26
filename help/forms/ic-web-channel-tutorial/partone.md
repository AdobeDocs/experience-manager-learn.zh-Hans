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
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 0%

---

# 安装和配置Tomcat {#install-and-configure-tomcat}

在本部分中，我们安装TOMCAT并将sampleRest.war文件部署到TOMCAT中。 此WAR文件公开的REST端点是我们数据源和表单数据模型的基础。

要设置tomcat，请按照以下说明操作：

1. 下载并安装JDK1.8。
2. 将JAVA_HOME设置为指向JDK1.8。
3. 下载 [tomcat](https://tomcat.apache.org/). 此war文件已通过Tomcat版本8.5.x和9.0.x测试。
4. 下载首选项的tomcat版本。 您可以在核心部分下下载64位windows zip。
5. 将内容解压缩到c：\tomcat。
6. 你应该在C驱动器中看到这样的东西 **c：\tomcat\apache-tomcat-8.5.27** 取决于您的tomcat的版本
7. 创建一个名为“CATALINA_HOME”的环境变量，并将其值设置为tomcat安装文件夹示例c：\tomcat\apache- tomcat-8.5.27
8. 将SampleRest.war文件复制到Tomcat安装的webapps文件夹中
9. 启动新的命令提示符窗口。
10. 导航到 &lt;tomcat install=&quot;&quot; folder=&quot;&quot;>\bin并运行startup.bat
11. tomcat启动后，通过以下方式测试由WAR文件公开的端点 [单击此处](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. 您应该获得作为此调用结果的示例数据。

恭喜!!!。 您已设置tomcat并部署SampleRest.war文件。

## 后续步骤

[配置RESTful数据源](./parttwo.md)
