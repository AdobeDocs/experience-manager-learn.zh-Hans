---
title: 在Adobe Experience Manager Sites自定义组件图标
description: 组件图标允许作者使用图标或有意义的缩写快速识别组件。 现在，作者可以比以往更快地找到构建其Web体验所需的组件。
topics: components
audience: administrator, developer
doc-type: technical video
activity: develop
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: c85a59a8bd180d5affe2a5bf5939dabfb2776d73
workflow-type: tm+mt
source-wordcount: '375'
ht-degree: 1%

---


# 自定义组件图标 {#developing-component-icons-in-aem-sites}

组件图标允许作者使用图标或有意义的缩写快速识别组件。 现在，作者可以比以往更快地找到构建其Web体验所需的组件。

>[!VIDEO](https://video.tv.adobe.com/v/16778/?quality=9&learn=on)

组件浏览器现在以一致的灰色主题显示，其中显示：

* **[!UICONTROL 组件组]**
* **[!UICONTROL 组件标题]**
* **[!UICONTROL 组件描述]**
* **[!UICONTROL 组件图标]**
   * 组件标题的前两个字 *母（默认）*
   * 自定义PNG *图像（由开发人员配置）*
   * 自定义SVG *图像（由开发人员配置）*
   * CoralUI图 *标（由开发人员配置）*

## 组件图标配置选项 {#component-icon-configuration-options}

### 缩写 {#abbreviations}

默认情况下，组件标题的前2个&#x200B;**[字符(cq:]Component@jcr:title**)用作缩写。 例如，如 **[果cq:Component]@jcr:title=Article列表** ，缩写将显示为“**Ar**”。

可以通过cq:Component@ **[abbreation属性自定义]缩写** 。 虽然此值可以接受2个以上的字符，但建议将缩写限制为2个字符，以避免任何视觉干扰。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### CoralUI图标 {#coralui-icons}

AEM提供的CoralUI图标可用于组件图标。 要配置CoralUI图标，请将 **[cq:Component]@cq:icon属性设置为所需CoralUI图标的HTML图标属性值(在CoralUI文档** 中枚举 [](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### PNG图像 {#png-images}

PNG图像可用于组件图标。 要将PNG图像配置为组件图标，请在cq:Component下将所 **需图像添加为名** 为 **cq:icon** .png的 **[nt:file]**。

PNG应具有透明背景或背景颜色设置为 **#707070**。

PNG图像将按20px **缩放为20px**。 但是，最好将 **Retina显示屏****调整为40px** x 40px。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### SVG图像 {#svg-images}

SVG图像（基于矢量）可用于组件图标。 要将SVG图像配置为组件图标，请将所需的SVG添 **加为cq** : **component下名** 为 **[cq:icon.]** svg的nt:file。

SVG图像的背景颜色应设 **置为#707070** ，大小 **应设置为20px x 20px。**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## 其他资源 {#additional-resources}

* [可用的CoralUI图标](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
