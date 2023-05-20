---
title: 取得請求引數
description: 使用表單資料模型的預填服務存取請求引數
feature: Adaptive Forms
version: 6.4,6.5
kt: 5815
thumbnail: kt-5815.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a640539d-c67f-4224-ad81-dd0b62e18c79
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 3%

---

# 取得請求引數

## 取得empID引數

下一步是從url存取empID引數。 然後empID要求引數的值會傳遞至 **_get_** 表單資料模型的服務操作。
出於本課程的目的，我們建立了並提供以下內容

* 調適型表單範本已呼叫 **_FDMDemo_**
* 呼叫的頁面元件 **_fdmdemo_**
* 已將我們的自訂jsp與頁面元件包括在內
* 將最適化表單範本與頁面元件相關聯

藉由執行此操作，我們的自訂jsp中的程式碼只有在基於此自訂範本的最適化表單轉譯時才會執行

* [匯入套件](assets/template-page-component.zip) 使用 [封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)
* [開啟fdmrequest.jsp](http://localhost:4502/crx/de/index.jsp#/apps/fdmdemo/component/page/fdmdemo/fdmrequest.jsp)
* 取消註解註解的行。
* 儲存您的變更

```java
if(request.getParameter("empID")!=null)
    {
      //System.out.println("Adobe !!!There is a empID parameter in the request "+request.getParameter("empID"));
      //java.util.Map paraMap = new java.util.HashMap();
      //paraMap.put("empID",request.getParameter("empID"));
      //slingRequest.setAttribute("paramMap",paraMap);
    }
```

empID的值與paraMap中稱為empID的索引鍵相關聯。 然後，此對應會傳遞至slingRequest

>[!NOTE]
>
>金鑰empID必須與newhire實體取得服務的繫結值相符

## 后续步骤

[根據表單資料模型建立最適化表單](./create-adaptive-form.md)
