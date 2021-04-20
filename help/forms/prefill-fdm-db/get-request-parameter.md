---
title: 获取请求参数
description: 访问表单数据模型的预填服务的请求参数
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5815
thumbnail: kt-5815.jpg
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 4%

---

# 获取请求参数

## 获取empID参数

下一步是从url访问empID参数。 empID请求参数的值随后被传递到表单数据模型的&#x200B;**_get_**服务操作。
为了本课程的目的，我们创建并提供了以下内容

* 名为&#x200B;**_FDMDemo_**&#x200B;的自适应表单模板
* 名为&#x200B;**_fdmdemo_**&#x200B;的页面组件
* 将我们的自定义jsp包含在页面组件中
* 将自适应表单模板与页面组件关联

通过执行此操作，我们在自定义jsp中的代码只有在渲染基于此自定义模板的自适应表单时才会执行

* [使用包管](assets/template-page-component.zip) 理器 [导入包](http://localhost:4502/crx/packmgr/index.jsp)
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

empID的值与paraMap中名为empID的键关联。 然后，此映射将传递给slingRequest

>[!NOTE]
>
>键empID必须与新实体获取服务的绑定值匹配
