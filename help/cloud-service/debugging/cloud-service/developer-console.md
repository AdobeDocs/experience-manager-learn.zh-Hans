---
title: 开发人员控制台
description: AEM as a Cloud Service为每个环境提供一个开发人员控制台，该控制台显示运行的AEM服务的各种详细信息，这些详细信息在调试中非常有用。
feature: Developer Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5433
thumbnail: kt-5433.jpg
topic: 开发
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: 048a37a9813e7b61ff069c4606b8d23cc6b6844f
workflow-type: tm+mt
source-wordcount: '1351'
ht-degree: 0%

---


# 使用开发人员控制台将AEM作为Cloud Service进行调试

AEM as a Cloud Service为每个环境提供一个开发人员控制台，该控制台显示运行的AEM服务的各种详细信息，这些详细信息在调试中非常有用。

每个AEM作为Cloud Service环境，都有自己的开发人员控制台。

## 开发人员控制台访问

要访问和使用开发人员控制台，必须通过[Adobe的Admin Console](https://adminconsole.adobe.com)向开发人员的Adobe ID授予以下权限。

1. 确保已将Cloud Manger和AEM作为Cloud Service产品的Adobe组织在Adobe组织切换程序中处于活动状态。
1. 开发人员必须是Cloud Manager产品&#x200B;__开发人员 — Cloud Service__&#x200B;产品用户档案的成员。
   + 如果此会员资格不存在，则开发人员将无法登录到开发人员控制台。
1. 开发人员必须是AEM作者和/或发布上的&#x200B;__AEM Users__&#x200B;或&#x200B;__AEM Administrators__&#x200B;产品用户档案的成员。
   + 如果此成员身份不存在，[status](#status)转储将超时，并显示401未授权错误。

### 开发人员控制台访问疑难解答

#### 401倾倒状态时发生未授权错误

![开发人员控制台 — 401未授权](./assets/developer-console/troubleshooting__401-unauthorized.png)

如果报告了“401未授权”错误的倾弃状态，则表示您的用户尚不存在具有AEM中作为Cloud Service所需权限的用户，或者登录令牌的使用无效或已过期。

要解决401未授权问题：

1. 确保您的用户是相应的AdobeIMS产品用户档案(AEM管理员或AEM用户)的成员，将开发人员控制台关联的AEM作为Cloud Service产品实例。
   + 请记住，开发者控制台访问2个AdobeIMS产品实例；AEM作为“Cloud Service作者”和“发布”产品实例，因此，根据哪个服务层需要通过“开发人员控制台”访问，请确保使用正确的产品用户档案。
1. 以Cloud Service（作者或发布）身份登录到AEM，并确保您的用户和组已正确同步到AEM。
   + 开发人员控制台要求在相应的AEM服务层中创建用户记录，以便其验证到该服务层。
1. 清除您的浏览器Cookie以及应用程序状态(本地存储)并重新登录到开发人员控制台，确保访问令牌开发人员控制台正在使用的正确且未过期。

## 窗格

AEM作为Cloud Service作者和发布服务分别由多个实例组成，以处理流量可变性和滚动更新，而不需要停机。 这些实例称为“窗格”。 开发人员控制台中的窗格选择定义将通过其他控件公开的数据范围。

![开发人员控制台 — 窗格](./assets/developer-console/pod.png)

+ 窗格是AEM服务（“作者”或“发布”）的一个独立实例
+ 窗格是临时的，即AEM作为Cloud Service会根据需要创建和销毁
+ 只有作为Cloud Service环境的关联AEM的一部分的窗格会列出该环境的开发人员控制台的窗格切换器。
+ 在窗格切换器底部，便利选项允许按服务类型选择窗格：
   + 所有作者
   + 所有发布者
   + 所有实例

## 状态

状态提供了以文本或JSON输出输出特定AEM运行时状态的选项。 开发人员控制台提供与AEM SDK的本地快速启动的OSGi Web控制台类似的信息，其中显示的区别是开发人员控制台是只读的。

![开发人员控制台 — 状态](./assets/developer-console/status.png)

### 捆绑

将列表捆绑在AEM中的所有OSGi绑定。 此功能类似于[AEM SDK的本地快速启动程序的OSGi Bundles](http://localhost:4502/system/console/bundles)（位于`/system/console/bundles`）。

通过以下方式捆绑调试帮助：

+ 列出部署到AEM作为服务的所有OSGi包
+ 列出每个OSGi捆绑包的状态；包括是否处于活动状态
+ 向未解析的依赖关系提供详细信息，这些依赖关系会导致OSGi包变为活动

### 组件

组件列表AEM中的所有OSGi组件。 此功能类似于[AEM SDK的本地快速入门的OSGi组件](http://localhost:4502/system/console/components)（位于`/system/console/components`）。

组件可通过以下方式帮助进行调试：

+ 列出部署到AEM的所有OSGi组件作为Cloud Service
+ 提供每个OSGi组件的状态；包括活动或未满足
+ 向未满足的服务引用提供详细信息可能导致OSGi组件变为活动
+ 列出OSGi属性及其绑定到OSGi组件的值

### 配置

配置列表所有OSGi组件的配置（OSGi属性和值）。 此功能类似于[AEM SDK的本地快速启动的OSGi Configuration Manager](http://localhost:4502/system/console/configMgr)（位于`/system/console/configMgr`）。

配置通过以下方式帮助进行调试：

+ 按OSGi组件列出OSGi属性及其值
+ 查找和识别配置错误的属性

### Oak索引

Oak索引提供在`/oak:index`下定义的节点的转储。 请记住，这不会显示合并索引，在修改AEM索引时会出现合并索引。

Oak索引通过以下方式帮助进行调试：

+ 列出所有Oak Index定义，以便深入了解如何在AEM中执行搜索查询。 请记住，修改为AEM索引后不会反映在此处。 此视图仅对仅由AEM提供或仅由自定义代码提供的索引有用。

### OSGi Services

组件列表所有OSGi服务。 此功能类似于[AEM SDK的本地快速入门的OSGi Services](http://localhost:4502/system/console/services)（位于`/system/console/services`）。

OSGi服务可通过以下方式帮助进行调试：

+ 列出AEM中的所有OSGi服务，以及它提供的OSGi捆绑包，以及使用它的所有OSGi捆绑包

### Sling 作业

Sling Jobs会列表所有Sling Jobs队列。 此功能类似于[AEM SDK的本地快速启动作业](http://localhost:4502/system/console/slingevent)（位于`/system/console/slingevent`）。

Sling Jobs通过以下方式帮助进行调试：

+ Sling作业队列列表及其配置
+ 提供对活动、已排队和已处理Sling作业数量的洞察，这有助于调试工作流、临时工作流以及AEM中Sling作业执行的其他工作。

## Java包

Java包允许检查Java包和版本是否可以在AEM中用作Cloud Service。 此功能与[AEM SDK的本地快速启动的依赖项查找器](http://localhost:4502/system/console/depfinder)（位于`/system/console/depfinder`）相同。

![开发人员控制台 — Java包](./assets/developer-console/java-packages.png)

Java包用于遇到以下问题：由于未解析的导入或脚本（HTL、JSP等）中未解析的类，无法拍摄未启动的包。 如果Java包报告没有包导出Java包（或版本与OSGi包导入的版本不匹配）：

+ 确保项目的AEM API主依赖项的版本与环境的AEM发行版本匹配（如果可能，请将所有内容更新为最新版本）。
+ 如果在Maven项目中使用其他Maven依赖项
   + 确定是否可以改用AEM SDK API依赖关系提供的替代API。
   + 如果需要额外的依赖关系，请确保它作为OSGi包（而不是普通Jar）提供，并且它嵌入到项目的代码包(`ui.apps`)中，与核心OSGi Bundle嵌入`ui.apps`包的方式类似。

## Servlet

Servlet用于提供AEM如何解析URL到最终处理请求的Java Servlet或脚本(HTL、JSP)的分析。 此功能与[AEM SDK的本地快速启动程序的Sling Servlet解析程序](http://localhost:4502/system/console/servletresolver)（位于`/system/console/servletresolver`）相同。

![开发人员控制台 — Servlet](./assets/developer-console/servlets.png)

Servlet有助于调试确定：

+ 如何将URL分解为其可寻址部分（资源、选择器、扩展）。
+ URL解析为什么Servlet或脚本，有助于识别格式错误的URL或注册错误的Servlet/脚本。

## 查询

查询可帮助您深入了解在AEM上执行搜索查询的方式和内容。 此功能与[AEM SDK的本地快速启动工具>查询性能](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html)控制台相同。

查询仅在选择特定窗格时起作用，因为该窗格会打开该窗格的“查询性能”Web控制台，要求开发人员有权登录AEM服务。

![开发人员控制台 — 查询 — 说明查询](./assets/developer-console/queries__explain-query.png)

查询通过以下方式帮助进行调试：

+ 解释Oak如何解释、分析和执行查询。 当跟踪查询速度慢的原因并了解如何加速时，这非常重要。
+ 列出AEM中运行的最受欢迎的查询，并能够解释它们。
+ 列出AEM中运行速度最慢的查询，并能解释它们。
