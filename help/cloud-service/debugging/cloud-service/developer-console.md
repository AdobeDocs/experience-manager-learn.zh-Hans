---
title: 开发人员控制台
description: AEM asCloud Service为每个环境提供一个开发人员控制台，该控制台显示运行的AEM服务的各种详细信息，这些详细信息在调试中很有帮助。
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5433
thumbnail: kt-5433.jpg
translation-type: tm+mt
source-git-commit: 1af3661e5c18206d58d339d51d5189834e843023
workflow-type: tm+mt
source-wordcount: '1344'
ht-degree: 0%

---


# 使用开发人员控制台将AEM作为Cloud Service进行调试

AEM asCloud Service为每个环境提供一个开发人员控制台，该控制台显示运行的AEM服务的各种详细信息，这些详细信息在调试中很有帮助。

每个AEM作为Cloud Service环境，都有自己的开发人员控制台。

## 开发人员控制台访问

要访问和使用开发人员控制台，必须通过Adobe的Adobe ID向开发人员的Admin Console [授予以下权限](https://adminconsole.adobe.com)。

1. 确保Adobe组织切换器中已将Cloud Manger和AEM作为Cloud Service产品生效的Adobe组织。
1. 开发人员必须是Cloud Manager产品的开发人员- __Cloud Service产品用户档案__ 。
   + 如果此会员资格不存在，则开发人员将无法登录到开发人员控制台。
1. 开发人员必须是AEM作者和发布服务的AEM Administrators产品 __用户档案的成员__ 。
   + 如果此成员身份不存在，状态转储 [将超时](#status) ，并显示401未授权错误。

### 开发人员控制台访问疑难解答

#### 401转储状态时出现未授权错误

![开发人员控制台- 401未授权](./assets/developer-console/troubleshooting__401-unauthorized.png)

如果报告了“401未授权”错误的倾弃状态，则表示您的用户尚未作为Cloud Service在AEM中具有必要权限，或者登录令牌的使用无效或已过期。

要解决401未授权问题：

1. 确保您的用户是相应AdobeIMS产品用户档案(AEM管理员或AEM用户)的成员，作为开发人员控制台的关联的AEM(Cloud Service产品实例)。
   + 请记住，开发者控制台访问2个AdobeIMS产品实例；aem作为Cloud Service作者和发布产品实例，因此，根据需要通过开发人员控制台访问的服务层，确保使用正确的产品用户档案。
1. 以Cloud Service（作者或发布）身份登录到AEM，并确保您的用户和组已正确同步到AEM。
   + 开发人员控制台要求在相应的AEM服务层创建用户记录，以便其验证到该服务层。
1. 清除您的浏览器cookies以及应用程序状态(本地存储)并重新登录到开发人员控制台，确保访问令牌开发人员控制台正在使用并且未过期。

## 窗格

AEM作为Cloud Service作者和发布服务分别由多个实例组成，以处理流量可变性和滚动更新，而无需停机。 这些实例称为“窗格”。 开发人员控制台中的窗格选择定义将通过其他控件公开的数据范围。

![开发人员控制台——窗格](./assets/developer-console/pod.png)

+ 窗格是AEM服务（作者或发布）的一个离散实例
+ 窗格是临时的，这意味着AEM是Cloud Service根据需要创建和销毁它们
+ 只有作为Cloud Service环境的关联AEM的一部分的窗格会列出该环境的开发人员控制台的窗格切换程序。
+ 在窗格切换器的底部，便利选项允许按服务类型选择窗格：
   + 所有作者
   + 所有发布者
   + 所有实例

## 状态

状态提供了以文本或JSON输出形式输出特定AEM运行时状态的选项。 开发人员控制台提供与AEM SDK的本地快速启动OSGi Web控制台类似的信息，其区别在于开发人员控制台是只读的。

![开发人员控制台——状态](./assets/developer-console/status.png)

### 捆绑

捆绑AEM中所有OSGi捆绑的列表。 此功能与AEM SDK的 [本地快速启动的OSGi Bundles类似](http://localhost:4502/system/console/bundles) ，位 `/system/console/bundles`置

通过以下方式捆绑调试帮助：

+ 列出部署到AEM作为服务的所有OSGi捆绑包
+ 列出每个OSGi捆绑包的状态；包括是否处于活动状态
+ 向未解析的依赖关系提供详细信息，这些依赖关系导致OSGi捆绑包变为活动

### 组件

组件列表AEM中的所有OSGi组件。 此功能与AEM SDK的 [本地快速启动的OSGi组件类似](http://localhost:4502/system/console/components) ，位 `/system/console/components`置。

组件通过以下方式在调试时提供帮助：

+ 列出部署到AEM的所有OSGi组件作为Cloud Service
+ 提供每个OSGi组件的状态；包括活动还是不满意
+ 向未满足的服务引用提供详细信息可能导致OSGi组件变为活动
+ 列出绑定到OSGi组件的OSGi属性及其值

### 配置

配置列表所有OSGi组件的配置（OSGi属性和值）。 此功能与AEM SDK的 [本地快速启动的OSGi Configuration Manager类似](http://localhost:4502/system/console/configMgr) ，位 `/system/console/configMgr`置。

配置通过以下方式帮助进行调试：

+ 按OSGi组件列出OSGi属性及其值
+ 查找和识别配置错误的属性

### Oak索引

Oak索引提供在下定义的节点的转储 `/oak:index`。 请记住，这不会显示合并索引，这在修改AEM索引时发生。

Oak索引通过以下方式帮助进行调试：

+ 列出所有Oak Index定义，深入了解AEM中如何执行搜索查询。 请记住，修改为AEM索引后，此处不会反映。 此视图仅适用于仅由AEM提供或仅由自定义代码提供的索引。

### OSGi Services

组件列表所有OSGi服务。 此功能与AEM SDK的 [本地快速启动的OSGi服务类似](http://localhost:4502/system/console/services) ，位 `/system/console/services`置

OSGi Services通过以下方式帮助进行调试：

+ 列出AEM中的所有OSGi服务及其提供的OSGi捆绑包，以及使用它的所有OSGi捆绑包

### Sling 作业

Sling Jobs会列表所有Sling Jobs队列。 此功能与AEM SDK的 [本地快速启动作业类似](http://localhost:4502/system/console/slingevent) ，位 `/system/console/slingevent`置

Sling Jobs通过以下方式帮助进行调试：

+ Sling Job队列列表及其配置
+ 提供对活动、排队和已处理的Sling作业数的洞察，这有助于调试工作流、临时工作流以及AEM中Sling作业执行的其他工作。

## Java包

Java包允许检查Java包和版本是否可作为Cloud Service在AEM中使用。 此功能与AEM SDK的 [本地快速启动的依赖项查找器相同](http://localhost:4502/system/console/depfinder) ，位 `/system/console/depfinder`置

![开发人员控制台- Java包](./assets/developer-console/java-packages.png)

Java包用于解决由于未解析导入或脚本（HTL、JSP等）中未解析的类而无法启动的捆绑包的拍摄问题。 如果Java包报告没有捆绑包导出Java包（或版本与由OSGi捆绑包导入的版本不匹配）:

+ 确保项目的AEM API主依赖项的版本与环境的AEM发行版的版本匹配（如果可能，请将所有内容更新为最新版本）。
+ 如果在Maven项目中使用额外的Maven依赖项
   + 确定是否可以改用AEM SDK API依赖关系提供的替代API。
   + 如果需要额外的依赖项，请确保它作为OSGi捆绑包提供（而不是普通Jar），并且它嵌入到项目的代码包中(`ui.apps`)，与核心OSGi捆绑包嵌入到包中的方 `ui.apps` 式相似。

## Servlet

Servlet用于提供AEM如何将URL解析为最终处理请求的Java Servlet或脚本(HTL,JSP)的分析。 此功能与AEM SDK的 [本地快速启动的Sling Servlet Resolver相同](http://localhost:4502/system/console/servletresolver) ，位 `/system/console/servletresolver`。

![开发人员控制台- Servlet](./assets/developer-console/servlets.png)

Servlet有助于调试确定：

+ URL如何分解为其可寻址部分（资源、选择器、扩展）。
+ URL解析为什么servlet或脚本，有助于识别格式错误的URL或注册错误的servlet/脚本。

## 查询

查询帮助提供关于在AEM上执行搜索查询的方式和方式的洞察。 此功能与AEM SDK的本 [地快速启动工具>查询性能控制台相 ](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) 同。

查询仅在选择特定窗格时(当它打开该窗格的“查询性能”Web控制台时)才起作用，这要求开发人员有权登录AEM服务。

![开发人员控制台-查询-说明查询](./assets/developer-console/queries__explain-query.png)

查询通过以下方式帮助进行调试：

+ 解释Oak如何解释、分析和执行查询。 当跟踪查询速度慢的原因以及了解如何加快速度时，这非常重要。
+ 列出AEM中运行的最受欢迎的查询，并能够解释它们。
+ 列出AEM中运行速度最慢的查询，并能够解释它们。
