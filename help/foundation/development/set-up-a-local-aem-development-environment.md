---
title: 设置本地AEM开发环境
description: 为AEM的Adobe Experience Manager设置本地开发指南。 涵盖本地安装、Apache Maven、集成开发环境以及调试/疑难解答的重要主题。 讨论了使用Eclipse IDE、CRXDE-Lite、Visual Studio代码和IntelliJ进行开发。
version: 6.4, 6.5
feature: Developer Tools
topics: development
activity: develop
audience: developer
topic: Development
role: Developer
level: Beginner
source-git-commit: ac93d6ba636e64ba6d8bbdb0840810b8f47a25c8
workflow-type: tm+mt
source-wordcount: '2661'
ht-degree: 0%

---


# 设置本地AEM开发环境

为AEM的Adobe Experience Manager设置本地开发指南。 涵盖本地安装、Apache Maven、集成开发环境以及调试/疑难解答的重要主题。 讨论了使用&#x200B;**[!DNL Eclipse IDE]、[!DNL CRXDE Lite]、[!DNL Visual Studio Code]和[!DNL IntelliJ]**&#x200B;进行开发的问题。

## 概述

为Adobe Experience Manager或AEM进行开发时，首先需要设置本地开发环境。 请花时间设置一个质量开发环境，以更快地提高生产效率并编写更好的代码。 我们可以将AEM的本地开发环境划分为4个方面：

* 本地AEM实例
* [!DNL Apache Maven] 项目
* 集成开发环境(IDE)
* 疑难解答

## 安装本地AEM实例

当我们引用本地AEM实例时，我们讨论的是在开发人员的个人计算机上运行的Adobe Experience Manager副本。 ****** 所有AEM开发应首先针对本地AEM实例编写并运行代码。

如果您是初次使用AEM，则可以安装两种基本的运行模式：***作者***&#x200B;和&#x200B;***发布***。 ***Author*** [runmode](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/configure-runmodes.html)是数字营销人员用于创建和管理内容的环境。 在开发&#x200B;**大多数**&#x200B;时，会将代码部署到创作实例。 这允许您创建新页面以及添加和配置组件。 AEM Sites是WYSIWYG创作CMS，因此大多数CSS和JavaScript都可以针对创作实例进行测试。

它还是针对本地&#x200B;***Publish***&#x200B;实例的&#x200B;*critical*&#x200B;测试代码。 ***Publish***&#x200B;实例是网站访客将与之交互的AEM环境。 虽然&#x200B;***Publish***&#x200B;实例与&#x200B;***Author***&#x200B;实例的技术堆栈相同，但配置和权限存在一些主要区别。 在将代码提升到更高级别的环境之前，应该先对本地&#x200B;***Publish***&#x200B;实例对代码&#x200B;*始终*&#x200B;进行测试。

### 步骤

1. 确保安装了[Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)。
   * 对于AEM 6.5+，首选[Java JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
   * [适用于AEM 6.5之前版本的AEM JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html#JDK8) 
2. 获取[AEM QuickStart Jar和 [!DNL license.properties]](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)的副本。
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

6. 双击&#x200B;***aem-author-p4502.jar***&#x200B;文件以安装&#x200B;**Author**&#x200B;实例。 这将启动在本地计算机上的端口&#x200B;**4502**&#x200B;上运行的创作实例。

   双击&#x200B;***aem-publish-p4503.jar***&#x200B;文件以安装&#x200B;**Publish**&#x200B;实例。 这将启动在本地计算机上的端口&#x200B;**4503**&#x200B;上运行的Publish实例。

   >[!NOTE]
   >
   >根据开发计算机的硬件，可能很难同时运行&#x200B;**Author和Publish**&#x200B;实例。 您很少需要在本地设置上同时运行这两个设置。

   有关更多信息，请参阅[部署和维护AEM实例](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html)。

## 安装Apache Maven

***[!DNL Apache Maven]*** 是一个用于管理基于Java的项目的生成和部署过程的工具。AEM是一个基于Java的平台，[!DNL Maven]是管理AEM项目代码的标准方式。 当我们说&#x200B;***AEM Maven Project***&#x200B;或仅说您的&#x200B;***AEM Project***&#x200B;时，我们指的是一个Maven项目，其中包含您网站的所有&#x200B;*自定义*&#x200B;代码。

所有AEM项目都应基于&#x200B;**[!DNL AEM Project Archetype]**&#x200B;的最新版本：[https://github.com/Adobe-Marketing-Cloud/aem-project-archetype](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype)。 [!DNL AEM Project Archetype]将创建包含一些示例代码和内容的AEM项目的引导。 [!DNL AEM Project Archetype]还包括配置为用于您的项目的&#x200B;**[!DNL AEM WCM Core Components]**。

>[!CAUTION]
>
>启动新项目时，最好使用最新版本的原型。 请记住，原型有多个版本，并且并非所有版本都与AEM的早期版本兼容。

### 步骤

1. 下载[Apache Maven](https://maven.apache.org/download.cgi)
2. 安装[Apache Maven](https://maven.apache.org/install.html)并确保已将安装添加到命令行`PATH`。
   * [!DNL macOS] 用户可以使用Homebrew安装 [Maven](https://brew.sh/)
3. 通过打开新的命令行终端并执行以下操作来验证是否安装了&#x200B;**[!DNL Maven]**:

   ```shell
   $ mvn --version
   Apache Maven 3.3.9
   Maven home: /Library/apache-maven-3.3.9
   Java version: 1.8.0_111, vendor: Oracle Corporation
   Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home/jre
   Default locale: en_US, platform encoding: UTF-8
   ```

4. 将&#x200B;**[!DNL adobe-public]**&#x200B;配置文件添加到[!DNL Maven] [settings.xml](https://maven.apache.org/settings.html)文件中，以便自动将&#x200B;**[!DNL repo.adobe.com]**&#x200B;添加到Maven构建过程。

5. 在`~/.m2/settings.xml`处创建名为`settings.xml`的文件（如果该文件不存在）。

6. 根据[此处的](https://repo.adobe.com/)说明，将&#x200B;**[!DNL adobe-public]**&#x200B;配置文件添加到`settings.xml`文件中。

   下面列出了一个示例`settings.xml`。 *请注意，和的命 `settings.xml` 名约定以及用户目录下的 `.m2` 位置很重要。*

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

7. 通过运行以下命令，验证&#x200B;**adobe-public**&#x200B;配置文件是否处于活动状态：

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

   如果未看到&#x200B;**[!DNL adobe-public]**，则表示在`~/.m2/settings.xml`文件中未正确引用Adobe存储库。 请重新访问前面的步骤，并验证settings.xml文件是否引用了Adobe存储库。

## 设置集成开发环境

集成开发环境或IDE是一个应用程序，它结合了文本编辑器、语法支持和构建工具。 根据您正在进行的开发类型，一个IDE可能比另一个IDE更好。 无论使用哪种IDE，都必须能够定期将&#x200B;***推***&#x200B;代码推送到本地AEM实例，以对其进行测试。 有时还应务必将&#x200B;******&#x200B;配置从本地AEM实例拉入AEM项目，以便能够持久保留到Git等源代码管理系统。

以下是一些更受欢迎的IDE，这些IDE与AEM开发一起使用，其对应的视频显示了与本地AEM实例的集成。

>[!NOTE]
>
> WKND项目已更新为默认使用AEM as aCloud Service。 已更新为[向后兼容6.5/6.4](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx)。 如果使用AEM 6.5或6.4，请将`classic`配置文件附加到任何Maven命令。

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

使用IDE时，请确保在Maven Profile选项卡中检查`classic`。

![Maven配置文件选项卡](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*IntelliJ Maven配置文件*

### [!DNL Eclipse] IDE

**[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)**&#x200B;是Java开发中比较流行的IDE之一，这在很大程度上是因为它是开源IDE，而&#x200B;***free***! Adobe为[!DNL Eclipse]提供了一个插件&#x200B;**[[!DNL AEM Developer Tools]](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html)**，以便使用良好的GUI更轻松地开发代码，从而将代码与本地AEM实例同步。 对于很大程度上不熟悉AEM的开发人员，建议使用[!DNL Eclipse] IDE，因为[!DNL AEM Developer Tools]支持GUI。

#### 安装和设置

1. 下载并安装[!DNL Eclipse] IDE for [!DNL Java EE Developers]:[https://www.eclipse.org](https://www.eclipse.org/)
1. 按照安装[!DNL AEM Developer Tools]插件的说明进行操作：[https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 — 导入Maven项目
* 01:24 — 使用Maven构建和部署源代码
* 04:33 — 使用AEM开发人员工具更改推送代码
* 10:55 — 使用AEM开发人员工具提取代码更改
* 13:12 — 使用Eclipse的集成调试工具

### IntelliJ IDEA

**[IntelliJ IDEA](https://www.jetbrains.com/idea/)**&#x200B;是用于专业Java开发的强大IDE。 [!DNL IntelliJ IDEA] 有两种口味：免费 ****** [!DNL Community] 版和商业版（付费） [!DNL Ultimate] 版。免费的[!DNL Community]版本的[!DNL IntellIJ IDEA]足以进行更多AEM开发，但[!DNL Ultimate] [扩展了其功能集](https://www.jetbrains.com/idea/download)。

#### [!DNL Installation and Setup]

1. 下载并安装[!DNL IntelliJ IDEA]:[https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. 安装[!DNL Repo]（命令行工具）：[https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

* 00:00 — 导入Maven项目
* 05:47 — 使用Maven构建和部署源代码
* 08:17 — 使用Repo推送更改
* 14:39 — 使用Repo提取更改
* 17:25 — 使用IntelliJ IDEA的集成调试工具

### [!DNL Visual Studio Code]

**[Visual Studio代](https://code.visualstudio.com/)** 码已迅速成为前端开发人员最喜 ***爱的工*** 具，具有增强的JavaScript支持和 [!DNL Intellisense]浏览器调试支持。**[!DNL Visual Studio Code]** 是开源的免费扩展，具有许多功能强大的扩展。[!DNL Visual Studio Code] 可设置为借助Adobe工具repo **[](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)与AEM集成。** 还可以安装几个社区支持的扩展，以与AEM集成。

[!DNL Visual Studio Code] 对于主要编写CSS/LESS和JavaScript代码以创建AEM客户端库的前端开发人员而言，这是一个绝佳选择。对于新的AEM开发人员而言，此工具可能不是最佳选择，因为节点定义（对话框、组件）都需要以原始XML进行编辑。 有几个Java扩展可用于[!DNL Visual Studio Code]，但是，如果主要使用Java开发[!DNL Eclipse IDE]或[!DNL IntelliJ]，则可能首选使用这些扩展。

#### 重要链接

* [****](https://code.visualstudio.com/Download) **DownloadVisual Studio代码**
* **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)**  — 适用于JCR内容的类似FTP的工具
* **[aemfed](https://aemfed.io/)**  — 加快AEM前端工作流
* **[AEM同步](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)**  - Visual Studio代码支持*扩展

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 — 导入Maven项目
* 00:53 — 使用Maven构建和部署源代码
* 04:03 — 使用Repo命令行工具更改推送代码
* 08:29 — 使用Repo命令行工具提取代码更改
* 10:40 — 使用aemfed工具更改推送代码
* 14:24 — 疑难解答，重建客户端库

### [!DNL CRXDE Lite]

[CRXDE](https://helpx.adobe.com/experience-manager/6-4/sites/developing/using/developing-with-crxde-lite.html) 列表基于浏览器的AEM存储库视图。[!DNL CRXDE Lite] 嵌入在AEM中，允许开发人员执行标准开发任务，如编辑文件、定义组件、对话框和模板。[!DNL CRXDE Lite] 并不 ****** 是一个完整的开发环境，但它作为调试工具非常有效。[!DNL CRXDE Lite] 在扩展或仅了解代码库以外的产品代码时，此变量将非常有用。[!DNL CRXDE Lite] 提供了存储库的强大视图，以及有效测试和管理权限的方法。

[!DNL CRXDE Lite] 应始终与其他IDE结合使用来测试和调试代码，但绝不能将其用作主要开发工具。它具有有限的语法支持、无自动完成功能以及与源代码管理系统的有限集成。

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## 疑难解答

***帮助!*** 我的代码不起作用！与所有开发一样，有时候（可能有很多）您的代码无法按预期工作。 AEM是一个功能强大的平台，但其强大的功能……具有极大的复杂性。 下面是疑难解答和跟踪问题的一些高级起点（但远非详尽的可能出错的事情列表）：

### 验证代码部署

遇到问题时，一个好的第一步是验证代码是否已成功部署并安装到AEM。

1. **选中 [!UICONTROL 包]** 管理器，以确保已上传并安装代码包： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)。检查时间戳以验证软件包是否最近安装过。
1. 如果使用[!DNL Repo]或[!DNL AEM Developer Tools]之类的工具进行增量文件更新，则&#x200B;**请检查[!DNL CRXDE Lite]**&#x200B;文件是否已推送到本地AEM实例，并且文件内容已更新：[http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **如果在OSGi包中看到与** Java代码相关的问题，请检查包是否上载。打开[!UICONTROL Adobe Experience Manager Web控制台]:[http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)并搜索包。 确保包的状态为&#x200B;**[!UICONTROL 活动]**。 有关对&#x200B;**[!UICONTROL Installed]**&#x200B;状态的包进行故障排除的详细信息，请参阅下文。

#### 检查日志

AEM是一个动态平台，在&#x200B;**error.log**&#x200B;中记录了许多有用信息。 在安装了AEM的位置可以找到&#x200B;**error.log**:&lt;`aem-installation-folder>/crx-quickstart/logs/error.log`。

跟踪下拉问题的一种有用方法是，在Java代码中添加log语句：

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

默认情况下， **error.log**&#x200B;配置为记录&#x200B;*[!DNL INFO]*&#x200B;语句。 如果要更改日志级别，可以通过转到[!UICONTROL 日志支持]来更改：[http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog)。 您可能还会发现&#x200B;**error.log**&#x200B;过于健壮。 您可以使用[!UICONTROL Log Support]为指定的Java包配置log语句。 这是项目的最佳实践，旨在轻松地将自定义代码问题与OOTB AEM平台问题分开。

![在AEM中记录配置](./assets/set-up-a-local-aem-development-environment/logging.png)

#### 包处于“已安装”状态 {#bundle-active}

所有包（不包括片段）都应处于&#x200B;**[!UICONTROL 活动]**&#x200B;状态。 如果您在[!UICONTROL Installed]状态中看到代码包，则需要解决一个问题。 大多数情况下，这是依赖关系问题：

![AEM中的包错误](assets/set-up-a-local-aem-development-environment/bundle-error.png)

在上述屏幕截图中，[!DNL WKND Core bundle]是[!UICONTROL Installed]状态。 这是因为包的版本应与AEM实例中提供的版本不同。`com.adobe.cq.wcm.core.components.models`

[!UICONTROL 依赖关系查找器]是一个有用的工具：[http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder)。 添加Java包名称以检查AEM实例上的可用版本：

![核心组件](assets/set-up-a-local-aem-development-environment/core-components.png)

继续上述示例，我们可以看到在AEM实例上安装的版本是包预期的&#x200B;**12.2**&#x200B;与&#x200B;**12.6**。 从此处，您可以向后工作，并查看AEM上的[!DNL Maven]依赖项是否与AEM项目中的[!DNL Maven]依赖项匹配。 在上述示例中，[!DNL Core Components] **v2.2.0**&#x200B;安装在AEM实例上，但生成的代码包依赖于&#x200B;**v2.2.2**，因此出现依赖关系问题的原因。

#### 验证Sling模型注册 {#osgi-component-sling-models}

AEM组件应始终由[!DNL Sling Model]进行备份，以封装任何业务逻辑，并确保HTL渲染脚本保持干净。 如果遇到找不到Sling模型的问题，从控制台中检查[!DNL Sling Models]可能会很有帮助：[http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)。 这将告诉您Sling模型是否已注册以及它绑定到的资源类型（组件路径）。

![Sling模型状态](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

显示与`wknd/components/content/byline`组件资源类型绑定的[!DNL Sling Model]、`BylineImpl`的注册。

#### CSS或JavaScript问题

对于大多数CSS和JavaScript问题，使用浏览器的开发工具是进行故障诊断的最有效方法。 要在针对AEM创作实例进行开发时缩小问题，查看“已发布”页面会很有帮助。

![CSS或JS问题](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

打开[!UICONTROL 页面属性]菜单，然后单击[!UICONTROL 查看已发布的]。 这将在没有AEM编辑器的情况下打开页面，并将查询参数设置为&#x200B;**wcmmode=disabled**。 这将有效地禁用AEM创作UI，并使前端问题的疑难解答/调试变得更加轻松。

开发前端代码时遇到的另一个常见问题是，加载的CSS/JS是旧的或已过时的。 第一步，确保浏览器历史记录已清除，并在必要时启动隐身浏览器或新会话。

#### 调试客户端库

使用不同的类别和嵌入方法来包含多个客户端库时，可能很麻烦进行故障诊断。 AEM会提供一些可帮助解决此问题的工具。 最重要的工具之一是[!UICONTROL 重建客户端库]，这将强制AEM重新编译任何LESS文件并生成CSS。

* [转储库](http://localhost:4502/libs/granite/ui/content/dumplibs.html)  — 列出在AEM实例中注册的所有客户端库。&lt;host>/libs/granite/ui/content/dumplibs.html
* [测试输出](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  — 允许用户根据类别查看clientlib包含的预期HTML输出。&lt;host>/libs/granite/ui/content/dumplibs.test
* [库依赖项验证](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  — 突出显示找不到的任何依赖项或嵌入的类别。&lt;host>/libs/granite/ui/content/dumplibs.validate
* [重建客户端库](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)  — 允许用户强制AEM重建所有客户端库或使客户端库的缓存失效。使用LESS进行开发时，此工具特别有效，因为这样可以强制AEM重新编译生成的CSS。 通常，与重建所有库相比，使缓存失效然后执行页面刷新更有效。 &lt;host>/libs/granite/ui/content/dumplibs.rebuild

![调试Clientlibs](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>如果您不断必须使用[!UICONTROL 重建客户端库]工具使缓存失效，则一次重建所有客户端库可能值得。 这可能需要大约15分钟，但通常会在将来消除任何缓存问题。
