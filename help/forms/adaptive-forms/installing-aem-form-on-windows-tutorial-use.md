---
title: 在Windows上安装AEM Forms的简化步骤
seo-title: 在Windows上安装AEM Forms的简化步骤
description: 在Windows上安装AEM Forms的快速、简单步骤
seo-description: 在Windows上安装AEM Forms的快速、简单步骤
uuid: a148b8f0-83db-47f6-89d3-c8a9961be289
feature: Adaptive Forms
topics: administration
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 1182ef4d-5838-433b-991d-e24ab805ae0e
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 5%

---


# 在Windows上安装AEM Forms的简化步骤

>[!NOTE]
>
>如果您要使用AEM Forms，则不要多次单击AEM快速开始jar。
>
>另外，请确保AEM Forms安装文件夹路径中没有空格。
>
>例如，不要在c:\jack and jill\AEM Forms folder中安装AEM Forms

>[!NOTE]
>
>如果您正在安装AEM Forms 6.5，请确保您已安装以下32位Microsoft Visual C++可再发行组件。
>
>* Microsoft Visual C++ 2008可再分发
>* Microsoft Visual C++ 2010可再分发
>* Microsoft Visual C++ 2012可再分发
>* Microsoft Visual C++ 2013可再发行版（截至6.5版）


尽管我们建议遵循[正式文档](https://helpx.adobe.com/cn/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html)安装AEM Forms。 可以按照以下步骤在Windows环境上安装和配置AEM Forms:

* 确保已安装相应的JDK
   * AEM 6.2：您需要：Oracle SE 8 JDK 1.8.x（64位）
* 
   * AEM 6.3和AEM 6.4：您需要：Oracle SE 8 JDK 1.8.x（64位）
* AEM 6.5您需要JDK 8或JDK 11
* [此处列](https://helpx.adobe.com/experience-manager/6-3/sites/deploying/using/technical-requirements.html) 出官方JDK要求
* 确保将JAVA_HOME设置为指向已安装的JDK。
   * 要在窗口中创建JAVA_HOME变量，请执行以下步骤：
      * 右键单击“我的计算机”，然后选择“属性”
      * 在“高级”选项卡上，选择“环境变量”并创建一个名为JAVA_HOME的新系统变量。
      * 将变量值设置为指向安装在系统上的JDK。 例如c:\program files\java\jdk1.8.0_25

* 在您的C驱动器上创建一个名为AEMForm的文件夹
* 找到AEMQuickStart.Jar并将其移至AEMForms文件夹
* 将license.properties文件复制到此AEMForms文件夹中
* 使用以下内容创建一个名为“StartAemForms.bat”的批处理文件：
   * java -d64 -Xmx2048M -jar AEM_6.3_Quickstart.jar -gui。此处为AEM_6.3_Quickstart.jar是我的AEM快速启动jar的名称。
   * 您可以将jar重命名为任何名称，但请确保该名称反映在批处理文件中。将批处理文件保存在AEMForms文件夹中。

* 打开新的命令提示符，然后导航到c:\aemforms。

* 从命令提示符中执行StartAemForms.bat文件。

* 您应当看到一个小对话框，指示启动的进度。

* 启动完成后，打开sling.properties文件。 此位置位于c:\AEMForms\crx-quickstart\conf folder。

* 将以下2行复制到文件底部
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa。*** **sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider=org.bouncycastle.***
* 这两个属性是文档服务工作所必需的
* 保存sling.properties文件

* [登录到分享包](http://localhost:4502/crx/packageshare/login.html)

   * 您需要AdobeId登录包共享
   * 搜索适合您版本的AEM Forms和操作系统的AEM Forms Add包
   * 或者[您可以下载相应的表单，包](https://helpx.adobe.com/cn/aem-forms/kb/aem-forms-releases.html)
   * 安装包上的Add后，需要遵循以下步骤

      * **确保所有捆绑包都处于活动状态。（除AEMFD签名包外）。**
      * **通常需要5分钟或更长时间，所有包才能进入活动状态。**
   * **所有捆绑包都处于活动状态（AEMFD签名捆绑除外）后，请重新启动系统以完成AEM Forms安装**


* 将`sun.util.calendar`包添加到允许列表:

   1. 在您的[浏览器窗口](http://localhost:4502/system/console/configMgr)中打开Felix Web控制台
   2. 搜索并打开反序列化防火墙配置：`com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
   3. 将`sun.util.calendar`添加为`com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`下的新条目
   4. 保存更改。

祝贺你！!! 您现在已在系统上安装和配置AEM Forms。
根据您的需要，您可以在服务器上配置[Reader扩展](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html)或[ PDFG](https://helpx.adobe.com/experience-manager/6-3/forms/using/install-configure-pdf-generator.html)
