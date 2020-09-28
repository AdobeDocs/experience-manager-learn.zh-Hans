---
title: 获取请求参数
description: 访问表单数据模型的预填服务的请求参数
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5815
thumbnail: kt-5815.jpg
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 0%

---

# 获取请求参数

## 获取empID参数

下一步是从url访问empID参数。 然后，empID请求参数的值被传递到表 **_单数据_** 模型的get服务操作。
为了本课程的目的，我们创建并提供了以下内容

* 自适应表单模板名为 **_FDMDemo_**
* 名为fdmdemo的页面组 **_件_**
* 将我们的自定义jsp包含在页面组件中
* 将自适应表单模板与页面组件关联

通过执行此操作，只有在渲染基于此自定义模板的自适应表单时，才会执行自定义jsp中的代码

* [使用包管理器](assets/template-page-component.zip) 导 [入包](http://localhost:4502/crx/packmgr/index.jsp)
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

empID的值与paraMap中名为empID的键相关联。 然后，此映射将传递给slingRequest

>[!NOTE]
>
>密钥empID必须与新实体获取服务的绑定值相匹配
