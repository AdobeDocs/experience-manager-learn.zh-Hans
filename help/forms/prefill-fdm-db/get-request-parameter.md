---
title: 获取请求参数
description: 访问表单数据模型的预填充服务的请求参数
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-5815
thumbnail: kt-5815.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a640539d-c67f-4224-ad81-dd0b62e18c79
duration: 49
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '176'
ht-degree: 1%

---

# 获取请求参数

## 获取empID参数

下一步是从url访问empID参数。 随后，empID请求参数的值将传递到 **_get_** 表单数据模型的服务操作。
出于本课程的目的，我们创建并提供了以下内容

* 调用的自适应表单模板 **_FDMDemo_**
* 已调用的页面组件 **_fdmdemo_**
* 已将我们的自定义jsp与页面组件包括在内
* 已将自适应表单模板与页面组件关联

这样一来，我们的自定义jsp中的代码仅在基于此自定义模板的自适应表单呈现时执行

* [导入资源包](assets/template-page-component.zip) 使用 [包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* [打开fdmrequest.jsp](http://localhost:4502/crx/de/index.jsp#/apps/fdmdemo/component/page/fdmdemo/fdmrequest.jsp)
* 取消注释已注释的行。
* 保存更改

```java
if(request.getParameter("empID")!=null)
    {
      //System.out.println("Adobe !!!There is a empID parameter in the request "+request.getParameter("empID"));
      //java.util.Map paraMap = new java.util.HashMap();
      //paraMap.put("empID",request.getParameter("empID"));
      //slingRequest.setAttribute("paramMap",paraMap);
    }
```

empID的值与paraMap中名为empID的键关联。 然后，此映射将传递到slingRequest

>[!NOTE]
>
>密钥empID必须与新实体get服务的绑定值匹配

## 后续步骤

[根据表单数据模型创建自适应表单](./create-adaptive-form.md)
