---
title: 在Windows上安装AEM Forms的简化步骤
description: 在Windows上快速、轻松地安装AEM Forms
feature: 自适应表单
type: Tutorial
version: 6.4,6.5
topic: 开发
role: Admin
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 5%

---


# 在Windows上安装AEM Forms的简化步骤

>[!NOTE]
>
>如果您打算使用AEM，请勿双击“AEM Forms快速入门”Jar 。
>
>此外，请确保AEM Forms安装文件夹路径中没有空格。
>
>例如，请勿在c:\jack and jill\AEM Forms folder中安装AEM Forms

>[!NOTE]
>
>如果您正在安装AEM Forms 6.5，请确保已安装以下32位Microsoft Visual C++可再发行版。
>
>* Microsoft Visual C++ 2008可再发行版
>* Microsoft Visual C++ 2010可再发行版
>* Microsoft Visual C++ 2012可再发行版
>* Microsoft Visual C++ 2013可再发行版（自6.5起）


尽管我们建议遵循[正式文档](https://helpx.adobe.com/cn/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html)来安装AEM Forms。 可以按照以下步骤在Windows环境中安装和配置AEM Forms:

* 确保安装了相应的JDK
   * AEM 6.2，您需要：OracleSE 8 JDK 1.8.x（64位）
* 
   * AEM 6.3和AEM 6.4中，您需要：OracleSE 8 JDK 1.8.x（64位）
* AEM 6.5您需要JDK 8或JDK 11
* [此处列](https://helpx.adobe.com/experience-manager/6-3/sites/deploying/using/technical-requirements.html) 出了官方JDK要求
* 确保将JAVA_HOME设置为指向您安装的JDK。
   * 要在窗口中创建JAVA_HOME变量，请执行以下步骤：
      * 右键单击“My Computer（我的计算机）” ，然后选择“Properties（属性）”
      * 在高级选项卡上，选择环境变量并创建一个名为JAVA_HOME的新系统变量。
      * 将变量值设置为指向系统上安装的JDK。 例如c:\program files\java\jdk1.8.0_25

* 在C驱动器上创建一个名为AEMForms的文件夹
* 找到AEMQuickStart.Jar并将其移入AEMForms文件夹
* 将license.properties文件复制到此AEMForms文件夹中
* 创建名为“StartAemForms.bat”的批处理文件，其中包含以下内容：
   * java -d64 -Xmx2048M -jar AEM_6.3_Quickstart.jar -gui。此处为AEM_6.3_Quickstart.jar是我的AEM快速入门Jar的名称。
   * 您可以将jar重命名为任意名称，但请确保该名称反映在批处理文件中。请将批处理文件保存在AEMForms文件夹中。

* 打开新的命令提示符，然后导航到c:\aemforms。

* 从命令提示符执行StartAemForms.bat文件。

* 您应该会看到一个小对话框，指示启动的进度。

* 启动完成后，打开sling.properties文件。 此地址位于c:\AEMForms\crx-quickstart\conf folder。

* 将以下2行复制到文件底部
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa。*** **sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider=org.bouncycastle.***
* 文档服务需要这两个属性才能正常工作
* 保存sling.properties文件

* [登录到分享包](http://localhost:4502/crx/packageshare/login.html)

   * 您需要AdobeId才能登录到包共享
   * 搜索适用于您的AEM Forms和操作系统版本的AEM Forms Add包
   * 或者[您可以下载相应的表单附加包](https://helpx.adobe.com/cn/aem-forms/kb/aem-forms-releases.html)
   * 安装附加组件包后，需要执行以下步骤

      * **确保所有包都处于活动状态。（除AEMFD签名包外）。**
      * **所有包通常需要5分钟或更长时间才能进入活动状态。**
   * **所有包均处于活动状态（AEMFD签名包除外）后，请重新启动系统以完成AEM Forms安装**


* 将`sun.util.calendar`包添加到允许列表:

   1. 在[浏览器窗口](http://localhost:4502/system/console/configMgr)中打开Felix Web控制台
   2. 搜索并打开反序列化防火墙配置：`com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
   3. 在`com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`下添加`sun.util.calendar`作为新条目
   4. 保存更改。

恭喜你!!! 您现在已在系统上安装并配置AEM Forms。
根据您的需要，可以在服务器上配置[Reader扩展](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html)或[ PDFG](https://helpx.adobe.com/experience-manager/6-3/forms/using/install-configure-pdf-generator.html)
