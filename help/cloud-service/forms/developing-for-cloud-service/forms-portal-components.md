---
title: 启用AEM Forms Portal组件
description: 使用核心组件构建AEM Forms门户
solution: Experience Manager
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Core Components
jira: KT-10373
exl-id: ab01573a-e95f-4041-8ccf-16046d723aba
duration: 78
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 2%

---

# Forms Portal组件

AEM Forms提供了以下开箱即用的门户组件：

**搜索和列表程序**：此组件允许您在门户页面上列出表单存储库中的表单，并提供配置选项以根据指定条件列出表单。

**草稿和提交**：当Search &amp; Lister组件显示由Forms作者公开的表单时，草稿和提交组件显示另存为草稿的表单，以供稍后完成和提交的表单。 此组件可为任何登录用户提供个性化体验。

**链接**：此组件允许您在页面上的任意位置创建表单的链接。

## 启用Forms Portal组件

启动IntelliJ并打开在[之前的步骤中创建的BankingApplication项目。](./getting-started.md)展开ui.apps->src->main->content->jcr_root->apps.bankingapplication->components

要在Adobe Experience Manager (AEM)站点中使用任何核心组件（包括现成的门户组件），您必须创建代理组件并为您的站点启用它。
新创建的代理组件需要指向现成的表单组件，以便它们继承来自它们的所有内容。 这是通过更改代理组件的content.xml中的resourceSuperType来完成的。 在content.xml中，我们还指定标题和组件组。
>[!NOTE]
>
> 您可以从此处[&#128279;](https://github.com/adobe/aem-core-forms-components/tree/master/ui.apps/src/main/content/jcr_root/apps/core/fd/components/formsportal)为这些组件中的每一个构建资源超级类型


### 草稿和提交

制作现有组件的副本（例如`button`），并将其命名为&#x200B;_draftsandsubmissions_。
![draftsandsubmissions](assets/forms-portal-components2.png)
将`.content.xml`中的内容替换为以下XML：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Drafts And Submissions"
          sling:resourceSuperType="core/fd/components/formsportal/draftsandsubmissions/v1/draftsandsubmissions"
          componentGroup="BankingApplication - Content"/>
```

### 搜索和侦听器

制作按钮组件的副本并将其重命名为&#x200B;_searchandlister_。
将`.content.xml`中的内容替换为以下XML：


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Search And Lister"
          sling:resourceSuperType="core/fd/components/formsportal/searchlister/v1/searchlister"
          componentGroup="BankingApplication - Content"/>
```

### 链接组件

制作按钮组件的副本并将其重命名为&#x200B;_链接_。
将`.content.xml`中的内容替换为以下XML：


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Link to Adaptive Form"
          sling:resourceSuperType="core/fd/components/formsportal/link/v2/link"
          componentGroup="BankingApplication - Content"/>
```

部署项目后，您应该能够在您的AEM页面中使用这些组件来创建Forms门户。

## 后续步骤

[包括云服务配置](./azure-storage-fdm.md)
