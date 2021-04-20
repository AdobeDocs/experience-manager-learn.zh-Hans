---
title: 设置本地AEM开发环境
description: 为Adobe Experience Manager AEM设置本地开发的指南。 涵盖本地安装、Apache Maven、集成开发环境和调试/疑难解答等重要主题。 讨论了使用Eclipse IDE、CRXDE-Lite、Visual Studio代码和IntelliJ进行开发。
version: 6.4, 6.5
feature: Developer Tools
topics: development
activity: develop
audience: developer
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2657'
ht-degree: 1%

---


# 设置本地AEM开发环境

为Adobe Experience Manager AEM设置本地开发的指南。 涵盖本地安装、Apache Maven、集成开发环境和调试/疑难解答等重要主题。 讨论了使用&#x200B;**[!DNL Eclipse IDE]、[!DNL CRXDE Lite]、[!DNL Visual Studio Code]和[!DNL IntelliJ]**&#x200B;进行开发。

## 概述

设置本地开发环境是为Adobe Experience Manager或AEM进行开发时的第一步。 请花时间设置一个高质量的开发环境，以更快地提高工作效率并编写更好的代码。 我们可以将AEM本地开发环境分为4个方面：

* 本地AEM实例
* [!DNL Apache Maven] 项目
* 集成开发环境(IDE)
* 疑难解答

## 安装本地AEM实例

当我们引用本地AEM实例时，我们讨论的是在开发人员个人计算机上运行的Adobe Experience Manager副本。 ***所*** 有AEM开发应针对本地AEM实例编写和运行代码以进行开始。

如果您不熟悉AEM，可以安装两种基本运行模式：***作者***&#x200B;和&#x200B;***发布***。 ***作者*** [runmode](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/configure-runmodes.html)是数字营销人员用于创建和管理内容的环境。 在开发&#x200B;**时，大多数**&#x200B;将代码部署到Author实例。 这允许您创建新页面以及添加和配置组件。 AEM Sites是WYSIWYG创作CMS，因此大多数CSS和JavaScript都可以针对创作实例进行测试。

它也是针对本地&#x200B;***Publish***&#x200B;实例的&#x200B;*关键*&#x200B;测试代码。 ***Publish***&#x200B;实例是访客到您网站的将与之交互的AEM环境。 虽然&#x200B;***Publish***&#x200B;实例与&#x200B;***Author***&#x200B;实例是相同的技术堆栈，但在配置和权限方面存在一些主要区别。 在将代码提升到更高级别环境之前，应对本地&#x200B;***Publish***&#x200B;实例对代码&#x200B;*always*&#x200B;进行测试。

### 步骤

1. 确保已安装[Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)。
   * 对于AEM 6.5+，首选[Java JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Content%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModifiestStStStSt&amp;ord&amp;orderby.st&amp;st.st.st.st.st.st.st.st.st.st.st.st.sordered&amp;ordesc&amp;ordesc&amp;st.st.s.st.s.s.st.sorder=s.sorder=s&amp;desc&amp;reat.sordest&amp;r.s&amp;desc&amp;re=列表&amp;p.offset=0&amp;p.limit=14)
   * [适用于AEM](https://www.oracle.com/technetwork/java/javase/downloads/index.html#JDK8)  6.5之前版本的Java JDK 8
2. 获取[AEM QuickStart Jar和a [!DNL license.properties]](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)的副本。
3. 在计算机上创建文件夹结构，如下所示：

   ```plain
   ~/aem-sdk
       /author
       /publish
   ```

4. 将[!DNL QuickStart] JAR重命名为&#x200B;***aem-author-p4502.jar***，并将其放在`/author`目录下。 在`/author`目录下添加&#x200B;***[!DNL license.properties]***&#x200B;文件。
5. 制作[!DNL QuickStart] JAR的副本，将其重命名为&#x200B;***aem-publish-p4503.jar***，并将其放在`/publish`目录下。 在`/publish`目录下添加&#x200B;***[!DNL license.properties]***&#x200B;文件的副本。

   ```plain
   ~/aem-sdk
       /author
           + aem-author-p4502.jar
           + license.properties
       /publish
           + aem-publish-p4503.jar
           + license.properties
   ```

6. 多次 — 单击&#x200B;***aem-author-p4502.jar***&#x200B;文件以安装&#x200B;**Author**&#x200B;实例。 这将开始在本地计算机上的端口&#x200B;**4502**&#x200B;上运行的作者实例。

   多次 — 单击&#x200B;***aem-publish-p4503.jar***&#x200B;文件以安装&#x200B;**Publish**&#x200B;实例。 这将开始Publish实例，该实例在本地计算机上的端口&#x200B;**4503**&#x200B;上运行。

   >[!NOTE]
   >
   >根据开发机器的硬件，可能很难同时运行&#x200B;**作者实例和Publish**&#x200B;实例。 您很少需要在本地设置上同时运行两者。

   有关详细信息，请参阅[部署和维护AEM实例](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html)。

## 安装Apache Maven

***[!DNL Apache Maven]*** 是用于管理基于Java的项目的构建和部署过程的工具。AEM是一个基于Java的平台，[!DNL Maven]是管理AEM项目代码的标准方式。 当我们说&#x200B;***AEM Maven Project***&#x200B;或仅说您的&#x200B;***AEM Project***&#x200B;时，我们指的是一个Maven项目，它包含您站点的所有&#x200B;*自定义*&#x200B;代码。

所有AEM项目均应基于&#x200B;**[!DNL AEM Project Archetype]**&#x200B;的最新版本：[https://github.com/Adobe-Marketing-Cloud/aem-project-archetype](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype)。 [!DNL AEM Project Archetype]将创建包含一些示例代码和内容的AEM项目的引导。 [!DNL AEM Project Archetype]还包括配置为用于您的项目的&#x200B;**[!DNL AEM WCM Core Components]**。

>[!CAUTION]
>
>启动新项目时，最好使用最新版本的原型。 请记住，原型有多个版本，并且并非所有版本都与AEM的早期版本兼容。

### 步骤

1. 下载[Apache Maven](https://maven.apache.org/download.cgi)
2. 安装[Apache Maven](https://maven.apache.org/install.html)并确保已将安装添加到命令行`PATH`。
   * [!DNL macOS] 用户可以使用Homebrew安装 [Maven](https://brew.sh/)
3. 打开新命令行终端并执行以下操作，验证是否已安装&#x200B;**[!DNL Maven]**:

   ```shell
   $ mvn --version
   Apache Maven 3.3.9
   Maven home: /Library/apache-maven-3.3.9
   Java version: 1.8.0_111, vendor: Oracle Corporation
   Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home/jre
   Default locale: en_US, platform encoding: UTF-8
   ```

4. 将&#x200B;**[!DNL adobe-public]**&#x200B;用户档案添加到[!DNL Maven] [settings.xml](https://maven.apache.org/settings.html)文件，以便自动将&#x200B;**[!DNL repo.adobe.com]**&#x200B;添加到主生成过程。

5. 在`~/.m2/settings.xml`处创建名为`settings.xml`的文件（如果它尚不存在）。

6. 根据此处](https://repo.adobe.com/)的说明，将&#x200B;**[!DNL adobe-public]**&#x200B;用户档案添加到`settings.xml`文件。[

   下面列出了示例`settings.xml`。 *请注意，用户目录 `settings.xml` 下的命名约定和位置 `.m2` 很重要。*

   ```xml
   <settings xmlns="https://maven.apache.org/SETTINGS/1.0.0"
     xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="https://maven.apache.org/SETTINGS/1.0.0
                         https://maven.apache.org/xsd/settings-1.0.0.xsd">
   <profiles>
    <!-- ====================================================== -->
    <!-- A D O B E   P U B L I C   P R O F I L E                -->
    <!-- ====================================================== -->
        <profile>
            <id>adobe-public</id>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
            <properties>
                <releaseRepository-Id>adobe-public-releases</releaseRepository-Id>
                <releaseRepository-Name>Adobe Public Releases</releaseRepository-Name>
                <releaseRepository-URL>https://repo.adobe.com/nexus/content/groups/public</releaseRepository-URL>
            </properties>
            <repositories>
                <repository>
                    <id>adobe-public-releases</id>
                    <name>Adobe Public Repository</name>
                    <url>https://repo.adobe.com/nexus/content/groups/public</url>
                    <releases>
                        <enabled>true</enabled>
                        <updatePolicy>never</updatePolicy>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </repository>
            </repositories>
            <pluginRepositories>
                <pluginRepository>
                    <id>adobe-public-releases</id>
                    <name>Adobe Public Repository</name>
                    <url>https://repo.adobe.com/nexus/content/groups/public</url>
                    <releases>
                        <enabled>true</enabled>
                        <updatePolicy>never</updatePolicy>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </pluginRepository>
            </pluginRepositories>
        </profile>
   </profiles>
    <activeProfiles>
        <activeProfile>adobe-public</activeProfile>
    </activeProfiles>
   </settings>
   ```

7. 通过运行以下命令验证&#x200B;**adobe-public**&#x200B;用户档案是否处于活动状态：

   ```shell
   $ mvn help:effective-settings
   ...
   <activeProfiles>
       <activeProfile>adobe-public</activeProfile>
   </activeProfiles>
   <pluginGroups>
       <pluginGroup>org.apache.maven.plugins</pluginGroup>
       <pluginGroup>org.codehaus.mojo</pluginGroup>
   </pluginGroups>
   </settings>
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  0.856 s
   ```

   如果未看到&#x200B;**[!DNL adobe-public]**，则表明您的`~/.m2/settings.xml`文件中未正确引用Adobe回购。 请重新访问前面的步骤，并验证settings.xml文件是否引用了Adobe回购。

## 设置集成开发环境

集成开发环境或IDE是一个应用程序，它结合了文本编辑器、语法支持和构建工具。 根据您正在进行的开发类型，一个IDE可能比另一个IDE更好。 无论使用哪种IDE，都必须能够定期将&#x200B;***push***&#x200B;代码推送到本地AEM实例以对其进行测试。 偶尔将&#x200B;***从本地AEM实例拉***&#x200B;配置导入AEM项目，以便继续使用Git等源控制管理系统，这一点也很重要。

下面是一些更受欢迎的IDE，它们与AEM开发一起使用，并带有显示与本地AEM实例集成的相应视频。

>[!NOTE]
>
> WKND项目已更新为默认值，可将AEM作为Cloud Service使用。 它已更新为与6.5/6.4](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx)后向兼容。 [如果使用AEM 6.5或6.4，请将`classic`用户档案附加到任何Maven命令。

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

使用IDE时，请确保在“主用户档案”选项卡中检查`classic`。

![Maven用户档案选项卡](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*IntelliJ Maven用户档案*

### [!DNL Eclipse] IDE

**[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)**&#x200B;是Java开发中比较流行的IDE之一，很大程度上是因为它是开放源代码，而&#x200B;***free***! Adobe为[!DNL Eclipse]提供了一个插件&#x200B;**[[!DNL AEM Developer Tools]](https://eclipse.adobe.com/aem/dev-tools/)**，以便使用一个不错的GUI更轻松地开发代码，从而使代码与本地AEM实例同步。 对于很大程度上不熟悉AEM的开发人员，建议使用[!DNL Eclipse] IDE，因为[!DNL AEM Developer Tools]支持GUI。

#### 安装和设置

1. 下载并安装[!DNL Java EE Developers]的[!DNL Eclipse] IDE:[https://www.eclipse.org](https://www.eclipse.org/)
1. 按照说明安装[!DNL AEM Developer Tools]插件：[https://eclipse.adobe.com/aem/dev-tools/](https://eclipse.adobe.com/aem/dev-tools/)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 — 导入Maven项目
* 01:24 — 使用Maven构建和部署源代码
* 04:33 — 使用AEM Developer Tool推送代码更改
* 10:55 — 使用AEM Developer Tool提取代码更改
* 13:12 — 使用Eclipse的集成调试工具

### IntelliJ IDEA

**[IntelliJ IDEA](https://www.jetbrains.com/idea/)**&#x200B;是用于专业Java开发的强大IDE。 [!DNL IntelliJ IDEA] 有两种口味，一种是 ****** [!DNL Community] 免费版，另一种是商业（付费） [!DNL Ultimate] 版。免费的[!DNL Community]版本[!DNL IntellIJ IDEA]足以进行更多AEM开发，但[!DNL Ultimate] [扩展了其功能集](https://www.jetbrains.com/idea/download)。

#### [!DNL Installation and Setup]

1. 下载并安装[!DNL IntelliJ IDEA]:[https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. 安装[!DNL Repo]（命令行工具）：[https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

* 00:00 — 导入Maven项目
* 05:47 — 使用Maven构建和部署源代码
* 08:17 — 使用回购协议推送更改
* 14:39 — 利用回购协议进行更改
* 17:25 — 使用IntelliJ IDEA的集成调试工具

### [!DNL Visual Studio Code]

**[Visual Studio Code](https://code.visualstudio.com/)** 借助增强的JavaScript支持和浏 ***览器调试支*** 持，迅速成为前端开发人 [!DNL Intellisense]员最喜爱的工具。**[!DNL Visual Studio Code]** 是开放源，免费，并有许多强大的扩展。[!DNL Visual Studio Code] 可以设置为借助Adobe工具repo与AEM集 **[成](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)。** 还可以安装几个社区支持的扩展以与AEM集成。

[!DNL Visual Studio Code] 对于前端开发人员来说，这是一个绝佳的选择，他们将主要编写CSS/LESS和JavaScript代码以创建AEM客户端库。对于新的AEM开发人员来说，此工具可能不是最佳选择，因为节点定义（对话框、组件）都需要使用原始XML进行编辑。 [!DNL Visual Studio Code]有几个可用的Java扩展，但是，如果主要进行Java开发[!DNL Eclipse IDE]或[!DNL IntelliJ]，则可能是首选。

#### 重要链接

* [**下**](https://code.visualstudio.com/Download) **载Visual Studio代码**
* **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)**  — 适用于JCR内容的类似FTP的工具
* **[Aemfed](https://aemfed.io/)**  — 加快AEM前端工作流程
* **[AEM Sync](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)** - Community支持* Visual Studio代码扩展

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 — 导入Maven项目
* 00:53 — 使用Maven构建和部署源代码
* 04:03 — 使用回购命令行工具推送代码更改
* 08:29 — 使用回购命令行工具提取代码更改
* 10:40 — 使用aemfed工具推送代码更改
* 14:24 — 疑难解答，重新构建客户端库

### [!DNL CRXDE Lite]

[CRXDE ](https://helpx.adobe.com/experience-manager/6-4/sites/developing/using/developing-with-crxde-lite.html) Lite是AEM存储库的基于浏览器的视图。[!DNL CRXDE Lite] 嵌入AEM中，允许开发人员执行标准开发任务，如编辑文件、定义组件、对话框和模板。[!DNL CRXDE Lite] 不 ****** 是完全开发环境，但是作为调试工具非常有效。[!DNL CRXDE Lite] 在扩展或仅了解您的代码库之外的产品代码时非常有用。[!DNL CRXDE Lite] 提供存储库的强大视图，以及有效测试和管理权限的方法。

[!DNL CRXDE Lite] 应始终与其他IDE一起使用以测试和调试代码，但绝不应作为主开发工具。它的语法支持有限，没有自动完成功能，与源代码控制管理系统的集成也有限。

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## 疑难解答

***帮助!*** 我的密码无效！与所有开发一样，有时（可能有许多）您的代码无法按预期工作。 AEM是一个强大的平台，但其强大功能……非常复杂。 以下是故障排除和跟踪问题的几个高级起点(但远非对可能出错的事情进行详尽的列表):

### 验证代码部署

遇到问题时，一个好的第一步是验证是否已将代码成功部署到AEM。

1. **检查 [!UICONTROL 包]** 管理器，确保已上载并安装代码包： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)。检查时间戳以验证最近已安装程序包。
1. 如果使用[!DNL Repo]或[!DNL AEM Developer Tools]等工具进行增量文件更新，则&#x200B;**请检查[!DNL CRXDE Lite]**&#x200B;文件是否已推送到本地AEM实例，以及文件内容是否已更新：[http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **如果发现与OSGi捆绑包** 中的Java代码相关的问题，请检查捆绑包是否已上载。打开[!UICONTROL Adobe Experience Manager Web Console]:[http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)并搜索您的捆绑包。 确保捆绑包具有&#x200B;**[!UICONTROL 活动]**&#x200B;状态。 有关对&#x200B;**[!UICONTROL 已安装]**&#x200B;状态中的捆绑包进行故障诊断的详细信息，请参见下文。

#### 检查日志

AEM是一个动态平台，在&#x200B;**error.log**&#x200B;中记录了大量有用信息。 在安装了AEM的位置可以找到&#x200B;**error.log**:&lt;`aem-installation-folder>/crx-quickstart/logs/error.log`。

跟踪问题的一个有用方法是在Java代码中添加log语句：

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

默认情况下，将&#x200B;**error.log**&#x200B;配置为记录&#x200B;*[!DNL INFO]*&#x200B;语句。 如果要更改日志级别，可转到[!UICONTROL 日志支持]:[http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog)。 您也会发现&#x200B;**error.log**&#x200B;过于动态。 可以使用[!UICONTROL 日志支持]为指定的Java包配置日志语句。 这是项目的最佳实践，以便轻松将自定义代码问题与OOTB AEM平台问题分开。

![在AEM中记录配置](./assets/set-up-a-local-aem-development-environment/logging.png)

#### 捆绑处于安装状态{#bundle-active}

所有捆绑包（不包括片段）都应处于&#x200B;**[!UICONTROL 活动]**&#x200B;状态。 如果您在[!UICONTROL 已安装]状态中看到代码包，则需要解决一个问题。 多数情况下，这是一个依赖性问题：

![AEM中的捆绑错误](assets/set-up-a-local-aem-development-environment/bundle-error.png)

在上面的屏幕截图中，[!DNL WKND Core bundle]是[!UICONTROL 已安装]状态。 这是因为捆绑需要的`com.adobe.cq.wcm.core.components.models`版本与AEM实例上的版本不同。

[!UICONTROL 依赖关系查找器]是一个有用的工具：[http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder)。 添加Java包名称以检查AEM实例上可用的版本：

![核心组件](assets/set-up-a-local-aem-development-environment/core-components.png)

继续使用上述示例，我们可以看到安装在AEM实例上的版本是包期望的&#x200B;**12.2**&#x200B;与&#x200B;**12.6**。 从此处，您可以向后工作并查看AEM上的[!DNL Maven]依赖关系是否与AEM项目中的[!DNL Maven]依赖关系相匹配。 在上面的示例中，AEM实例上安装了[!DNL Core Components] **v2.2.0**，但生成的代码包依赖于&#x200B;**v2.2.2**，因此这是依赖性问题的原因。

#### 验证Sling型号注册{#osgi-component-sling-models}

AEM组件应始终由[!DNL Sling Model]备份，以封装任何业务逻辑并确保HTL呈现脚本保持干净。 如果遇到找不到Sling Model的问题，请从控制台中检查[!DNL Sling Models]可能会很有帮助：[http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)。 这将告诉您是否已注册Sling Model以及它所绑定的资源类型（组件路径）。

![Sling模型状态](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

显示与`wknd/components/content/byline`的组件资源类型关联的[!DNL Sling Model]、`BylineImpl`的注册。

#### CSS或JavaScript问题

对于大多数CSS和JavaScript问题，使用浏览器的开发工具是解决问题的最有效方式。 要在针对AEM作者实例进行开发时缩小问题范围，请将页面“已发布”视图为有帮助。

![CSS或JS问题](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

打开[!UICONTROL 页面属性]菜单，然后单击[!UICONTROL 视图作为已发布]。 这将在没有AEM编辑器的情况下打开页面，并将查询参数设置为&#x200B;**wcmmode=disabled**。 这将有效地禁用AEM创作UI，并使前端问题的疑难解答/调试更轻松。

正在加载旧或过时的CSS/JS，开发前端代码时遇到的另一个常见问题。 首先，确保已清除浏览器历史记录，并在必要时开始Incognito浏览器或新会话。

#### 调试客户端库

使用不同的类别和嵌入方法来包含多个客户端库，可能难以进行疑难解答。 AEM提供了多种工具来帮助解决此问题。 最重要的工具之一是[!UICONTROL 重建客户端库]，这将强制AEM重新编译任何LESS文件并生成CSS。

* [转储库](http://localhost:4502/libs/granite/ui/content/dumplibs.html) -列表在AEM实例中注册的所有客户端库。&lt;host>/libs/granite/ui/content/dumplibs.html
* [测试输出](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  — 允许用户根据类别查看clientlib的预期HTML输出。&lt;host>/libs/granite/ui/content/dumplibs.test
* [库依赖项验证](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  — 突出显示找不到的任何依赖项或嵌入类别。&lt;host>/libs/granite/ui/content/dumplibs.validate
* [重新构建客户端](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) 库 — 允许用户强制AEM重新构建所有客户端库或使客户端库的缓存失效。此工具在使用LESS进行开发时特别有效，因为这样可以迫使AEM重新编译生成的CSS。 通常，使缓存失效，然后执行页面刷新与重建所有库都更为有效。 &lt;host>/libs/granite/ui/content/dumplibs.rebuild

![调试客户端](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>如果您不断使用[!UICONTROL 重建客户端库]工具使缓存失效，则一次重建所有客户端库可能值得。 这可能需要大约15分钟，但通常会在将来消除任何缓存问题。
