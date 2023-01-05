---
title: 响应式断点
description: 了解如何为AEM响应式页面编辑器配置新的响应式断点。
version: Cloud Service
feature: Page Editor
topic: Mobile, Development
role: Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-01-05T00:00:00Z
kt: 11664
thumbnail: kt-11664.jpeg
source-git-commit: c82965636ddeef7dc165e0bea079c99f1a16e0ca
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---


# 响应式断点

了解如何为AEM响应式页面编辑器配置新的响应式断点。

## 创建CSS断点

首先，在响应式AEM网站遵循的AEM响应式网格CSS中创建媒体断点。

在 `/ui.apps/src/main/content/jcr_root/apps/[app name]/clientlibs/clientlib-grid/less/grid.less` ，请创建要与移动模拟器一起使用的断点。 记下 `max-width` 对于每个断点，因为这会将CSS断点映射到AEM响应式页面编辑器断点。

![创建新的响应式断点](./assets/responsive-breakpoints/create-new-breakpoints.jpg)

## 自定义模板的断点

打开 `ui.content/src/main/content/jcr_root/conf/<app name>/settings/wcm/templates/page-content/structure/.content.xml` 文件和更新 `cq:responsive/breakpoints` 定义断点节点。 每个 [CSS断点](#create-new-css-breakpoints) 应在下拥有相应的节点 `breakpoints` 使用 `width` 属性设置为CSS断点的 `max-width`.

![自定义模板的响应式断点](./assets/responsive-breakpoints/customize-template-breakpoints.jpg)

## 创建模拟器

必须定义AEM模拟器，以允许作者在页面编辑器中选择要编辑的响应式视图。

在下创建模拟器节点 `/ui.apps/src/main/content/jcr_root/apps/<app name>/emulators`

例如：`/ui.apps/src/main/content/jcr_root/apps/wknd-examples/emulators/phone-landscape`。从复制引用模拟器节点 `/libs/wcm/mobile/components/emulators` CRXDE Lite并更新副本，以加快节点定义。

![创建新模拟器](./assets/responsive-breakpoints/create-new-emulators.jpg)

## 创建设备组

将模拟器分组到 [在AEM页面编辑器中提供](#update-the-templates-device-group).

创建 `/apps/settings/mobile/groups/<name of device group>` 节点结构 `/ui.apps/src/main/content/jcr_root`.

![创建新设备组](./assets/responsive-breakpoints/create-new-device-group.jpg)

创建 `.content.xml` 文件 `/apps/settings/mobile/groups/<device group name>` 使用与以下类似的代码定义新模拟器：

![创建新设备](./assets/responsive-breakpoints/create-new-device.jpg)

## 更新模板的设备组

最后，将设备组映射回页面模板，以便在页面编辑器中为从此模板创建的页面提供模拟器。

打开 `ui.content/src/main/content/jcr_root/conf/[app name]/settings/wcm/templates/page-content/structure/.content.xml` 文件并更新 `cq:deviceGroups` 引用新移动设备组的属性(例如， `cq:deviceGroups="[mobile/groups/customdevices]"`)
