---
title: 使用AEM中的系統概觀儀表板
description: 在舊版AEM中，管理員需要檢視多個位置，才能完整瞭解AEM執行個體。 「系統概覽」旨在透過單一儀表板提供AEM執行個體的設定、硬體和健康情況的高層級檢視，以解決此問題。
version: 6.4, 6.5
topics: administration, operations, monitoring
activity: use
audience: administrator, architect, developer, implementer
doc-type: technical video
contentOwner: dgordon
topic: Administration
role: Admin
level: Beginner
exl-id: af8f499c-4955-44b5-8f21-085263ca31a3
source-git-commit: 4b47daf82e27f6bea4be30e3cdd132f497f4c609
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---

# 使用系統概觀儀表板

Adobe Experience Manager (AEM) [!UICONTROL 系統概觀] 從單一儀表板提供AEM執行個體的組態、硬體及健康情況的高層級檢視。

>[!VIDEO](https://video.tv.adobe.com/v/21340?quality=12&learn=on)

1. 系統概觀可從下列位置存取： **AEM開始** > **[!UICONTROL 工具]** > **[!UICONTROL 作業]** > **[!UICONTROL 系統概觀]**

   直接在 **`<server-host>/libs/granite/operations/content/systemoverview.html`**

1. 資訊來自 [!UICONTROL 系統概觀] 可透過按一下 [!UICONTROL 下載] 按鈕。 此資訊也會透過下列方式公開 [!DNL REST] 端點：
1. 以下是從匯出的JSON輸出範例 [!UICONTROL 系統概觀]：

   ```json
   {
       "Health Checks": {
           "1 Critical": "System Maintenance",
           "3 Warn": "Replication Queue, Log Errors, Sling/Granite Content Access Check"
       },
       "Instance": {
           "Adobe Experience Manager": "6.4.0",
           "Run Modes": "s7connect, crx3, non-composite, author, samplecontent, crx3tar",
           "Instance Up Since": "2018-01-22 10:50:37"
       },
       "Repository": {
           "Apache Jackrabbit Oak": "1.8.0",
           "Node Store": "Segment Tar",
           "Repository Size": "0.26 GB",
           "File Data Store": "crx-quickstart/repository/datastore"
       },
       "Maintenance Tasks": {
           "Failed": "AuditLog Maintenance Task, Project Purge, Workflow Purge",
           "Succeeded": "Data Store Garbage Collection, Lucene Binaries Cleanup, Revision Clean Up, Version Purge, Purge of ad-hoc tasks"
       },
       "System Information": {
           "Mac OS X": "10.12.6",
           "System Load Average": "2.29",
           "Usable Disk Space": "89.44 GB",
           "Maximum Heap": "0.97 GB"
       },
       "Estimated Node Counts": {
           "Total": "232448",
           "Tags": "62",
           "Assets": "267",
           "Authorizables": "210",
           "Pages": "1592"
       },
       "Replication Agents": {
           "Blocked": "publish, publish2",
           "Idle": "offloading_b3deb296-9b28-4f60-8587-c06afa2e632c, offloading_outbox, offloading_reverse_b3deb296-9b28-4f60-8587-c06afa2e632c, publish_reverse, scene7, screens, screens2, test_and_target"
       },
       "Distribution Agents": {
           "Blocked": "publish"
       },
       "Workflows": {
           "Running Workflows": "15",
           "Failed Workflows": "15",
           "Failed Jobs": "150"
       },
       "Sling Jobs": {
           "Failed": "305"
       }
   }
   ```
