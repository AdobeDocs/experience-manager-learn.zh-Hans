---
title: 设置本地AEM开发环境
description: 了解如何为Experience Manager设置本地开发环境。 熟悉本地安装、Apache Maven、集成开发环境以及调试和疑难解答。 使用Eclipse IDE、CRXDE-Lite、Visual Studio Code和IntelliJ。
version: 6.5
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
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '2603'
ht-degree: 1%

---

# 设置本地AEM开发环境

为Adobe Experience Manager AEM设置本地开发的指南。 涵盖本地安装、Apache Maven、集成开发环境和调试/故障排除等重要主题。 开发工具 **Eclipse IDE、CRXDE Lite、Visual Studio代码和IntelliJ** 都经过了讨论。

## 概述

设置本地开发环境是为Adobe Experience Manager或AEM进行开发的第一步。 请花些时间来设置一个高质量的开发环境，以提高您的生产效率并更快地编写更好的代码。 我们可以将AEM本地开发环境分为四个方面：

* 本地AEM实例
* [!DNL Apache Maven] 项目
* 集成开发环境(IDE)
* 疑难解答

## 安装本地AEM实例

当我们提及本地AEM实例时，我们谈论的是在开发人员个人计算机上运行的Adobe Experience Manager的副本。 ***全部*** AEM开发应首先针对本地AEM实例编写和运行代码。

如果您是初次使用AEM，则可以安装两种基本运行模式： ***作者*** 和 ***Publish***. 此 ***作者*** [运行模式](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/configuring/configure-runmodes.html?lang=en)  是数字营销人员用于创建和管理内容的环境。 大多数情况下，在开发代码时，您都是将代码部署到创作实例。 这允许您创建页面并添加和配置组件。 AEM Sites是WYSIWYG创作CMS，因此大多数CSS和JavaScript都可以针对创作实例进行测试。

它也是 *关键* 针对本地测试代码 ***Publish*** 实例。 此 ***Publish*** 实例是网站访客与之交互的AEM环境。 而 ***Publish*** 实例与的技术栈栈相同 ***作者*** 例如，配置和权限有一些主要区别。 必须针对本地代码进行测试 ***Publish*** 实例升级。

### 步骤

1. 确保已安装Java™。
   * 首选 [Java™ JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) 适用于AEM 6.5+
   * [Java™ JDK 8](https://www.oracle.com/java/technologies/downloads/) 对于AEM 6.5之前的AEM版本
1. 获取 [AEM QuickStart Jar和 [!DNL license.properties]](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html).
1. 在计算机上创建文件夹结构，如下所示：

```plain
~/aem-sdk
    /author
    /publish
```

1. 重命名 [!DNL QuickStart] JAR到 ***aem-author-p4502.jar*** 然后放在下面 `/author` 目录。 添加 ***[!DNL license.properties]*** 文件位于 `/author` 目录。

1. 复制 [!DNL QuickStart] JAR，将其重命名为 ***aem-publish-p4503.jar*** 然后放在下面 `/publish` 目录。 添加副本 ***[!DNL license.properties]*** 文件位于 `/publish` 目录。

```plain
~/aem-sdk
    /author
        + aem-author-p4502.jar
        + license.properties
    /publish
        + aem-publish-p4503.jar
        + license.properties
```

1. 双击 ***aem-author-p4502.jar*** 文件以安装 **作者** 实例。 这会启动创作实例，在端口上运行 **4502** 在本地计算机上。

双击 ***aem-publish-p4503.jar*** 文件以安装 **Publish** 实例。 这将启动发布实例，在端口上运行 **4503** 在本地计算机上。

>[!NOTE]
>
>根据开发计算机的硬件，可能很难同时拥有 **创作和发布** 实例同时运行。 很少需要在本地设置上同时运行这两者。

### 使用命令行

双击JAR文件的替代方法是：从命令行启动AEM或创建脚本(`.bat` 或 `.sh`)，具体取决于您当地的操作系统风格。 以下是示例命令的示例：

```shell
$ java -Xmx2048M -Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=30303 -jar aem-author-p4502.jar -gui -r"author,localdev"
```

在此， `-X` 是JVM选项和 `-D` 是其他框架属性，有关详细信息，请参阅 [部署和维护AEM实例](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html) 和 [快速入门文件中提供的更多选项](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/custom-standalone-install.html#further-options-available-from-the-quickstart-file).

## 安装Apache Maven

***[!DNL Apache Maven]*** 是一个用于管理基于Java的项目的生成和部署过程的工具。 AEM是一个基于Java的平台，并且 [!DNL Maven] 是管理AEM项目代码的标准方法。 当我们说 ***AEM Maven项目*** 或仅您的 ***AEM项目***，我们指的是一个Maven项目，其中包含所有 *自定义* 您网站的代码。

所有AEM项目都应基于最新版本的 **[!DNL AEM Project Archetype]**： [https://github.com/adobe/aem-project-archetype](https://github.com/adobe/aem-project-archetype). 此 [!DNL AEM Project Archetype] 提供了AEM项目的引导程序，其中包含一些示例代码和内容。 此 [!DNL AEM Project Archetype] 还包括 **[!DNL AEM WCM Core Components]** 配置为在您的项目上使用。

>[!CAUTION]
>
>在启动新项目时，最佳实践是使用原型的最新版本。 请记住，原型有多个版本，并非所有版本都与早期版本的AEM兼容。

### 步骤

1. 下载 [Apache Maven](https://maven.apache.org/download.cgi)
2. 安装 [Apache Maven](https://maven.apache.org/install.html) 并确保已将安装添加到命令行中 `PATH`.
   * [!DNL macOS] 用户可以使用安装Maven [Homebrew](https://brew.sh/)
3. 验证 **[!DNL Maven]** 通过打开新的命令行终端并执行以下命令来安装：

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
> 在中，过去添加的 `adobe-public` 需要Maven配置文件来指向 `nexus.adobe.com` 下载AEM工件。 AEM现在可通过Maven Central和 `adobe-public` 配置文件不是必需的。

## 设置集成开发环境

集成开发环境或IDE是一个结合了文本编辑器、语法支持和构建工具的应用程序。 根据正在执行的开发类型，一个IDE可能比另一个IDE更可取。 不论IDE是什么，定期能够使用 ***推送*** 代码到本地AEM实例以进行测试。 偶尔使用很重要 ***提取*** 将本地AEM实例中的配置移入您的AEM项目，以便保留到源代码控制管理系统（如Git）。

下面是一些与AEM开发一起使用的更流行的IDE，它们与相应的视频一起显示了与本地AEM实例的集成。

>[!NOTE]
>
> WKND项目已更新为默认可在AEMas a Cloud Service上使用。 已更新为 [向后兼容6.5/6.4](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx). 如果使用AEM 6.5或6.4，请附加 `classic` 配置文件到任何Maven命令。

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

当时，使用IDE时，请确保检查 `classic` 在您的Maven配置文件选项卡中。

![Maven配置文件选项卡](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*IntelliJ Maven配置文件*

### [!DNL Eclipse] IDE

此 **[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)** 是Java™开发中比较流行的IDE之一，这在很大程度上是因为它是开源 ***免费***！ Adobe提供了一个插件， **[[!DNL AEM Developer Tools]](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)**，表示 [!DNL Eclipse] 以便能够使用良好的GUI进行更轻松的开发，从而将代码与本地AEM实例同步。 此 [!DNL Eclipse] IDE推荐给不熟悉AEM的开发人员，这在很大程度上是因为GUI支持 [!DNL AEM Developer Tools].

#### 安装和设置

1. 下载并安装 [!DNL Eclipse] IDE用于 [!DNL Java™ EE Developers]： [https://www.eclipse.org](https://www.eclipse.org/)
1. 按照说明安装 [!DNL AEM Developer Tools] 插件： [https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 — 导入Maven项目
* 01:24 — 使用Maven构建和部署源代码
* 04:33 — 使用AEM开发人员工具更改推送代码
* 10:55 — 使用AEM开发人员工具更改拉取代码
* 13:12 — 使用Eclipse的集成调试工具

### IntelliJ IDEA

此 **[IntelliJ IDEA](https://www.jetbrains.com/idea/)** 是一个用于专业Java™开发的强大IDE。 [!DNL IntelliJ IDEA] 有两种口味，一种 ***免费*** [!DNL Community] 版和商业（付费） [!DNL Ultimate] 版本。 免费 [!DNL Community] 版本 [!DNL IntellIJ IDEA] 对于更多AEM开发来说已经足够，但是 [!DNL Ultimate] [扩展其功能集](https://www.jetbrains.com/idea/download).

#### [!DNL Installation and Setup]

1. 下载并安装 [!DNL IntelliJ IDEA]： [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. 安装 [!DNL Repo] （命令行工具）： [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089?quality=12&learn=on)

* 00:00 — 导入Maven项目
* 05:47 — 使用Maven构建和部署源代码
* 08:17 — 使用Repo推送更改
* 14:39 — 使用Repo提取更改
* 17:25 — 使用IntelliJ IDEA的集成调试工具

### [!DNL Visual Studio Code]

**[Visual Studio Code](https://code.visualstudio.com/)** 很快成了人们最喜爱的工具 ***前端开发人员*** 具有增强的JavaScript支持， [!DNL Intellisense]和浏览器调试支持。 **[!DNL Visual Studio Code]** 是开源、免费的，并且有许多功能强大的扩展。 [!DNL Visual Studio Code] 可以设置为在Adobe工具的帮助下与AEM集成， **[存储库](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code).** 此外，还可以安装多个社区支持的扩展以与AEM集成。

[!DNL Visual Studio Code] 对于主要编写CSS/LESS和JavaScript代码以创建AEM客户端库的前端开发人员而言，这是一个很好的选择。 此工具可能不是新AEM开发人员的最佳选择，因为节点定义（对话框、组件）需要以原始XML进行编辑。 有多个可用于的Java™扩展 [!DNL Visual Studio Code]但是，如果主要进行Java™开发 [!DNL Eclipse IDE] 或 [!DNL IntelliJ] 可能首选。

#### 重要链接

* [**下载**](https://code.visualstudio.com/Download) **Visual Studio Code**
* **[存储库](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)**  — 用于JCR内容的类似FTP的工具
* **[Aemfed](https://aemfed.io/)**  — 加快AEM前端工作流程
* **[AEM同步](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)**  — 社区支持&#42; Visual Studio Code的扩展

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 — 导入Maven项目
* 00:53 — 使用Maven构建和部署源代码
* 04:03 — 使用Repo命令行工具更改推送代码
* 08:29 — 使用Repo命令行工具更改拉取代码
* 10:40 — 使用嵌入工具更改推送代码
* 14:24 — 故障排除，重建客户端库

### [!DNL CRXDE Lite]

[CRXDE Lite](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/developing-with-crxde-lite.html) 是AEM存储库的基于浏览器的视图。 [!DNL CRXDE Lite] 嵌入在AEM中，允许开发人员执行标准开发任务，如编辑文件、定义组件、对话框和模板。 [!DNL CRXDE Lite] 是 ***非*** 旨在作为完整开发环境，但可用作调试工具。 [!DNL CRXDE Lite] 在扩展或仅了解代码库以外的产品代码时，非常有用。 [!DNL CRXDE Lite] 提供了存储库的强大视图，以及有效测试和管理权限的方法。

[!DNL CRXDE Lite] 应与其他IDE一起使用来测试和调试代码，但绝不能作为主要开发工具。 它的语法支持有限，没有自动完成功能，并且与源代码控制管理系统的集成有限。

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## 疑难解答

***帮助!*** 我的代码不起作用！ 与所有开发一样，代码有时无法按预期工作（可能是很多次）。 AEM是一个功能强大的平台，但强大的……带来极大的复杂性。 以下是故障排除和跟踪问题时的几个高级起点（但远非可出错的详尽列表）：

### 验证代码部署

遇到问题时，正确的第一步是验证代码是否已成功部署并安装到AEM。

1. **Check [!UICONTROL 包管理器]** 要确保已上传并安装代码包，请执行以下操作： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). 检查时间戳以验证最近是否安装了软件包。
1. 如果使用如下工具执行增量文件更新 [!DNL Repo] 或 [!DNL AEM Developer Tools]， **check[!DNL CRXDE Lite]** 文件已推送到本地AEM实例，并且文件内容已更新： [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **检查是否已上传该捆绑包** 如果在OSGi捆绑包中看到与Java™代码相关的问题。 打开 [!UICONTROL Adobe Experience Manager Web控制台]： [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) 并搜索您的捆绑包。 确保捆绑包具有 **[!UICONTROL 活动]** 状态。 有关对中的捆绑包进行故障诊断的更多信息，请参阅下文 **[!UICONTROL 已安装]** 省/州。

#### 检查日志

AEM是一个聊天平台，将有用的信息记录在 **error.log**. 此 **error.log** 可以在已安装AEM的位置找到： &lt; `aem-installation-folder>/crx-quickstart/logs/error.log`.

用于跟踪问题的一种有效方法是在Java™代码中添加log语句：

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

默认情况下， **error.log** 配置为记录 *[!DNL INFO]* 语句。 如果要更改日志级别，可以转到 [!UICONTROL 日志支持]： [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog). 您还可以发现 **error.log** 太扯了。 您可以使用 [!UICONTROL 日志支持] 为指定的Java™包配置log语句。 这是项目的最佳实践，以便轻松地将自定义代码问题与OOTB AEM平台问题区分开。

![AEM中的日志记录配置](./assets/set-up-a-local-aem-development-environment/logging.png)

#### 捆绑包处于已安装状态 {#bundle-active}

所有包（不包括片段）应位于 **[!UICONTROL 活动]** 省/州。 如果您在中看到代码包 [!UICONTROL 已安装] 则存在一个需要解决的问题。 大多数情况下，这是一个依赖性问题：

![AEM中的包错误](assets/set-up-a-local-aem-development-environment/bundle-error.png)

在上面的屏幕快照中， [!DNL WKND Core bundle] 是 [!UICONTROL 已安装] 省/州。 这是因为该捆绑包需要其他版本的 `com.adobe.cq.wcm.core.components.models` 比AEM实例上可用的要多。

可以使用的一个有用工具是 [!UICONTROL 依赖项查找器]： [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder). 添加Java™包名称以检查AEM实例上可用的版本：

![核心组件](assets/set-up-a-local-aem-development-environment/core-components.png)

继续上面的示例，我们可以看到AEM实例上安装的版本是 **12.2** 对比 **12.6** 捆绑包所期待的。 从那里，您可以向后工作，查看 [!DNL Maven] AEM的依赖项与 [!DNL Maven] AEM项目中的依赖关系。 在中，以上示例 [!DNL Core Components] **v2.2.0** 安装在AEM实例上，但生成代码捆绑包时依赖于 **v2.2.2**，因此出现了依赖关系问题。

#### 验证Sling模型注册 {#osgi-component-sling-models}

AEM组件必须受 [!DNL Sling Model] 封装任何业务逻辑并确保HTL渲染脚本保持干净。 如果遇到无法找到Sling模型的问题，查看 [!DNL Sling Models] 从控制台中： [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels). 这可告知您的Sling模型是否已注册，以及它绑定的资源类型（组件路径）。

![Sling模型状态](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

显示注册 [!DNL Sling Model]， `BylineImpl` 绑定到组件资源类型的 `wknd/components/content/byline`.

#### CSS或JavaScript问题

对于大多数CSS和JavaScript问题，使用浏览器的开发工具是进行故障排除的最有效方法。 在针对AEM创作实例进行开发时，要缩小问题范围，查看“已发布”页面很有帮助。

![CSS或JS问题](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

打开 [!UICONTROL 页面属性] 菜单并单击 [!UICONTROL 查看已发布的项目]. 这将打开页面，但不带AEM编辑器，且查询参数设置为 **wcmmode=disabled**. 这可以有效地禁用AEM创作UI，并使前端问题的疑难解答/调试变得更加容易。

开发前端代码时遇到另一个常见问题，即代码过时或正在加载过期的CSS/JS。 第一步，确保已清除浏览器历史记录，如有必要，请启动无痕浏览器或新会话。

#### 调试客户端库

使用不同的类别和嵌入方法包含多个客户端库，进行故障排除可能会很麻烦。 AEM会公开一些可帮助解决此问题的工具。 最重要的工具之一是 [!UICONTROL 重建客户端库] 会强制AEM重新编译任何LESS文件并生成CSS。

* [转储库](http://localhost:4502/libs/granite/ui/content/dumplibs.html)  — 列出在AEM实例中注册的所有客户端库。 &lt;host>/libs/granite/ui/content/dumplibs.html
* [测试输出](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  — 允许用户根据类别查看clientlib include的预期HTML输出。 &lt;host>/libs/granite/ui/content/dumplibs.test.html
* [库依赖项验证](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  — 突出显示任何无法找到的依赖项或嵌入类别。 &lt;host>/libs/granite/ui/content/dumplibs.validate.html
* [重建客户端库](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)  — 允许用户强制AEM重建所有客户端库或使客户端库的缓存失效。 在使用LESS进行开发时，此工具有效，因为这会强制AEM重新编译生成的CSS。 通常，使缓存失效然后执行页面刷新比重新生成所有库更有效。 &lt;host>/libs/granite/ui/content/dumplibs.rebuild.html

![调试Clientlibs](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>如果您必须经常使用 [!UICONTROL 重建客户端库] 工具，则可能值得一次性重建所有客户端库。 这可能需要大约15分钟，但通常情况下会消除未来的任何缓存问题。
