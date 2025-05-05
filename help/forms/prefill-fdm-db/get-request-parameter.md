---
title: 获取请求参数
description: 访问表单数据模型的预填充服务的请求参数
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-5815
thumbnail: kt-5815.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a640539d-c67f-4224-ad81-dd0b62e18c79
duration: 40
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '176'
ht-degree: 1%

---

# 获取请求参数

## 获取empID参数

下一步是从url访问empID参数。 然后，empID请求参数的值被传递到表单数据模型的&#x200B;**_get_**&#x200B;服务操作。
出于本课程的目的，我们创建并提供了以下内容

* 名为&#x200B;**_FDMDemo_**&#x200B;的自适应表单模板
* 名为&#x200B;**_fdmdemo_**&#x200B;的页面组件
* 已将我们的自定义jsp与页面组件包括在内
* 已将自适应表单模板与页面组件关联

这样一来，我们的自定义jsp中的代码仅在基于此自定义模板的自适应表单呈现时执行

* [&#128279;](assets/template-page-component.zip)使用[包管理器](http://localhost:4502/crx/packmgr/index.jsp)导入包
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
