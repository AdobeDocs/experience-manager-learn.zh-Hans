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
jira: KT-11664
thumbnail: kt-11664.jpeg
exl-id: 8b48c28f-ba7f-4255-be96-a7ce18ca208b
duration: 82
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---

# 响应式断点

了解如何为AEM响应式页面编辑器配置新的响应式断点。

## 创建CSS断点

首先，在AEM响应式网格CSS中创建响应式AEM站点所遵循的媒体断点。

在 `/ui.apps/src/main/content/jcr_root/apps/[app name]/clientlibs/clientlib-grid/less/grid.less` 文件，创建要与移动设备模拟器一起使用的断点。 记下 `max-width` 对于每个断点，因为这会将CSS断点映射到AEM响应式页面编辑器断点。

![创建新的响应式断点](./assets/responsive-breakpoints/create-new-breakpoints.jpg)

## 自定义模板的断点

打开 `ui.content/src/main/content/jcr_root/conf/<app name>/settings/wcm/templates/page-content/structure/.content.xml` 文件和更新 `cq:responsive/breakpoints` 与新断点节点定义一起使用。 每个 [CSS断点](#create-new-css-breakpoints) 下应该有对应的节点 `breakpoints` 及其 `width` 属性设置为CSS断点的 `max-width`.

![自定义模板的响应式断点](./assets/responsive-breakpoints/customize-template-breakpoints.jpg)

## 创建模拟器

必须定义AEM模拟器，以允许作者在页面编辑器中选择要编辑的响应式视图。

在下创建模拟器节点 `/ui.apps/src/main/content/jcr_root/apps/<app name>/emulators`

例如，`/ui.apps/src/main/content/jcr_root/apps/wknd-examples/emulators/phone-landscape`。从以下位置复制引用模拟器节点 `/libs/wcm/mobile/components/emulators` CRXDE Lite并更新副本，以加快节点定义。

![创建新模拟器](./assets/responsive-breakpoints/create-new-emulators.jpg)

## 创建设备组

将模拟器分组到 [使其在AEM页面编辑器中可用](#update-the-templates-device-group).

创建 `/apps/settings/mobile/groups/<name of device group>` 下的节点结构 `/ui.apps/src/main/content/jcr_root`.

![创建新设备组](./assets/responsive-breakpoints/create-new-device-group.jpg)

创建 `.content.xml` 文件位置 `/apps/settings/mobile/groups/<device group name>` 并使用与以下内容类似的代码定义新模拟器：

![创建新设备](./assets/responsive-breakpoints/create-new-device.jpg)

## 更新模板的设备组

最后，将设备组映射回页面模板，以便可以在页面编辑器中为使用此模板创建的页面使用模拟器。

打开 `ui.content/src/main/content/jcr_root/conf/[app name]/settings/wcm/templates/page-content/structure/.content.xml` 文件并更新 `cq:deviceGroups` 属性以引用新的移动设备组(例如， `cq:deviceGroups="[mobile/groups/customdevices]"`)
