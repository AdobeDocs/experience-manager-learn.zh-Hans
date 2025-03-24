---
title: 创建新页面组件
description: 创建新页面组件
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-13717
exl-id: 7469aa7f-1794-40dd-990c-af5d45e85223
duration: 67
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 8%

---

# 页面组件

页面组件是负责呈现页面的常规组件。 我们将创建一个新的页面组件，并将该页面组件与新的自适应表单模板相关联。 这可确保仅在自适应表单基于此特定模板时执行我们的代码。

## 创建页面组件

登录到本地云就绪的AEM Forms实例。 在apps文件夹下创建以下结构
![页面组件](./assets/page-component1.png)

1. 右键单击页面文件夹并创建一个名为storedfetch的节点，类型为cq：Component
1. 保存更改
1. 将以下属性添加到`storeandfetch`节点并保存

| **属性名称** | **属性类型** | **属性值** |
|-------------------------|-------------------|----------------------------------------|
| 组件组 | 字符串 | 隐藏 |
| jcr：description | 字符串 | 自适应表单模板页面类型 |
| jcr:title | 字符串 | 自适应表单模板页面 |
| sling:resourceSuperType | 字符串 | `fd/af/components/page2/aftemplatedpage` |

复制`/libs/fd/af/components/page2/aftemplatedpage/aftemplatedpage.jsp`并将其粘贴到`storeandfetch`节点下。 将`aftemplatedpage.jsp`重命名为`storeandfetch.jsp`。

打开`storeandfetch.jsp`并添加以下行：

```jsp
<cq:include script="azureportal.jsp"/>
```

在

```jsp
<cq:include script="fallbackLibrary.jsp"/>
```

最终代码应如下所示

```jsp
<cq:include script="fallbackLibrary.jsp"/>
<cq:include script="azureportal.jsp"/>
```

在storeandfetch节点下创建名为azureportal.jsp的文件
将以下代码复制到azureportal.jsp中并保存更改

```jsp
<%@page session="false" %>
<%@include file="/libs/fd/af/components/guidesglobal.jsp" %>
<%@ page import="org.apache.commons.logging.Log" %>
<%@ page import="org.apache.commons.logging.LogFactory" %>
<%
    if(request.getParameter("guid")!=null) {
            logger.debug( "Got Guid in the request" );
            String BlobId = request.getParameter("guid");
            java.util.Map paraMap = new java.util.HashMap();
            paraMap.put("BlobId",BlobId);
            slingRequest.setAttribute("paramMap",paraMap);
    } else {
            logger.debug( "There is no Guid in the request " );
    }            
%>
```

在此代码中，我们获取请求参数&#x200B;**guid**&#x200B;的值，并将其存储在名为BlobId的变量中。 然后，使用paramMap属性将此BlobId传递到sling请求中。 为使此代码正常工作，假定您有一个基于Azure Storage支持的表单数据模型的表单，并且该表单数据模型的读取服务绑定到名为BlobId的请求属性，如下面的屏幕快照所示。

![fdm-request-attribute](./assets/fdm-request-attribute.png)

### 后续步骤

[将页面组件与模板关联](./associate-page-component.md)
