---
title: 开发人员控制台
description: AEMas a Cloud Service为每个环境提供了一个开发人员控制台，该控制台显示运行的AEM服务的各种详细信息，这些详细信息对调试很有帮助。
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5433
thumbnail: kt-5433.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 0499ff9f-d452-459f-b1a2-2853a228efd1
duration: 295
source-git-commit: 1d9aeb4e5bd41096a28e3375d124bd6b6b8784aa
workflow-type: tm+mt
source-wordcount: '1562'
ht-degree: 0%

---

# 使用开发人员控制台调试AEMas a Cloud Service

AEMas a Cloud Service为每个环境提供了一个开发人员控制台，该控制台显示运行的AEM服务的各种详细信息，这些详细信息对调试很有帮助。

每个AEMas a Cloud Service环境都有自己的开发人员控制台。

## 导航到开发人员控制台

通过Cloud Manager，每个AEMas a Cloud Service环境可访问开发人员控制台。

![导航到开发人员控制台](./assets/developer-console/navigate.png)

1. 导航到 __[Cloud Manager](https://my.cloudmanager.adobe.com/)__
2. 打开 __项目__ 该API包含用于打开开发人员控制台的AEMas a Cloud Service环境。
3. 找到 __环境__，然后选择 `...`.
4. 选择 __开发人员控制台__ 下拉列表中。


## 开发人员控制台访问

要访问和使用Developer Console，必须通过以下方式向开发人员的Adobe ID授予以下权限 [AdobeAdmin Console](https://adminconsole.adobe.com).

1. 确保在Adobe组织切换器中查看与要在开发人员控制台中检查的Adobe相关的环境组织。
1. 为了能够登录到Developer Console，开发人员必须是以下任意角色的成员：
   + [Cloud Manager产品的 __开发人员 — Cloud Service__ 产品配置文件](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-cloud-manager.html#assign-developer)：在这种情况下，开发人员将看到在选定的开发人员控制台URL下可用的环境的完整列表；如果已在Cloud Manager中选择了开发环境或RDE，则可能会显示同一程序中的其他开发环境或RDE。
   + [__AEM管理员__ 上的产品配置文件 __AEM创作__](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html#aem-product-profiles)：在这种情况下，上一个项目符号中描述的环境列表将仅限于分配了此角色的相关产品配置文件。
1. 开发人员必须是 [__AEM用户__ 或 __AEM管理员__ AEM创作和/或发布的产品配置文件](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html#aem-product-profiles).
   + 如果此成员资格不存在， [状态](#status) 转储将超时，并出现401未授权错误。

### 开发人员控制台访问疑难解答

#### 登录时，我没有看到所查找的环境已列出

确保以下各项：

+ 通过通过Cloud Manager单击所选环境的三个圆点，然后选择开发人员控制台，您已选择正确的开发人员控制台URL。
+ 您或者拥有 [Cloud Manager产品的 __开发人员 — Cloud Service__ 产品配置文件](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-cloud-manager.html#assign-developer) 查看环境的完整列表，或者您属于 [__AEM管理员__ 上的产品配置文件 __AEM创作__](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html#aem-product-profiles) 对于您未找到的环境。

#### 401转储状态时发生未授权错误

![开发人员控制台 — 401未授权](./assets/developer-console/troubleshooting__401-unauthorized.png)

如果转储任何状态时报告了“401未授权错误”，则表示您的用户尚不存在，且具有AEMas a Cloud Service中的必要权限，或者登录令牌使用的无效或已过期。

要解决401未授权问题，请执行以下操作：

1. 确保您的用户是开发人员控制台关联的AEMas a Cloud Service产品实例所对应的AEM产品配置文件(AEM管理员或用户)的成员。
   + 请记住，Developer Console可访问2个Adobe IMS产品实例；AEMas a Cloud Service的创作和发布产品实例，因此请确保根据服务层需要通过Developer Console进行访问来使用正确的产品配置文件。
1. 登录到AEMas a Cloud Service（创作或发布），并确保您的用户和组已正确同步到AEM中。
   + 开发人员控制台要求在相应的AEM服务层中创建您的用户记录，以便它对该服务层进行身份验证。
1. 清除您的浏览器Cookie以及应用程序状态（本地存储）并重新登录到开发人员控制台，确保开发人员控制台使用的访问令牌正确且未过期。

## Pod

AEMas a Cloud Service的Author和Publish服务分别由多个实例组成，以便处理流量可变性和滚动更新，而无需停机。 这些实例称为Pod。 开发人员控制台中的Pod选择定义将通过其他控件公开的数据范围。

![开发人员控制台 — Pod](./assets/developer-console/pod.png)

+ Pod是属于AEM服务（创作或发布）的离散实例
+ Pod是瞬态的，这意味着AEMas a Cloud Service会根据需要创建和销毁它们
+ 只有属于关联AEMas a Cloud Service环境的Pod才会列在该环境的开发人员控制台的Pod切换器中。
+ 在Pod切换器的底部，方便选项允许按服务类型选择Pod：
   + 所有作者
   + 所有发布者
   + 所有实例

## 状态

状态提供用于以文本或JSON输出形式输出特定AEM运行时状态的选项。 开发人员控制台提供与AEM SDK的本地快速入门的OSGi Web控制台类似的信息，两者之间具有显着差异：开发人员控制台为只读。

![开发人员控制台 — 状态](./assets/developer-console/status.png)

### 包

包列出了AEM中的所有OSGi包。 此功能类似于 [AEM SDK的本地快速入门的OSGi包](http://localhost:4502/system/console/bundles) 在 `/system/console/bundles`.

捆绑包可帮助进行以下调试：

+ 列出部署到AEM as a Service的所有OSGi包
+ 列出每个OSGi捆绑包的状态；包括它们是否处于活动状态
+ 提供导致OSGi捆绑包变为活动状态的未解析依赖项的详细信息

### 组件

组件列出了AEM中的所有OSGi组件。 此功能类似于 [AEM SDK的本地快速入门的OSGi组件](http://localhost:4502/system/console/components) 在 `/system/console/components`.

组件通过以下方式帮助进行调试：

+ 列出部署到AEMas a Cloud Service的所有OSGi组件
+ 提供每个OSGi组件的状态；包括它们是活动的还是不满足的
+ 向不满足的服务引用提供详细信息可能会导致OSGi组件变为活动状态
+ 列出绑定到OSGi组件的OSGi属性及其值。
   + 这将显示通过注入的实际值 [OSGi环境配置变量](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values).

### 配置

配置列出了所有OSGi组件的配置（OSGi属性和值）。 此功能类似于 [AEM SDK的本地快速入门的OSGi Configuration Manager](http://localhost:4502/system/console/configMgr) 在 `/system/console/configMgr`.

配置可通过以下方式帮助进行调试：

+ 按OSGi组件列出OSGi属性及其值
   + 这不会显示通过插入的实际值 [OSGi环境配置变量](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values). 请参阅 [组件](#components) 以上，表示注入值。
+ 查找和识别配置错误的属性

### Oak索引

Oak索引提供下定义的节点的转储 `/oak:index`. 请记住，这不会显示合并的索引，在修改AEM索引时会发生这种情况。

Oak索引通过以下方式帮助进行调试：

+ 列出所有Oak索引定义，以提供有关如何在AEM中执行搜索查询的见解。 请记住，此处不反映修改为AEM索引的情况。 此视图仅对仅由AEM提供或仅由自定义代码提供的索引有帮助。

### OSGi服务

组件列出了所有OSGi服务。 此功能类似于 [AEM SDK的本地快速入门的OSGi服务](http://localhost:4502/system/console/services) 在 `/system/console/services`.

OSGi Services帮助通过以下方式调试：

+ 列出AEM中的所有OSGi服务，以及其提供的OSGi捆绑包和使用该捆绑包的所有OSGi捆绑包

### Sling 作业

Sling作业列出了所有Sling作业队列。 此功能类似于 [AEM SDK的本地快速入门作业](http://localhost:4502/system/console/slingevent) 在 `/system/console/slingevent`.

Sling作业通过以下方式帮助进行调试：

+ Sling作业队列及其配置列表
+ 提供对活动、已排队和已处理Sling作业数量的分析，这有助于调试工作流、临时工作流以及AEM中Sling作业执行的其他工作中存在的问题。

## Java包

Java包允许检查Java包和版本是否可以在AEMas a Cloud Service中使用。 此功能与 [AEM SDK的本地快速入门依赖项查找器](http://localhost:4502/system/console/depfinder) 在 `/system/console/depfinder`.

![开发人员控制台 — Java包](./assets/developer-console/java-packages.png)

Java包用于对由于未解析的导入或脚本（HTL、JSP等）中的类未解析而导致捆绑包无法启动进行故障排除。 如果Java包报告没有捆绑包导出Java包（或版本与OSGi捆绑包导入的版本不匹配）：

+ 确保项目的AEM API maven依赖项的版本与环境的AEM发行版本匹配（如果可能，将所有内容更新到最新版本）。
+ 如果在Maven项目中使用额外的Maven依赖项
   + 确定是否可改用AEM SDK API依赖项提供的替代API。
   + 如果需要额外的依赖项，请确保它作为OSGi捆绑包（而不是纯Jar）提供，并且嵌入到项目的代码包中，(`ui.apps`)，与中嵌入核心OSGi包的方式类似 `ui.apps` 包。

## Servlet

Servlet用于提供有关AEM如何将URL解析为最终处理请求的Java servlet或脚本(HTL、JSP)的分析。 此功能与 [AEM SDK的本地快速入门的Sling Servlet解析程序](http://localhost:4502/system/console/servletresolver) 在 `/system/console/servletresolver`.

![开发人员控制台 — Servlet](./assets/developer-console/servlets.png)

Servlet有助于调试确定：

+ 如何将URL分解为其可寻址部分（资源、选择器、扩展）。
+ URL解析到的servlet或脚本，可帮助识别格式错误的URL或注册错误的servlet/脚本。

## 查询

查询有助于深入分析在AEM上执行搜索查询的内容和方式。 此功能与  [AEM SDK的本地快速入门版的“工具”>“查询性能”](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) 控制台。

查询仅在选择了特定面板时有效，因为它会打开该面板的查询性能Web控制台，要求开发人员有权登录AEM服务。

![开发人员控制台 — 查询 — 说明查询](./assets/developer-console/queries__explain-query.png)

查询可通过以下方式帮助进行调试：

+ 解释Oak如何解释、分析和执行查询。 在跟踪为什么查询速度缓慢并了解如何加快查询速度时，这一点非常重要。
+ 列出在AEM中运行的最受欢迎的查询，并可解释这些查询。
+ 列出在AEM中运行最慢的查询，并可解释这些查询。
