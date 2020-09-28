---
title: 设置本地AEM开发环境
description: 为AEMAdobe Experience Manager建立地方发展指南。 涵盖本地安装、Apache Maven、集成开发环境和调试／疑难解答等重要主题。 讨论了使用Eclipse IDE、CRXDE-Lite、Visual Studio代码和IntelliJ进行开发。
version: 6.4, 6.5
feature: maven-archetype
topics: development
activity: develop
audience: developer
translation-type: tm+mt
source-git-commit: c85a59a8bd180d5affe2a5bf5939dabfb2776d73
workflow-type: tm+mt
source-wordcount: '2594'
ht-degree: 0%

---


# 设置本地AEM开发环境

为AEMAdobe Experience Manager建立地方发展指南。 涵盖本地安装、Apache Maven、集成开发环境和调试／疑难解答等重要主题。 讨论 **[!DNL Eclipse IDE]了[!DNL CRXDE Lite]与[!DNL Visual Studio Code]、和[!DNL IntelliJ]** 开发。

## 概述

建立地方发展环境是Adobe Experience Manager或AEM发展的第一步。 请花时间设置一个高质量的开发环境，以更快地提高工作效率并编写更好的代码。 我们可以将AEM的本地开发环境分为四个方面：

* 本地AEM实例
* [!DNL Apache Maven] 项目
* 集成开发环境(IDE)
* 疑难解答

## 安装本地AEM实例

当我们引用本地AEM实例时，我们将讨论在开发人员的个人计算机上运行的Adobe Experience Manager副本。 ***所有AEM*** 开发应针对本地AEM实例编写和运行代码，以实现开始。

如果您是AEM的新用户，可以安装两种基本的运行模式： ***创作*** 和 ***发布***。 Author ***运*** 行模 [](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/configure-runmodes.html) 式是数字营销人员用来创建和管理内容的环境。 在开发 **时** ，您将大部分时间将代码部署到创作实例。 这允许您创建新页面以及添加和配置组件。 AEM Sites是WYSIWYG创作CMS，因此大多数CSS和JavaScript都可以针对创作实例进行测试。

它也是针对 *本地* Publish实例的关键 ***测试代码*** 。 Publish ***实例*** 是AEM环境,访客到您的网站将与之交互。 虽然Publish ***实例与*** Author实例的技术堆栈 ***相同*** ，但在配置和权限方面存在一些主要区别。 在将代 *码提升* 到更高级别环境之前 ***，应始终针对本地*** Publish实例对代码进行测试。

### 步骤

1. 确 [保安](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html) 装了Java。
   * 对 [于AEM](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;orderby=%40jcr%3Content%2Fjcr%3AlastModifiest.3Modified&amp;stedSt.stedSt.st.st.d.st.st.st.st.st.st.st.st.st.st.st.st.st.st.st.sSt.st.st.sordordested&amp;st.st.st.st.st.s.st.s.s.st.s.s.st.s.sord=列表&amp;p.offset=0&amp;p.limit=14) 6.5+，请使用Java JDK 11
   * [AEM 6.5之前](https://www.oracle.com/technetwork/java/javase/downloads/index.html#JDK8) 的版本的Java JDK 8
2. 获取AEM QuickStart [Jar的副本和 [!DNL license.properties]](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)。
3. 在计算机上创建文件夹结构，如下所示：

   ```plain
   ~/aem-sdk
       /author
       /publish
   ```

4. 将JAR [!DNL QuickStart] 重命 ***名为aem-author-p4502.jar*** ，并将其放在目 `/author` 录下。 在目录 ***[!DNL license.properties]*** 下添加文 `/author` 件。
5. 制作JAR的副 [!DNL QuickStart] 本，将其重命 ***名为aem-publish-p4503.jar*** ，并将其放在目 `/publish` 录下。 在目录下添加文 ***[!DNL license.properties]*** 件的副 `/publish` 本。

   ```plain
   ~/aem-sdk
       /author
           + aem-author-p4502.jar
           + license.properties
       /publish
           + aem-publish-p4503.jar
           + license.properties
   ```

6. 多次-单 ***击aem-author-p4502.jar文件*** ，以安装 **Author** 实例。 这将开始在本地计算机上的端口4502 **上运行的** 创作实例。

   多次-单 ***击aem-publish-p4503.jar文件*** ，以安装 **Publish实例** 。 这将开始在本地计算机上的端口4503 **上运行的** Publish实例。

   >[!NOTE]
   >
   >根据开发机器的硬件，可能很难同时运 **行作者实例和** Publish实例。 您很少需要在本地设置上同时运行这两个程序。

   有关详细信息，请 [参阅部署和维护AEM实例](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html)。

## 安装Apache Maven

***[!DNL Apache Maven]*** 是一个用于管理基于Java的项目的构建和部署过程的工具。 AEM是基于Java的平台， [!DNL Maven] 是管理AEM项目代码的标准方式。 当我们说 ***AEM Maven Project*** ，或仅 ***指AEM Project***，我们指的是包含您站点的所有自 *定义代码的Maven* 项目。

所有AEM项目都应使用最新版本 **[!DNL AEM Project Archetype]**: [https://github.com/Adobe-Marketing-Cloud/aem-project-archetype](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype)。 将创 [!DNL AEM Project Archetype] 建包含一些示例代码和内容的AEM项目的引导。 还 [!DNL AEM Project Archetype] 包括 **[!DNL AEM WCM Core Components]** 配置为用于您的项目。

>[!CAUTION]
>
>启动新项目时，最好使用最新版本的原型。 请记住，原型有多个版本，并且并非所有版本都与AEM的早期版本兼容。

### 步骤

1. 下 [载Apache Maven](https://maven.apache.org/download.cgi)
2. 安 [装Apache](https://maven.apache.org/install.html) Maven并确保已将安装添加到命令行 `PATH`。
   * [!DNL macOS] 用户可以使用Homebrew安装 [Maven](https://brew.sh/)
3. 通过打 **[!DNL Maven]** 开新的命令行终端并执行以下操作来验证是否已安装：

   ```shell
   $ mvn --version
   Apache Maven 3.3.9
   Maven home: /Library/apache-maven-3.3.9
   Java version: 1.8.0_111, vendor: Oracle Corporation
   Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home/jre
   Default locale: en_US, platform encoding: UTF-8
   ```

4. 将用户档案 **[!DNL adobe-public]** 添加到 [!DNL Maven] settings.xml文 [件中，以便自动](https://maven.apache.org/settings.html)**[!DNL repo.adobe.com]** 添加到主生成过程。

5. 创建名为 `settings.xml` 的 `~/.m2/settings.xml` 文件（如果它尚不存在）。

6. 根据 **[!DNL adobe-public]** 此处的说 `settings.xml` 明将用户档案 [添加到文件](https://repo.adobe.com/)。

   下面列 `settings.xml` 出示例。 *请注意，用户目`settings.xml`录下的命名约定和位置`.m2`很重要。*

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

7. 通过运 **行以下命令** ，验证adobe-public用户档案是否处于活动状态：

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

   如果您看不到， **[!DNL adobe-public]** 则表明您的文件中未正确引用Adobe `~/.m2/settings.xml` 回购。 请重新访问前面的步骤，并验证settings.xml文件是否引用了Adobe回购。

## 设置集成开发环境

集成开发环境或IDE是一个应用程序，它结合了文本编辑器、语法支持和构建工具。 根据您正在进行的开发类型，一个IDE可能比另一个IDE更好。 无论使用哪种IDE，都必须能够定期将代码推 ***送*** 到本地AEM实例，以对其进行测试。 偶尔将配置从本地AEM ***实例*** 拉入AEM项目，以便继续使用像Git这样的源控制管理系统，这一点也很重要。

下面是与AEM开发一起使用的一些比较流行的IDE，这些IDE与显示与本地AEM实例集成的相应视频相结合。

### [!DNL Eclipse] IDE

IDE **[[!DNL Eclipse] 是](https://www.eclipse.org/ide/)** Java开发中最流行的IDE之一，很大程度上是因为它是开放源代码，是免 ***费的***! Adobe提供了一 **[个插件[!DNL AEM Developer Tools]](https://eclipse.adobe.com/aem/dev-tools/)**, [!DNL Eclipse] 它允许使用好的GUI更轻松地开发，以便与本地AEM实例同步代码。 由于 [!DNL Eclipse] GUI支持，因此建议初次接触AEM的开发人员使用IDE [!DNL AEM Developer Tools]。

#### 安装和设置

1. 下载并安装 [!DNL Eclipse] IDE，以 [!DNL Java EE Developers]便： [https://www.eclipse.org](https://www.eclipse.org/)
1. 按照说明安装插 [!DNL AEM Developer Tools] 件： [https://eclipse.adobe.com/aem/dev-tools/](https://eclipse.adobe.com/aem/dev-tools/)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 —— 导入Maven项目
* 01:24 —— 使用Maven构建和部署源代码
* 04:33 —— 使用AEM Developer Tool推送代码更改
* 10:55 —— 使用AEM Developer Tool提取代码更改
* 13:12 —— 使用Eclipse的集成调试工具

### IntelliJ IDEA

IntelliJ **[IDEA](https://www.jetbrains.com/idea/)** 是用于专业Java开发的强大IDE。 [!DNL IntelliJ IDEA] 有两种口味，一种是 ***免费***[!DNL Community] 版，另一种是商业（付费） [!DNL Ultimate] 版。 免费版 [!DNL Community] 本足 [!DNL IntellIJ IDEA] 以支持更多AEM开发，但扩展 [!DNL Ultimate] 了 [其功能集](https://www.jetbrains.com/idea/download)。

#### [!DNL Installation and Setup]

1. 下载并安装 [!DNL IntelliJ IDEA]: [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. 安 [!DNL Repo] 装（命令行工具）: [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

* 00:00 —— 导入Maven项目
* 05:47 —— 使用Maven构建和部署源代码
* 08:17 —— 使用回购工具推送更改
* 14:39 —— 利用回购协议拉取更改
* 17:25 —使用IntelliJ IDEA的集成调试工具

### [!DNL Visual Studio Code]

**[Visual Studio](https://code.visualstudio.com/)** Code凭借增强的JavaScript支 ***持和浏览器调试支持*** ，迅速成为前端开发 [!DNL Intellisense]人员最喜爱的工具。 **[!DNL Visual Studio Code]** 是开放源代码，免费，具有许多功能强大的扩展。 [!DNL Visual Studio Code] 可以设置为在Adobe工具repo的帮助下与AEM集 **[成](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)。** 还可以安装几个社区支持的扩展以与AEM集成。

[!DNL Visual Studio Code] 对于前端开发人员来说，这是一个绝佳选择，他们将主要编写CSS/LESS和JavaScript代码来创建AEM客户端库。 对于新的AEM开发人员来说，此工具可能不是最佳选择，因为节点定义（对话框、组件）都需要使用原始XML进行编辑。 有几种Java扩展可用 [!DNL Visual Studio Code]于，但主要是进行Java开 [!DNL Eclipse IDE] 发或 [!DNL IntelliJ] 首选。

#### 重要链接

* [**下载**](https://code.visualstudio.com/Download)**Visual Studio代码**
* **[回购](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)** -适用于JCR内容的类似FTP的工具
* **[Aemfed](https://aemfed.io/)** —— 加快AEM前端工作流程
* **[AEM Sync](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)** - Community支持的* Visual Studio代码扩展

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 —— 导入Maven项目
* 00:53 —— 使用Maven构建和部署源代码
* 04:03 —— 使用回购命令行工具推送代码更改
* 08:29 —— 使用回购命令行工具提取代码更改
* 10:40 —— 使用aemfed工具推送代码更改
* 14:24 —— 疑难解答，重建客户端库

### [!DNL CRXDE Lite]

[CRXDE Lite](https://helpx.adobe.com/experience-manager/6-4/sites/developing/using/developing-with-crxde-lite.html) 是AEM存储库的基于浏览器的视图。 [!DNL CRXDE Lite] 嵌入AEM中，允许开发人员执行标准开发任务，如编辑文件、定义组件、对话框和模板。 [!DNL CRXDE Lite] 不 ***是*** “完全开发环境”，但是它作为调试工具非常有效。 [!DNL CRXDE Lite] 在扩展或仅了解代码库之外的产品代码时非常有用。 [!DNL CRXDE Lite] 提供存储库的强大视图，以及有效测试和管理权限的方法。

[!DNL CRXDE Lite] 应始终与其他IDE结合使用来测试和调试代码，但绝不作为主开发工具。 它的语法支持有限，没有自动完成功能，与源代码控制管理系统的集成也有限。

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## 疑难解答

***帮助!*** 我的代码不起作用！ 与所有开发一样，有时（可能很多）您的代码会不按预期工作。 AEM是一个功能强大的平台，但其强大功能……复杂性很高。 以下是故障排除和跟踪问题的几个高级起点(但远非详尽地列表可能出问题的内容):

### 验证代码部署

遇到问题时，最好的第一步是验证是否已将代码成功部署到AEM。

1. **检查[!UICONTROL 包管理器]** ，确保已上传并安装代码包： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)。 检查时间戳以验证最近已安装该包。
1. 如果使用工具（如或）进行 [!DNL Repo] 增量文 [!DNL AEM Developer Tools]件 **更新，[!DNL CRXDE Lite]** 请检查文件是否已推送到本地AEM实例，以及文件内容是否已更新： [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **如果在OSGi捆绑包中看到与** Java代码相关的问题，请检查捆绑包是否已上传。 打开 [!UICONTROL Adobe Experience ManagerWeb控制台]: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) ，并搜索您的捆绑包。 确保捆绑包处于“活 **[!UICONTROL 动]** ”状态。 有关在“已安装”状态下对捆绑包进行故障诊断的更 **[!UICONTROL 多信息]** ，请参见下文。

#### 检查日志

AEM是一个动态平台，在error.log中记录大量 **有用信息**。 在 **安装AEM的位置** ，可以找到error.log:&lt; `aem-installation-folder>/crx-quickstart/logs/error.log`A.

跟踪问题的一种有用方法是在Java代码中添加日志语句：

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

默认情况下 **,error.log** 配置为记录语 *[!DNL INFO]* 句。 如果要更改日志级别，可转到日志支 [!UICONTROL 持]: [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog)。 您也会发现error. **log过于** chatty。 您可以使用 [!UICONTROL 日志支持] ，为指定的Java包配置日志语句。 这是项目的最佳实践，以便轻松将自定义代码问题与OOTB AEM平台问题分开。

![AEM中的日志记录配置](./assets/set-up-a-local-aem-development-environment/logging.png)

#### 捆绑处于“已安装”状态 {#bundle-active}

所有捆绑包（不包括片段）应处于“活 **[!UICONTROL 动]** ”状态。 如果您看到代码包处于“已 [!UICONTROL 安装] ”状态，则有一个问题需要解决。 大多数情况下，这是依赖关系问题：

![AEM中的捆绑错误](assets/set-up-a-local-aem-development-environment/bundle-error.png)

在上面的屏幕截图中， [!DNL WKND Core bundle] 是“已 [!UICONTROL 安装] ”状态。 这是因为捆绑需要的版本与AEM实例 `com.adobe.cq.wcm.core.components.models` 上的版本不同。

一个有用的工具是依赖关系 [!UICONTROL 查找器]: [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder)。 添加Java包名称以检查AEM实例上可用的版本：

![核心组件](assets/set-up-a-local-aem-development-environment/core-components.png)

继续使用上述示例，我们可以看到，在AEM实例上安 **装的版本是12** . **2，而在捆绑包中安装的版本是** 12.6。 从此处，您可以向后工作并查看AEM上的 [!DNL Maven] 依赖项是否与AEM [!DNL Maven] 项目中的依赖项匹配。 在上例中 [!DNL Core Components] , **v2.2.0安装在AEM实例上，但代码包是使用对** v2.2.2的依赖关系构建的 ****，因此是依赖关系问题的原因。

#### 验证Sling模型注册 {#osgi-component-sling-models}

AEM组件应始终由封装任何 [!DNL Sling Model] 业务逻辑的组件进行备份，并确保HTL渲染脚本保持干净。 如果遇到找不到Sling Model的问题，从控制台中检查 [!DNL Sling Models] 可能会很有帮助： [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)。 这将告诉您Sling Model是否已注册，以及它绑定到的资源类型（组件路径）。

![Sling模型状态](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

显示与组件资 [!DNL Sling Model]源 `BylineImpl` 类型关联的组件的注册 `wknd/components/content/byline`。

#### CSS或JavaScript问题

对于大多数CSS和JavaScript问题，使用浏览器的开发工具是解决问题的最有效方法。 要在根据AEM作者实例进行开发时缩小问题，将页面视图为“已发布”很有帮助。

![CSS或JS问题](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

打开“页 [!UICONTROL 面属性] ”菜单，然 [!UICONTROL 后单击“已发布”]。 此操作将在不使用AEM编辑器的情况下打开页面，并将查询参数设置 **为wcmmode=disabled**。 这将有效地禁用AEM创作UI，并使前端问题的疑难解答／调试更轻松。

前端代码开发时遇到的另一个常见问题是CSS/JS正在加载中过期或过时。 首先，确保已清除浏览器历史记录，并在必要时开始隐姓埋名的浏览器或新会话。

#### 调试客户端库

使用不同的类别和嵌入方法来包括多个客户端库，可能难以进行疑难解答。 AEM提供了多种工具来帮助解决此问题。 最重要的工具之一是重 [!UICONTROL 建客户端库] ，它将强制AEM重新编译任何LESS文件并生成CSS。

* [转储库](http://localhost:4502/libs/granite/ui/content/dumplibs.html) -列表AEM实例中注册的所有客户端库。 &lt;host>/libs/granite/ui/content/dumplibs.html
* [测试输出](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) -允许用户根据类别查看clientlib的预期HTML输出。 &lt;host>/libs/granite/ui/content/dumplibs.test
* [库依赖项验证](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) -突出显示找不到的任何依赖项或嵌入类别。 &lt;host>/libs/granite/ui/content/dumplibs.validate
* [重建客户端库](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) -允许用户强制AEM重建所有客户端库或使客户端库的缓存失效。 此工具在使用LESS进行开发时特别有效，因为这会迫使AEM重新编译生成的CSS。 通常，使缓存失效，然后执行页面刷新与重建所有库相比更有效。 &lt;host>/libs/granite/ui/content/dumplibs.rebuild

![调试客户端库](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>如果您经常必须使用“重建客户端库”工 [!UICONTROL 具使缓存失效] ，则对所有客户端库进行一次重建可能是值得的。 这可能需要大约15分钟，但通常会消除将来的任何缓存问题。
