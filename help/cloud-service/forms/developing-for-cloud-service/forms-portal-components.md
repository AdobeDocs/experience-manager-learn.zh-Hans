---
title: 启用AEM Forms Portal组件
description: 使用核心组件构建AEM Forms门户
solution: Experience Manager
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 10373
source-git-commit: 55583effd0400bac2e38756483d69f5bd114cb21
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%

---

# Forms门户组件

AEM Forms开箱即用地提供以下门户组件：

**搜索和制表人**:此组件允许您将表单从表单存储库列到门户页面上，并提供配置选项以根据指定的条件列出表单。

**草稿和提交**:搜索和制表人组件显示由Forms作者公开的表单，而草稿和提交组件则显示另存为草稿的表单，以供日后完成和提交的表单。 此组件可为任何已登录的用户提供个性化体验。

**链接**:此组件允许您创建指向页面上任意位置的表单的链接。

## 启用Forms Portal组件

启动IntelliJ，然后打开在 [更早的步骤。](./getting-started.md) 展开ui.apps->src->main->content->jcr_root->apps.bankingapplication->components

要在Adobe Experience Manager(AEM)网站中使用任何核心组件（包括现成的门户组件），您必须创建一个代理组件并为您的网站启用该组件。
新创建的代理组件需要指向现成的表单组件，以便它们继承其中的所有内容。 这是通过更改代理组件content.xml中的resourceSuperType来完成的。 在content.xml中，我们还指定标题和组件组。
>[!NOTE]
>
> 您可以为每个 [这些组件位于此处](https://github.com/adobe/aem-core-forms-components/tree/master/ui.apps/src/main/content/jcr_root/apps/core/fd/components/formsportal)


### 草稿和提交

复制现有组件(例如， `button`)并将其命名为 _草稿和提交_.
![草稿和提交](assets/forms-portal-components2.png)
替换 `.content.xml` ，并使用以下XML:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Drafts And Submissions"
          sling:resourceSuperType="core/fd/components/formsportal/draftsandsubmissions/v1/draftsandsubmissions"
          componentGroup="BankingApplication - Content"/>
```

### 搜索和Lister

复制按钮组件并将其重命名为 _搜索器_.
替换 `.content.xml` ，并使用以下XML:


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Search And Lister"
          sling:resourceSuperType="core/fd/components/formsportal/searchlister/v1/searchlister"
          componentGroup="BankingApplication - Content"/>
```

### 链接组件

复制按钮组件并将其重命名为 _链接_.
替换 `.content.xml` ，并使用以下XML:


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Link to Adaptive Form"
          sling:resourceSuperType="core/fd/components/formsportal/link/v2/link"
          componentGroup="BankingApplication - Content"/>
```

部署项目后，您应能够在AEM页面中使用这些组件来创建Forms门户。