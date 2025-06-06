---
title: 创建可单击的图像组件
description: 在AEM Forms as a Cloud Service中创建可单击的图像组件。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15968
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: c451472f-d282-4662-9852-8a3e73c5c853
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# 可单击图像简介

在Forms中使用可单击的图像，可以打造一种更加引人入胜、直观且美观的用户体验。 在本文中，我们将SVG用于可点击图像，因为它提供了几个优势，尤其是在设计灵活性、性能和用户体验方面。
可以使用Adobe Illustrator或任何免费在线工具创建SVG。 我已使用来自[&#128279;](https://simplemaps.com/resources/svg-us)简单映射的USA映射来演示用例。

## 使用可单击的“美国地图”的用例

美国的可点击地图允许用户浏览特定于状态的表单提交。 当用户单击状态时，将列出来自该状态的提交内容，并且可以选择打开特定的提交内容。
