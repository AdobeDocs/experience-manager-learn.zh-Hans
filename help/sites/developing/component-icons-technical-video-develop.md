---
title: 在Adobe Experience Manager Sites中自定义组件图标
description: 组件图标允许作者通过图标或有意义的缩写快速识别组件。 现在，作者可以比以往更快地找到构建其Web体验所需的组件。
topics: components
audience: administrator, developer
doc-type: technical video
activity: develop
version: 6.4, 6.5
feature: Core Components
topic: Development
role: User
level: Intermediate
exl-id: 37dc26aa-0773-4749-8c8b-4544bd4d5e5f
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 1%

---

# 自定义组件图标 {#developing-component-icons-in-aem-sites}

组件图标允许作者通过图标或有意义的缩写快速识别组件。 现在，作者可以比以往更快地找到构建其Web体验所需的组件。

>[!VIDEO](https://video.tv.adobe.com/v/16778/?quality=9&learn=on)

组件浏览器现在以一致的灰色主题显示，其中显示：

* **[!UICONTROL 组件组]**
* **[!UICONTROL 组件标题]**
* **[!UICONTROL 组件描述]**
* **[!UICONTROL 组件图标]**
   * 组件标题的前两个字母 *（默认）*
   * 自定义PNG图像 *（由开发人员配置）*
   * 自定义SVG图像 *（由开发人员配置）*
   * CoralUI图标 *（由开发人员配置）*

## 组件图标配置选项 {#component-icon-configuration-options}

### 缩写 {#abbreviations}

默认情况下，组件标题的前2个字符(**[cq：组件]@jcr:title**)用作缩写。 例如，如果 **[cq：组件]@jcr:title=Article List** 缩写将显示为“**Ar**&quot;

缩写可通过 **[cq：组件]@abbreviation** 属性。 虽然此值可以接受2个字符以上的字符，但建议将缩写限制为2个字符，以避免任何视觉干扰。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### CoralUI图标 {#coralui-icons}

AEM提供的CoralUI图标可用于组件图标。 要配置CoralUI图标，请设置 **[cq：组件]@cq:icon** 属性，以获取所需CoralUI图标的HTML图标属性值(在 [CoralUI文档](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### PNG图像 {#png-images}

PNG图像可用于组件图标。 要将PNG图像配置为组件图标，请将所需的图像添加为 **nt:file** 已命名 **cq:icon.png** 下 **[cq：组件]**.

PNG应具有透明背景或背景颜色设置为 **#707070**.

PNG图像会缩放到 **20px x 20px**. 但是，要适应视网膜显示器 **40px** by **40px** 可能更好。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### SVG图像 {#svg-images}

SVG图像（基于矢量）可用于组件图标。 要将SVG图像配置为组件图标，请将所需的SVG添加为 **nt:file** 已命名 **cq:icon.svg** 下 **[cq：组件]**.

SVG图像应将背景颜色设置为 **#707070** 和 **20px x 20px。**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## 其他资源 {#additional-resources}

* [可用的CoralUI图标](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
