---
title: 在Windows上安装AEM Forms的简化步骤
seo-title: 在Windows上安装AEM Forms的简化步骤
description: 在Windows上安装AEM Forms快速、轻松的步骤
seo-description: 在Windows上安装AEM Forms快速、轻松的步骤
uuid: a148b8f0-83db-47f6-89d3-c8a9961be289
feature: adaptive-forms
topics: administration
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 1182ef4d-5838-433b-991d-e24ab805ae0e
translation-type: tm+mt
source-git-commit: 82127d5be9a4b969537738f9ba537efe07f38479
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 2%

---

# 在Windows上安装AEM Forms的简化步骤

>[!NOTE]
>如果您要使用AEM Forms，则不要多次单击AEM快速开始jar。
>另外，确保“AEM Forms安装”文件夹路径中没有空格。
>例如，不要在c:\jack and jill\AEM Forms folder中安装AEM Forms

>[!NOTE]
如果您正在安装AEM Forms6.5，请确保您已安装以下32位Microsoft Visual C++可再发行组件。

* Microsoft Visual C++ 2008可再分发
* Microsoft Visual C++ 2010可再分发
* Microsoft Visual C++ 2012可再分发
* Microsoft Visual C++ 2013可再分发（截止到6.5）

尽管我们建议遵循安 [装AEM Forms](https://helpx.adobe.com/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html) 的官方文档。 可以按照以下步骤在Windows环境上安装和配置AEM Forms:

* 确保已安装相应的JDK
   * AEM 6.2，您需要：Oracle SE 8 JDK 1.8.x（64位）
* 
   * AEM 6.3和AEM 6.4，您需要：Oracle SE 8 JDK 1.8.x（64位）
* AEM 6.5您需要JDK 8或JDK 11
* [此处列出官方的](https://helpx.adobe.com/experience-manager/6-3/sites/deploying/using/technical-requirements.html) JDK要求
* 确保将JAVA_HOME设置为指向您已安装的JDK。
   * 要在窗口中创建JAVA_HOME变量，请执行以下步骤：
      * 右键单击“我的计算机”并选择“属性”
      * 在“高级”选项卡上，选择“环境变量”并创建一个名为JAVA_HOME的新系统变量。
      * 将变量值设置为指向安装在系统上的JDK。 例如c:\program files\java\jdk1.8.0_25

* 在C驱动器上创建一个名为AEMForm的文件夹
* 找到AEMQuickStart.Jar并将其移入AEMForms文件夹
* 将license.properties文件复制到此AEMForms文件夹
* 使用以下内容创建一个名为“StartAemForms.bat”的批处理文件：
   * java -d64 -Xmx2048M -jar AEM_6.3_Quickstart.jar -gui。此处AEM_6.3_Quickstart.jar是我的AEM快速启动jar的名称。
   * 您可以将jar重命名为任何名称，但请确保该名称反映在批处理文件中。将批处理文件保存在AEMForms文件夹中。

* 打开新的命令提示符，然后导航到c:\aemforms。

* 从命令提示符中执行StartAemForms.bat文件。

* 您应当看到一个指示启动进度的小对话框。

* 启动完成后，打开sling.properties文件。 此位置位于c:\AEMForms\crx-quickstart\conf folder。

* 将以下2行复制到文件底部
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa。*** sling.bootdelegation.class.org.bo **uncycastle.jce.provider.BouncyCastleProvider=org.bouncycastle.***
* 这两个属性是文档服务工作所必需的
* 保存sling.properties文件

* [登录到分享包](http://localhost:4502/crx/packageshare/login.html)

   * 您需要AdobeId登录包共享
   * 搜索适合您版本的AEM Forms和操作系统的AEM FormsAdd软件包
   * 或 [者，您可以下载相应的表单Addon包](https://helpx.adobe.com/cn/aem-forms/kb/aem-forms-releases.html)
   * 在安装了此加载项包后，需要遵循以下步骤

      * **确保所有捆绑包都处于活动状态。 （AEMFD签名包除外）。**
      * **通常需要5分钟或更长时间，所有包才能进入活动状态。**
   * **所有捆绑包都处于活动状态（AEMFD签名捆绑除外）后，请重新启动系统以完成AEM Forms安装**


* 将 `sun.util.calendar` 包添加到允许列表:

   1. 在浏览器窗口中打开Felix [Web控制台](http://localhost:4502/system/console/configMgr)
   2. 搜索并打开反序列化防火墙配置： `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
   3. 添加 `sun.util.calendar` 为新条目，位于 `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`
   4. 保存更改。

祝贺你！!! 您现在已在系统上安装并配置了AEM Forms。
根据您的需要，您可以在服 [务器上配置](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html)[ Reader扩展](https://helpx.adobe.com/experience-manager/6-3/forms/using/install-configure-pdf-generator.html) 或PDFG。
