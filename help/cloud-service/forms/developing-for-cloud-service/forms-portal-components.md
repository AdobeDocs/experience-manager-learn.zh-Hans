---
title: 启用AEM Forms Portal组件
description: 使用核心组件构建AEM Forms门户
solution: Experience Manager
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 10373
exl-id: ab01573a-e95f-4041-8ccf-16046d723aba
source-git-commit: 69cd5022d136e9fa84f33d2fc5ca249ac0fb6490
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%

---

# Forms Portal组件

AEM Forms提供以下现成的门户组件：

**搜索和列表程序**：利用此组件，可将表单存储库中的表单列出到门户页面上，并提供根据指定条件列出表单的配置选项。

**草稿和提交**：当Search &amp; Lister组件显示Forms作者公开表单时，Drafts &amp; Submissions组件显示另存为草稿的表单，以供以后填写和提交的表单。 此组件可为任何登录用户提供个性化体验。

**链接**：利用此组件，可创建指向页面上任何位置的表单的链接。

## 启用Forms Portal组件

启动IntelliJ并打开在 [更早的步骤。](./getting-started.md) 展开ui.apps->src->main->content->jcr_root->apps.bankingapplication->components

要在Adobe Experience Manager (AEM)站点中使用任何核心组件（包括现成的门户组件），您必须创建一个代理组件并为您的站点启用它。
新创建的代理组件需要指向现成的表单组件，以便它们继承来自它们的所有内容。 这是通过更改代理组件的content.xml中的resourceSuperType来完成的。 在content.xml中，我们还指定标题和组件组。
>[!NOTE]
>
> 您可以为的每个ID构建资源超类型 [这些组件来自此处](https://github.com/adobe/aem-core-forms-components/tree/master/ui.apps/src/main/content/jcr_root/apps/core/fd/components/formsportal)


### 草稿和提交

制作现有组件的副本(例如 `button`)，并将其命名为 _草稿和提交_.
![草稿和提交](assets/forms-portal-components2.png)
替换中的内容 `.content.xml` 与以下XML一起使用：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Drafts And Submissions"
          sling:resourceSuperType="core/fd/components/formsportal/draftsandsubmissions/v1/draftsandsubmissions"
          componentGroup="BankingApplication - Content"/>
```

### 搜索和列表程序

制作按钮组件的副本并将其重命名为 _searchandlister_.
替换中的内容 `.content.xml` 与以下XML一起使用：


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Search And Lister"
          sling:resourceSuperType="core/fd/components/formsportal/searchlister/v1/searchlister"
          componentGroup="BankingApplication - Content"/>
```

### 链接组件

制作按钮组件的副本并将其重命名为 _链接_.
替换中的内容 `.content.xml` 与以下XML一起使用：


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Link to Adaptive Form"
          sling:resourceSuperType="core/fd/components/formsportal/link/v2/link"
          componentGroup="BankingApplication - Content"/>
```

部署项目后，您应该能够在您的AEM页面中使用这些组件来创建Forms门户。
