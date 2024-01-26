---
title: 在Windows上安装AEM Forms的简化步骤
description: 在Windows上安装AEM Forms的快速轻松步骤
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Admin
level: Beginner
exl-id: 80288765-0b51-44a9-95d3-3bdb2da38615
last-substantial-update: 2020-06-09T00:00:00Z
duration: 149
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 1%

---

# 在Windows上安装AEM Forms的简化步骤

>[!NOTE]
>
>如果要使用AEM Forms，则切勿双击AEM快速入门jar。
>
>此外，请确保AEM Forms安装文件夹路径中没有空格。
>
>例如，不要在c：\jack和jill\AEM Forms文件夹中安装AEM Forms

>[!NOTE]
>
>如果要安装AEM Forms 6.5，请确保已安装了以下32位Microsoft Visual C++可再发行组件。
>
>* Microsoft Visual C++ 2008可再分发
>* Microsoft Visual C++ 2010可再分发
>* Microsoft Visual C++ 2012可再分发
>* Microsoft Visual C++ 2013可再分发（截止到6.5版）

虽然我们建议遵循 [官方文档](https://helpx.adobe.com/cn/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html) 安装AEM Forms。 可以执行以下步骤，在Windows环境中安装和配置AEM Forms：

* 确保安装了适当的JDK
   * 您需要AEM 6.2：OracleSE 8 JDK 1.8.x（64位）
   * 您需要AEM 6.3和AEM 6.4：OracleSE 8 JDK 1.8.x（64位）
   * AEM 6.5需要JDK 8或JDK 11
   * [JDK的官方要求](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/introduction/technical-requirements.html?lang=zh-Hans) 此处列出
* 确保将JAVA_HOME设置为指向您已安装的JDK。
   * 要在Windows中创建JAVA_HOME变量，请执行以下步骤：
      * 右键单击“我的电脑”并选择“属性”
      * 在高级选项卡上，选择环境变量，然后创建一个名为JAVA_HOME的新系统变量。
      * 将变量值设置为指向系统上安装的JDK。 例如c：\program files\java\jdk1.8.0_25

* 在C驱动器上创建名为AEMForms的文件夹
* 找到AEMQuickStart.Jar并将其移动到AEMForms文件夹中
* 将license.properties文件复制到此AEMForms文件夹中
* 创建一个名为“StartAemForms.bat”的批处理文件，该文件包含以下内容：
   * `java -d64 -Xmx2048M -jar AEM_6.5_Quickstart.jar -gui`
      * 此处，AEM_6.5_Quickstart.jar是我的AEM快速入门jar的名称。
   * 您可以将jar重命名为任何名称，但请确保该名称反映在批处理文件中。 将批处理文件保存在AEMForms文件夹中。

* 打开新的命令提示符，然后导航到 _c：\aemforms_.

* 从命令提示符执行StartAemForms.bat文件。

* 您应该会看到一个指示启动进度的小对话框。

* 启动完成后，打开sling.properties文件。 它位于c：\AEMForms\crx-quickstart\conf文件夹中。

* 将以下2行复制到文件底部
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.&#42;** **sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider=org.bouncycastle.&#42;**
* 文档服务需要这两个属性才能正常工作
* 保存sling.properties文件
* [下载相应的表单附加组件包](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=en)
* 使用以下方式安装表单加载项包 [包管理器](http://localhost:4502/crx/packmgr/index.jsp).
* 安装添加软件包后，需要执行以下步骤

   * **确保所有捆绑包都处于活动状态。 （AEMFD签名包除外）。**
   * **所有捆绑包通常需要5分钟或更长时间才能进入活动状态。**

   * **所有捆绑包均处于活动状态（AEMFD签名捆绑包除外）后，请重新启动系统以完成AEM Forms安装**

## sun.util.calendar允许列表包

1. 在中打开Felix web控制台 [浏览器窗口](http://localhost:4502/system/console/configMgr)
1. 搜索并打开反序列化防火墙配置： `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
1. 添加 `sun.util.calendar` 作为新条目，位于 `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`
1. 保存更改。

恭喜!!! 您现在已在系统上安装和配置AEM Forms。
根据您的需求，您可以配置  [Reader扩展](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html) 或 [PDFG](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/install-configure-document-services.html) 在您的服务器上
