---
title: 在Adobe Experience Manager Sites中自定义组件图标
description: 组件图标允许作者使用图标或有意义的缩写快速识别组件。 作者现在可以找到比以往更快地构建其Web体验所需的组件。
version: 6.4, 6.5
feature: Core Components
topic: Development
role: User
level: Intermediate
doc-type: Technical Video
exl-id: 37dc26aa-0773-4749-8c8b-4544bd4d5e5f
duration: 411
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 0%

---

# 自定义组件图标 {#developing-component-icons-in-aem-sites}

组件图标允许作者使用图标或有意义的缩写快速识别组件。 作者现在可以找到比以往更快地构建其Web体验所需的组件。

>[!VIDEO](https://video.tv.adobe.com/v/16778?quality=12&learn=on)

组件浏览器现在以一致的灰色主题显示，其中显示：

* **[!UICONTROL 组件组]**
* **[!UICONTROL 组件标题]**
* **[!UICONTROL 组件说明]**
* **[!UICONTROL 组件图标]**
   * 组件标题的前两个字母 *（默认）*
   * 自定义PNG图像 *（由开发人员配置）*
   * 自定义SVG图像 *（由开发人员配置）*
   * CoralUI图标 *（由开发人员配置）*

## 组件图标配置选项 {#component-icon-configuration-options}

### 缩写 {#abbreviations}

默认情况下，组件标题的前2个字符(**[cq：Component]@jcr：title**)作为缩写。 例如，如果 **[cq：Component]@jcr：title=文章列表** 缩写将显示为“**Ar**“。

缩写可通过以下方式自定义： **[cq：Component]@abbreviation** 属性。 虽然此值可以接受2个以上的字符，但建议将缩写限制为2个字符，以避免任何视觉干扰。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### CoralUI图标 {#coralui-icons}

AEM提供的CoralUI图标可用于组件图标。 要配置CoralUI图标，请设置 **[cq：Component]@cq：icon** 属性到所需的CoralUI图标的HTML图标属性值(枚举于 [CoralUI文档](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### PNG图像 {#png-images}

PNG图像可用于组件图标。 要将PNG图像配置为组件图标，请将所需图像添加为 **nt：file** 已命名 **cq：icon.png** 在 **[cq：Component]**.

PNG应具有透明背景，或将背景颜色设置为 **#707070**.

PNG图像将缩放到 **20px x 20px**. 但是，为了适应视网膜显示 **40像素** 按 **40像素** 也许更可取。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### SVG图像 {#svg-images}

SVG图像（基于矢量）可用于组件图标。 要将SVG图像配置为组件图标，请将所需SVG添加为 **nt：file** 已命名 **cq：icon.svg** 在 **[cq：Component]**.

SVG图像的背景颜色应设置为 **#707070** 大小为 **20px x 20px。**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## 其他资源 {#additional-resources}

* [可用的CoralUI图标](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
