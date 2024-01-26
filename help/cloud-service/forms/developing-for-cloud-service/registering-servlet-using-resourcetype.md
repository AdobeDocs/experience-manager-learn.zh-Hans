---
title: 使用资源类型注册servlet
description: 在AEM Forms CS中将servlet映射到资源类型
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Developer Tools
jira: KT-14581
duration: 108
exl-id: 2a33a9a9-1eef-425d-aec5-465030ee9b74
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 2%

---

# 简介

与按资源类型绑定相比，按路径绑定servlet有几个缺点，即：

* 无法使用默认JCR存储库ACL来控制路径绑定的servlet的访问
* 路径绑定的servlet只能注册到路径，而不能注册到资源类型（即没有后缀处理）
* 如果路径绑定的servlet不是活动的（例如，如果捆绑丢失或未启动），POST可能会导致意外结果。 通常创建节点 `/bin/xyz` 对于仅查看存储库的开发人员，由于存在这些缺点，强烈建议将servlet绑定到资源类型而不是路径

## 创建Servlet

在IntelliJ中启动您的aem-banking项目。 在servlet文件夹下创建一个名为GetFieldChoices的servlet，如下面的屏幕快照所示。
![选项](assets/fetchchoices.png)

## 示例Servlet

以下servlet绑定到Sling资源类型： _**azure/fetchchoices**_



```java
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.servlets.annotations.SlingServletResourceTypes;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.jcr.Session;
import javax.servlet.Servlet;
import java.io.IOException;
import java.io.Serializable;

@Component(
        service={Servlet.class }
)

        @SlingServletResourceTypes(
                resourceTypes="azure/fetchchoices",
                methods= "GET",
                extensions="json"
                )


public class GetFieldChoices extends SlingAllMethodsServlet implements Serializable {
    private static final long serialVersionUID = 1L;
    private final  transient Logger log = LoggerFactory.getLogger(this.getClass());


   

    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {

        log.debug("The form path I got was "+request.getParameter("formPath"));

    }
}
```

## 在CRX中创建资源

* 登录到本地AEM SDK。
* 创建名为的资源 `fetchchoices` （您可以根据需要命名此节点） `cq:Page` 在内容节点下。
* 保存更改
* 创建名为的节点 `jcr:content` 类型 `cq:PageContent` 并保存更改
* 将以下属性添加到 `jcr:content` 节点

| 属性名称 | 属性值 |
|--------------------|--------------------|
| jcr:title | 实用程序Servlet |
| sling:resourceType | `azure/fetchchoices` |


此 `sling:resourceType` 值必须与servlet中指定的resourceTypes=&quot;azure/fetchchchoices匹配。

您现在可以通过请求资源来调用servlet `sling:resourceType` = `azure/fetchchoices` 在其完整路径上，在Sling Servlet中注册了任何选择器或扩展。

```html
http://localhost:4502/content/fetchchoices/jcr:content.json?formPath=/content/forms/af/forrahul/jcr:content/guideContainer
```

路径 `/content/fetchchoices/jcr:content` 是资源和扩展的路径 `.json` 是servlet中指定的内容

## 同步您的AEM项目

1. 在您喜爱的编辑器中打开AEM项目。 我用IntelliJ做这个。
1. 创建名为的文件夹 `fetchchoices` 下 `\aem-banking-application\ui.content\src\main\content\jcr_root\content`
1. 右键单击 `fetchchoices` 文件夹并选择 `repo | Get Command` （此菜单项在本教程的上一章中进行设置）。

这应该会将此节点从AEM同步到您的本地AEM项目。

您的AEM项目结构应如下所示
![resource-resolver](assets/mapping-servlet-resource.png)
使用以下条目更新aem-banking-application\ui.content\src\main\content\META-INF\vault文件夹中的filter.xml

```xml
<filter root="/content/fetchchoices" mode="merge"/>
```

您现在可以使用Cloud Manager将更改推送到AEMas a Cloud Service环境。

## 后续步骤

[启用Forms Portal组件](./forms-portal-components.md)
