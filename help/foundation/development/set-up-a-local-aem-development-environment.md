---
title: 设置本地AEM开发环境
description: 了解如何设置本地开发环境以进行Experience Manager。 熟悉本地安装、Apache Maven、集成开发环境，以及调试和疑难解答。 使用Eclipse IDE、CRXDE-Lite、Visual Studio代码和IntelliJ。
version: 6.4, 6.5
feature: Developer Tools
topics: development
activity: develop
audience: developer
topic: Development
role: Developer
level: Beginner
exl-id: 58851624-71c9-4745-aaaf-305acf6ccb14
last-substantial-update: 2022-07-20T00:00:00Z
thumbnail: aem-local-dev-env.jpg
source-git-commit: 2b188cbe0ba968b553a20629b89edf5ed377f300
workflow-type: tm+mt
source-wordcount: '2603'
ht-degree: 1%

---

# 设置本地AEM开发环境

为AEM的Adobe Experience Manager设置本地开发指南。 涵盖本地安装、Apache Maven、集成开发环境以及调试/疑难解答的重要主题。 开发条件 **Eclipse IDE、CRXDE Lite、Visual Studio代码和IntelliJ** 讨论。

## 概述

为Adobe Experience Manager或AEM进行开发时，首先需要设置本地开发环境。 请花时间设置一个质量开发环境，以更快地提高生产效率并编写更好的代码。 我们可以将AEM的本地开发环境分为四个方面：

* 本地AEM实例
* [!DNL Apache Maven] 项目
* 集成开发环境(IDE)
* 疑难解答

## 安装本地AEM实例

当我们引用本地AEM实例时，我们讨论的是在开发人员的个人计算机上运行的Adobe Experience Manager副本。 ***全部*** AEM开发应首先针对本地AEM实例编写并运行代码。

如果您是初次使用AEM，则可以安装两种基本的运行模式： ***作者*** 和 ***发布***. 的 ***作者*** [运行模式](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/configuring/configure-runmodes.html?lang=en)  是数字营销人员用于创建和管理内容的环境。 在大多数情况下进行开发时，您需要将代码部署到创作实例。 这允许您创建页面以及添加和配置组件。 AEM Sites是WYSIWYG创作CMS，因此大多数CSS和JavaScript都可以针对创作实例进行测试。

也是 *关键* 针对本地测试代码 ***发布*** 实例。 的 ***发布*** 实例是指网站访客可与之交互的AEM环境。 而 ***发布*** 实例与技术堆栈相同 ***作者*** 例如，配置和权限存在一些主要区别。 必须对本地代码进行测试 ***发布*** 实例。

### 步骤

1. 确保已安装Java™。
   * 首选 [Java™ JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) 适用于AEM 6.5+
   * [Java™ JDK 8](https://www.oracle.com/java/technologies/downloads/) 适用于AEM 6.5之前的版本
1. 获取 [AEM QuickStart Jar和 [!DNL license.properties]](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html).
1. 在计算机上创建文件夹结构，如下所示：

```plain
~/aem-sdk
    /author
    /publish
```

1. 重命名 [!DNL QuickStart] JAR到 ***aem-author-p4502.jar*** 然后放在下面 `/author` 目录访问Advertising Cloud的帮助。 添加 ***[!DNL license.properties]*** 文件下方 `/author` 目录访问Advertising Cloud的帮助。

1. 复制 [!DNL QuickStart] JAR，将其重命名为 ***aem-publish-p4503.jar*** 然后放在下面 `/publish` 目录访问Advertising Cloud的帮助。 添加 ***[!DNL license.properties]*** 文件下方 `/publish` 目录访问Advertising Cloud的帮助。

```plain
~/aem-sdk
    /author
        + aem-author-p4502.jar
        + license.properties
    /publish
        + aem-publish-p4503.jar
        + license.properties
```

1. 双击 ***aem-author-p4502.jar*** 安装文件 **作者** 实例。 这将启动在端口上运行的创作实例 **4502** 在本地计算机上。

双击 ***aem-publish-p4503.jar*** 安装文件 **发布** 实例。 这将启动在端口上运行的Publish实例 **4503** 在本地计算机上。

>[!NOTE]
>
>根据您的开发机器的硬件，可能很难同时具有 **创作和发布** 同时运行的实例。 您很少需要在本地设置上同时运行这两个设置。

### 使用命令行

双击JAR文件的另一种方法是从命令行启动AEM或创建脚本(`.bat` 或 `.sh`)，具体取决于您的本地操作系统版本。 以下示例命令：

```shell
$ java -Xmx2048M -Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=30303 -jar aem-author-p4502.jar -gui -r"author,localdev"
```

这里， `-X` 是JVM选项和 `-D` 是其他框架属性，有关详细信息，请参阅 [部署和维护AEM实例](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html) 和 [快速入门文件中提供的其他选项](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/custom-standalone-install.html#further-options-available-from-the-quickstart-file).

## 安装Apache Maven

***[!DNL Apache Maven]*** 是一个用于管理基于Java的项目的生成和部署过程的工具。 AEM是一个基于Java的平台， [!DNL Maven] 是管理AEM项目代码的标准方法。 我们说 ***AEM Maven项目*** 或是 ***AEM项目***，我们指的是包含所有 *自定义* 网站的代码。

所有AEM项目都应基于 **[!DNL AEM Project Archetype]**: [https://github.com/adobe/aem-project-archetype](https://github.com/adobe/aem-project-archetype). 的 [!DNL AEM Project Archetype] 提供AEM项目的引导程序，其中包含一些示例代码和内容。 的 [!DNL AEM Project Archetype] 还包括 **[!DNL AEM WCM Core Components]** 配置为用于您的项目。

>[!CAUTION]
>
>启动新项目时，最好使用最新版本的原型。 请记住，原型有多个版本，并且并非所有版本都与AEM的早期版本兼容。

### 步骤

1. 下载 [阿帕奇·马文](https://maven.apache.org/download.cgi)
2. 安装 [阿帕奇·马文](https://maven.apache.org/install.html) 并确保已将安装添加到您的命令行中 `PATH`.
   * [!DNL macOS] 用户可以使用 [家酿](https://brew.sh/)
3. 验证 **[!DNL Maven]** 通过打开新的命令行终端并执行以下操作来安装：

```shell
$ mvn --version
Apache Maven 3.3.9
Maven home: /Library/apache-maven-3.3.9
Java version: 1.8.0_111, vendor: Oracle Corporation
Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home/jre
Default locale: en_US, platform encoding: UTF-8
```

>[!NOTE]
>
> 在以前添加的 `adobe-public` 需要Maven用户档案来指出 `nexus.adobe.com` 下载AEM工件。 现在，所有AEM工件均可通过Maven Central和 `adobe-public` 配置文件。

## 设置集成开发环境

集成开发环境或IDE是一个应用程序，它结合了文本编辑器、语法支持和构建工具。 根据您正在进行的开发类型，一个IDE可能比另一个IDE更好。 无论IDE如何，都必须能够定期 ***推送*** 代码到本地AEM实例以进行测试。 偶尔的话很重要 ***提取*** 将本地AEM实例配置到您的AEM项目中，以便能够持续保留到诸如Git之类的源代码管理系统。

下面是一些更受欢迎的IDE，这些IDE与AEM开发一起使用，并带有相应的视频，其中显示了与本地AEM实例的集成。

>[!NOTE]
>
> WKND项目已更新为默认可用于AEMas a Cloud Service。 已更新为 [向后兼容6.5/6.4](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx). 如果使用AEM 6.5或6.4，请在 `classic` 配置文件。

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

使用IDE时，请确保检查 `classic` 在Maven Profile选项卡中。

![Maven配置文件选项卡](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*IntelliJ Maven配置文件*

### [!DNL Eclipse] IDE

的 **[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)** 是Java™开发中比较流行的IDE之一，这在很大程度上是因为它是开源的，而且 ***免费***! Adobe提供插件， **[[!DNL AEM Developer Tools]](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html)**，对于 [!DNL Eclipse] 以便于使用良好的GUI进行开发，以便将代码与本地AEM实例同步。 的 [!DNL Eclipse] 对于很大程度上不熟悉AEM的开发人员，建议使用IDE，因为 [!DNL AEM Developer Tools].

#### 安装和设置

1. 下载并安装 [!DNL Eclipse] IDE [!DNL Java™ EE Developers]: [https://www.eclipse.org](https://www.eclipse.org/)
1. 按照安装 [!DNL AEM Developer Tools] 插件： [https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 — 导入Maven项目
* 01:24 — 使用Maven构建和部署源代码
* 04:33 — 使用AEM开发人员工具更改推送代码
* 10:55 — 使用AEM开发人员工具提取代码更改
* 13:12 — 使用Eclipse的集成调试工具

### IntelliJ IDEA

的 **[IntelliJ IDEA](https://www.jetbrains.com/idea/)** 是用于专业Java™开发的强大IDE。 [!DNL IntelliJ IDEA] 有两种风格a ***免费*** [!DNL Community] 版本和商业版（付费） [!DNL Ultimate] 版本。 免费 [!DNL Community] 版本 [!DNL IntellIJ IDEA] 足以进行更多AEM开发，但 [!DNL Ultimate] [扩展其功能集](https://www.jetbrains.com/idea/download).

#### [!DNL Installation and Setup]

1. 下载并安装 [!DNL IntelliJ IDEA]: [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. 安装 [!DNL Repo] （命令行工具）： [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

* 00:00 — 导入Maven项目
* 05:47 — 使用Maven构建和部署源代码
* 08:17 — 使用Repo推送更改
* 14:39 — 使用Repo提取更改
* 17:25 — 使用IntelliJ IDEA的集成调试工具

### [!DNL Visual Studio Code]

**[Visual Studio代码](https://code.visualstudio.com/)** 已迅速成为 ***前端开发人员*** 增强了JavaScript支持， [!DNL Intellisense]和浏览器调试支持。 **[!DNL Visual Studio Code]** 是开源的免费扩展，具有许多功能强大的扩展。 [!DNL Visual Studio Code] 可设置为借助Adobe工具与AEM集成， **[存储库](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code).** 还可以安装几个社区支持的扩展，以与AEM集成。

[!DNL Visual Studio Code] 对于主要编写CSS/LESS和JavaScript代码以创建AEM客户端库的前端开发人员而言，这是一个绝佳选择。 对于新的AEM开发人员而言，此工具可能不是最佳选择，因为需要使用原始XML编辑节点定义（对话框、组件）。 有几个Java™扩展可供使用 [!DNL Visual Studio Code]，但是，如果主要进行Java™开发 [!DNL Eclipse IDE] 或 [!DNL IntelliJ] 可能是首选。

#### 重要链接

* [**下载**](https://code.visualstudio.com/Download) **Visual Studio代码**
* **[存储库](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)**  — 适用于JCR内容的类似FTP的工具
* **[aemfed](https://aemfed.io/)**  — 加快AEM前端工作流
* **[AEM同步](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)**  — 社区支持&#42; Visual Studio代码的扩展

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 — 导入Maven项目
* 00:53 — 使用Maven构建和部署源代码
* 04:03 — 使用Repo命令行工具更改推送代码
* 08:29 — 使用Repo命令行工具提取代码更改
* 10:40 — 使用aemfed工具更改推送代码
* 14:24 — 疑难解答，重建客户端库

### [!DNL CRXDE Lite]

[CRXDE Lite](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/developing-with-crxde-lite.html) 是基于浏览器的AEM存储库视图。 [!DNL CRXDE Lite] 嵌入在AEM中，允许开发人员执行标准开发任务，如编辑文件、定义组件、对话框和模板。 [!DNL CRXDE Lite] is ***not*** 旨在成为完整的开发环境，但可用作调试工具。 [!DNL CRXDE Lite] 在扩展或仅了解代码库以外的产品代码时，此变量将非常有用。 [!DNL CRXDE Lite] 提供了存储库的强大视图，以及有效测试和管理权限的方法。

[!DNL CRXDE Lite] 应与其他IDE一起使用来测试和调试代码，但绝不能将其用作主要开发工具。 它具有有限的语法支持、无法自动完成的功能，以及与源代码管理系统的集成也有限。

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## 疑难解答

***帮助!*** 我的代码不起作用！ 与所有开发一样，有时您的代码无法按预期工作（可能有许多）。 AEM是一个功能强大的平台，但其强大的功能……具有极大的复杂性。 下面是疑难解答和跟踪问题的几个高级起点（但远非详尽的可能出错的事情列表）：

### 验证代码部署

遇到问题时，一个好的第一步是验证代码是否已成功部署并安装到AEM。

1. **检查 [!UICONTROL 包管理器]** 要确保已上载并安装代码包，请执行以下操作： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). 检查时间戳，以验证最近是否已安装包。
1. 如果使用诸如之类的工具执行增量文件更新 [!DNL Repo] 或 [!DNL AEM Developer Tools], **check[!DNL CRXDE Lite]** 文件已推送到本地AEM实例，且文件内容已更新： [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **检查是否上载了包** 如果在OSGi包中看到与Java™代码相关的问题。 打开 [!UICONTROL Adobe Experience Manager Web Console]: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) 并搜索包。 确保包具有 **[!UICONTROL 活动]** 状态。 有关对中的捆绑包进行故障诊断的详细信息，请参阅下文 **[!UICONTROL 已安装]** 状态。

#### 检查日志

AEM是一个动态平台，会在 **error.log**. 的 **error.log** 可在安装AEM的位置找到：&lt; `aem-installation-folder>/crx-quickstart/logs/error.log`.

跟踪下拉问题的一种有用方法是，在Java™代码中添加log语句：

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
...

public class MyClass {
    private final Logger log = LoggerFactory.getLogger(getClass());

    ...

    String myVariable = "My Variable";

    log.debug("Debug statement of myVariable {}", myVariable);

    log.info("Info statement of myVariable {}", myVariable);
}
```

默认情况下， **error.log** 配置为日志 *[!DNL INFO]* 语句。 如果要更改日志级别，可通过以下路径执行此操作： [!UICONTROL 日志支持]: [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog). 您可能还会发现 **error.log** 太吵了。 您可以使用 [!UICONTROL 日志支持] 为仅指定的Java™包配置log语句。 这是项目的最佳实践，以便轻松地将自定义代码问题与OOTB AEM平台问题分开。

![在AEM中记录配置](./assets/set-up-a-local-aem-development-environment/logging.png)

#### 包处于“已安装”状态 {#bundle-active}

所有包（不包括片段）都应位于 **[!UICONTROL 活动]** 状态。 如果您在 [!UICONTROL 已安装] 状态，则存在需要解决的问题。 大多数情况下，这是依赖关系问题：

![AEM中的包错误](assets/set-up-a-local-aem-development-environment/bundle-error.png)

在以上屏幕截图中， [!DNL WKND Core bundle] 是 [!UICONTROL 已安装] 状态。 这是因为包需要的是 `com.adobe.cq.wcm.core.components.models` 在AEM实例中可用。

一个有用的工具是 [!UICONTROL 依赖关系查找器]: [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder). 添加Java™包名称以检查AEM实例上的可用版本：

![核心组件](assets/set-up-a-local-aem-development-environment/core-components.png)

继续上述示例，我们可以看到在AEM实例上安装的版本是 **12.2** vs **12.6** 捆绑包的期望。 从此处，您可以向后工作，并查看 [!DNL Maven] 对AEM的依赖项匹配 [!DNL Maven] AEM项目中的依赖项。 在中，以上示例 [!DNL Core Components] **v2.2.0** 已安装在AEM实例上，但生成的代码包依赖于 **v2.2.2**，因此也是出现依赖关系问题的原因。

#### 验证Sling模型注册 {#osgi-component-sling-models}

AEM组件必须由 [!DNL Sling Model] 封装任何业务逻辑并确保HTL渲染脚本保持干净。 如果遇到找不到Sling模型的问题，检查 [!DNL Sling Models] 从控制台中： [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels). 这可告知您Sling模型是否已注册以及它绑定到的资源类型（组件路径）。

![Sling模型状态](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

显示 [!DNL Sling Model], `BylineImpl` 绑定到 `wknd/components/content/byline`.

#### CSS或JavaScript问题

对于大多数CSS和JavaScript问题，使用浏览器的开发工具是进行故障诊断的最有效方法。 要在针对AEM创作实例进行开发时缩小问题，查看“已发布”页面将会很有帮助。

![CSS或JS问题](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

打开 [!UICONTROL 页面属性] 菜单，单击 [!UICONTROL 查看已发布的项目]. 这会在不使用AEM编辑器且将查询参数设置为 **wcmmode=disabled**. 这会有效地禁用AEM创作UI，并使前端问题的疑难解答/调试变得容易得多。

开发前端代码时遇到的另一个常见问题是，加载的CSS/JS是旧的或已过时的CSS/JS。 第一步，确保浏览器历史记录已清除，并在必要时启动隐身浏览器或新会话。

#### 调试客户端库

使用不同的类别和嵌入方法来包含多个客户端库，可能会很麻烦进行故障诊断。 AEM会提供一些可帮助解决此问题的工具。 最重要的工具之一是 [!UICONTROL 重建客户端库] 强制AEM重新编译任何LESS文件并生成CSS。

* [转储库](http://localhost:4502/libs/granite/ui/content/dumplibs.html)  — 列出在AEM实例中注册的所有客户端库。 &lt;host>/libs/granite/ui/content/dumplibs.html
* [测试输出](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  — 允许用户查看基于类别的clientlib的预期HTML输出。 &lt;host>/libs/granite/ui/content/dumplibs.test
* [库依赖关系验证](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  — 突出显示找不到的任何依赖项或嵌入的类别。 &lt;host>/libs/granite/ui/content/dumplibs.validate
* [重建客户端库](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)  — 允许用户强制AEM重建所有客户端库或使客户端库的缓存失效。 使用LESS进行开发时，此工具非常有效，因为这样可以强制AEM重新编译生成的CSS。 通常，与重建所有库相比，使缓存失效然后执行页面刷新更有效。 &lt;host>/libs/granite/ui/content/dumplibs.rebuild

![调试Clientlibs](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>如果您经常必须使用 [!UICONTROL 重建客户端库] 工具，则一次性重建所有客户端库可能值得。 这可能需要大约15分钟，但通常会在将来消除任何缓存问题。
