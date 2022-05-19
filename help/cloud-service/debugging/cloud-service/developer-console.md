---
title: 开发人员控制台
description: AEM as a Cloud Service为每个环境提供了开发人员控制台，该控制台会公开运行的AEM服务的各种详细信息，这些信息在调试中很有帮助。
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5433
thumbnail: kt-5433.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 0499ff9f-d452-459f-b1a2-2853a228efd1
source-git-commit: 751aed9b8659d6a600429efb2bf60825b6d39144
workflow-type: tm+mt
source-wordcount: '1396'
ht-degree: 0%

---

# 使用开发人员控制台调试AEMas a Cloud Service

AEM as a Cloud Service为每个环境提供了开发人员控制台，该控制台会公开运行的AEM服务的各种详细信息，这些信息在调试中很有帮助。

每个AEMas a Cloud Service环境都有自己的开发人员控制台。

## 开发人员控制台访问

要访问和使用开发人员控制台，必须通过向开发人员的Adobe ID授予以下权限 [AdobeAdmin Console](https://adminconsole.adobe.com).

1. 确保已实现Cloud Manger和AEMas a Cloud Service产品的Adobe组织在Adobe组织切换器中处于活动状态。
1. 开发人员必须是Cloud Manager产品的成员 __开发人员 — Cloud Service__ 产品配置文件。
   + 如果此会员资格不存在，则开发人员将无法登录到开发人员控制台。
1. 开发人员必须是 __AEM用户__ 或 __AEM管理员__ AEM创作和/或发布上的产品配置文件。
   + 如果此会员资格不存在，则 [状态](#status) 转储将超时，并出现401未授权错误。

### 开发人员控制台访问疑难解答

#### 401倾倒状态时未授权错误

![开发人员控制台 — 401未授权](./assets/developer-console/troubleshooting__401-unauthorized.png)

如果报告了倾弃任何状态a 401未授权错误，则意味着您的用户尚不存在，且尚不具备AEMas a Cloud Service中的必要权限，或者登录令牌的使用无效或已过期。

要解决401未授权问题，请执行以下操作：

1. 确保用户是开发人员控制台关联的AEMas a Cloud Service产品实例的相应Adobe IMS产品配置文件(AEM管理员或AEM用户)的成员。
   + 请记住，开发人员控制台访问2个Adobe IMS产品实例；AEMas a Cloud Service创作和发布产品实例，因此请根据开发人员控制台需要访问的服务层，确保使用正确的产品配置文件。
1. 登录到AEMas a Cloud Service（创作或发布），并确保您的用户和组已正确同步到AEM。
   + 开发人员控制台要求在相应的AEM服务层中创建用户记录，以便对该服务层进行身份验证。
1. 清除浏览器Cookie以及应用程序状态（本地存储），并重新登录到开发人员控制台，以确保开发人员控制台使用的访问令牌正确且未过期。

## 面板

AEMas a Cloud Service创作和发布服务分别由多个实例组成，以便处理流量可变性和滚动更新，而无需停机。 这些实例称为Pod。 开发人员控制台中的面板选择定义将通过其他控件公开的数据范围。

![开发人员控制台 — 面板](./assets/developer-console/pod.png)

+ 面板是AEM Service的一部分（“创作”或“发布”）的离散实例
+ Pod是临时的，这意味着AEMas a Cloud Service会根据需要创建和销毁它们
+ 该环境的开发人员控制台的面板切换器中只列出关联的AEMas a Cloud Service环境中的Pod。
+ 在面板切换器的底部，方便选项允许按服务类型选择Pod:
   + 所有作者
   + 所有发布者
   + 所有实例

## 状态

状态提供了用于以文本或JSON输出输出特定AEM运行时状态的选项。 开发人员控制台提供的信息与AEM SDK的本地快速启动OSGi Web控制台类似，其中显示的区别在于，开发人员控制台是只读的。

![开发人员控制台 — 状态](./assets/developer-console/status.png)

### 包

包列出了AEM中的所有OSGi包。 此功能类似于 [AEM SDK的本地快速启动OSGi包](http://localhost:4502/system/console/bundles) at `/system/console/bundles`.

通过以下方式提供调试帮助的包：

+ 列出部署到AEM as a Service的所有OSGi包
+ 列出每个OSGi包的状态；包括是否处于活动状态
+ 提供导致OSGi包变得活动的未解析依赖项的详细信息

### 组件

组件列出了AEM中的所有OSGi组件。 此功能类似于 [AEM SDK的本地快速启动OSGi组件](http://localhost:4502/system/console/components) at `/system/console/components`.

组件可通过以下方式帮助进行调试：

+ 列出部署到AEM as a Cloud Service的所有OSGi组件
+ 提供每个OSGi组件的状态；包括活动状态或未满足状态
+ 向未满足的服务引用提供详细信息可能会导致OSGi组件变得活动
+ 列出OSGi属性及其与OSGi组件绑定的值。
   + 这将显示通过 [OSGi环境配置变量](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values).

### 配置

配置列出了所有OSGi组件的配置（OSGi属性和值）。 此功能类似于 [AEM SDK的本地快速启动的OSGi配置管理器](http://localhost:4502/system/console/configMgr) at `/system/console/configMgr`.

配置可通过以下方式帮助进行调试：

+ 按OSGi组件列出OSGi属性及其值
   + 这不会显示通过 [OSGi环境配置变量](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values). 请参阅 [组件](#components) 上，用于插入的值。
+ 查找和识别配置错误的属性

### Oak索引

Oak索引提供下面定义的节点的转储 `/oak:index`. 请记住，这不会显示合并的索引，在修改AEM索引时会发生合并的索引。

Oak索引有助于通过以下方式进行调试：

+ 列出所有Oak索引定义，以分析如何在AEM中执行搜索查询。 请记住，修改为AEM索引的内容不会反映在此处。 此视图仅适用于仅由AEM提供或仅由自定义代码提供的索引。

### OSGi服务

组件列出了所有OSGi服务。 此功能类似于 [AEM SDK的本地快速启动的OSGi服务](http://localhost:4502/system/console/services) at `/system/console/services`.

OSGi Services可通过以下方式帮助进行调试：

+ 列出AEM中的所有OSGi服务，以及其提供的OSGi包，以及使用该服务的所有OSGi包

### Sling 作业

Sling作业列出了所有Sling作业队列。 此功能类似于 [AEM SDK的本地快速启动作业](http://localhost:4502/system/console/slingevent) at `/system/console/slingevent`.

Sling作业可通过以下方式帮助进行调试：

+ Sling作业队列及其配置的列表
+ 分析处于活动状态、已排队和已处理的Sling作业的数量，这有助于调试工作流、临时工作流以及AEM中Sling作业执行的其他工作问题。

## Java包

Java包允许检查Java包和版本是否可在AEMas a Cloud Service中使用。 此功能与 [AEM SDK的本地快速启动依赖关系查找器](http://localhost:4502/system/console/depfinder) at `/system/console/depfinder`.

![开发人员控制台 — Java包](./assets/developer-console/java-packages.png)

Java包用于故障诊断由于未解析的导入或脚本（HTL、JSP等）中的未解析类而未启动的包。 如果Java包未报告导出Java包的包（或者版本与由OSGi包导入的版本不匹配）：

+ 确保项目的AEM API Maven依赖项的版本与环境的AEM版本相匹配（如果可能，请将所有内容更新为最新版本）。
+ 如果Maven项目中使用了额外的Maven依赖项
   + 确定是否可以改用由AEM SDK API依赖关系提供的替代API。
   + 如果需要额外的依赖项，请确保它作为OSGi包（而不是纯Jar）提供，并且它嵌入到您项目的代码包中(`ui.apps`)，与核心OSGi包在 `ui.apps` 包。

## Servlet

Servlet用于分析AEM如何将URL解析为最终处理请求的Java Servlet或脚本(HTL、JSP)。 此功能与 [AEM SDK的本地快速启动Sling Servlet解析程序](http://localhost:4502/system/console/servletresolver) at `/system/console/servletresolver`.

![开发人员控制台 — Servlet](./assets/developer-console/servlets.png)

Servlet有助于调试确定：

+ 如何将URL分解为其可寻址部分（资源、选择器、扩展）。
+ URL解析到的Servlet或脚本，有助于识别格式错误的URL或注册错误的Servlet/脚本。

## 查询

查询有助于深入分析在AEM上执行搜索查询的内容和方式。 此功能与  [AEM SDK的本地快速启动工具>查询性能 ](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) 控制台。

仅当选择了特定面板（当该面板打开其查询性能Web控制台时）时，查询才起作用，这要求开发人员有权登录AEM服务。

![开发人员控制台 — 查询 — 说明查询](./assets/developer-console/queries__explain-query.png)

查询可通过以下方式帮助进行调试：

+ 解释Oak如何解释、分析和执行查询。 当跟踪查询速度缓慢的原因以及了解如何加快查询速度时，这非常重要。
+ 列出AEM中运行的最常用查询，并提供“解释”功能。
+ 列出AEM中运行最慢的查询，并能够解释这些查询。
