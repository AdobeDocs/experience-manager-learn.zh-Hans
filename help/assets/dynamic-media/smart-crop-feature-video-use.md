---
title: 搭配AEM Assets Dynamic Media使用智慧型裁切
description: 智慧型裁切使用Adobe Sensei來消除針對回應式設計裁切內容所費時費力的工作。
feature: Smart Crop, Image Profiles
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 295bbfb6-241f-41c0-972d-d9688863cea1
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 2%

---

# 搭配AEM Assets Dynamic Media使用智慧型裁切{#using-smart-crop-with-aem-assets-dynamic-media}

智慧型裁切使用Adobe Sensei來消除針對回應式設計裁切內容所費時費力的工作。

>[!VIDEO](https://video.tv.adobe.com/v/21519?quality=12&learn=on)

>[!NOTE]
>
>影片假設您的AEM執行個體在Dynamic Media S7模式下執行。 [有關使用Dynamic Media設定AEM的指示，請參閱此處。](https://helpx.adobe.com/cn/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Adobe Experience Manager的Dynamic Media智慧型裁切功能包括

* AEM資產管理員可以根據裝置寬度和高度，輕鬆建立智慧型裁切的影像設定檔。
* 可對個別資產執行智慧型裁切，或可對資料夾內的所有資產執行智慧型裁切。
* 智慧型裁切編輯版面可以調整大小，以提升可見度。
* AEM Sites的Dynamic Media元件支援智慧型裁切。
* 智慧型裁切資產的已發佈URL可用於接受託管資產的第三方應用程式。

>[!NOTE]
>
>智慧型裁切座標會與外觀比例相關。 也就是說，針對影像設定檔中的各種智慧型裁切設定，如果影像設定檔中新增維度的外觀比例相同，則會將相同的外觀比例傳送至Dynamic Media。 因此，建議在智慧型裁切編輯器中使用相同的裁切區域。 例如，裁切設定100x100和200x200會導致系統產生相同的智慧型裁切。
