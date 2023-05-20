---
title: 搭配AEM Forms工作流程使用LDAP
description: 指派AEM Forms工作流程任務給提交者的管理員
feature: Adaptive Forms, Workflow
topic: Integrations
role: Developer
version: 6.4,6.5
level: Intermediate
exl-id: 2e9754ff-49fe-4260-b911-796bcc4fd266
last-substantial-update: 2021-09-18T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 0%

---

# 搭配AEM Forms工作流程使用LDAP

指派AEM Forms工作流程任務給提交者的管理員。

在AEM工作流程中使用最適化表單時，您會想要動態地將任務指派給表單提交者的管理員。 為了完成此使用案例，我們必須使用Ldap設定AEM。

有關使用LDAP設定AEM所需步驟的說明，請參閱 [詳細資訊。](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html)

出於本文的目的，我附加了使用Adobe Ldap設定AEM時使用的設定檔案。 這些檔案包含在套件中，可使用套件管理員匯入這些檔案。

在下方熒幕擷圖中，我們將擷取屬於特定成本中心的所有使用者。 如果您想要擷取LDAP中的所有使用者，則不能使用額外的篩選器。

![LDAP設定](assets/costcenterldap.gif)

在下方熒幕擷圖中，我們將群組指派給從LDAP擷取到AEM的使用者。 請注意指派給匯入使用者的表單 — 使用者群組。 使用者必須是此群組的成員，才能與AEM Forms互動。 我們也會將管理員屬性儲存在AEM中的設定檔/管理員節點下。

![Synchandler](assets/synchandler.gif)

設定LDAP並將使用者匯入AEM後，我們就可以建立工作流程，將任務指派給提交者的管理員。 為了撰寫本文章的目的，我們已開發一個簡單的單步驟核准工作流程。

工作流程的第一步是將initialstep的值設定為「否」。 最適化表單中的商業規則將停用「提交者詳細資料」面板，並根據初始步驟值顯示「核准者」面板。

第二個步驟會將任務指派給提交者的管理員。 我們會使用自訂程式碼來取得提交者的管理員。

![分配任务](assets/assigntask.gif)

```java
public String getParticipant(WorkItem workItem, WorkflowSession wfSession, MetaDataMap arg2) throws WorkflowException{
resourceResolver = wfSession.adaptTo(ResourceResolver.class);
UserManager userManager = resourceResolver.adaptTo(UserManager.class);
Authorizable workflowInitiator = userManager.getAuthorizable(workItem.getWorkflow().getInitiator());
.
.
String managerPorperty = workflowInitiator.getProperty("profile/manager")[0].getString();
.
.

}
```

程式碼片段負責擷取管理員ID並將任務指派給管理員。

我們掌握啟動工作流程的人員。 然後我們取得管理員屬性的值。

視管理員屬性儲存在LDAP中的方式而定，您可能必須執行一些字串操作才能取得管理員ID。

請閱讀本文章以實作您自己的 [  參與者選擇器。](https://helpx.adobe.com/experience-manager/using/dynamic-steps.html)

要在您的系統上測試此專案(對於Adobe員工，您可以立即使用此範例)

* [下載和部署setvalue套件組合](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). 這是用於設定管理員屬性的自訂OSGI套件組合。
* [下載並安裝DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [使用封裝管理程式，將與本文相關的資產匯入AEM](assets/aem-forms-ldap.zip).LDAP組態檔、工作流程以及最適化表單都包含在此套件中。
* 使用適當的LDAP認證設定AEM與LDAP。
* 使用您的LDAP憑證登入AEM。
* 開啟 [timeoffrequestform](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* 填寫表單並提交。
* 提交者的經理應取得表單以供檢閱。

>[!NOTE]
>
>此用於擷取管理員名稱的自訂程式碼已針對AdobeLDAP進行測試。 如果您要針對不同的LDAP執行此程式碼，您必須修改或撰寫您自己的getParticipant實作才能取得管理員名稱。
