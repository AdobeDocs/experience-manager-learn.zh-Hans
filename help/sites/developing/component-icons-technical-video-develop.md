---
title: 在Adobe Experience Manager Sites中自定义组件图标
description: 组件图标允许作者使用图标或有意义的缩写快速标识组件。 现在，作者可以比以往更快地找到构建其Web体验所需的组件。
topics: components
audience: administrator, developer
doc-type: technical video
activity: develop
version: 6.3, 6.4, 6.5
feature: Core Components
topic: Development
role: Business Practitioner
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 2%

---


# 自定义组件图标{#developing-component-icons-in-aem-sites}

组件图标允许作者使用图标或有意义的缩写快速标识组件。 现在，作者可以比以往更快地找到构建其Web体验所需的组件。

>[!VIDEO](https://video.tv.adobe.com/v/16778/?quality=9&learn=on)

组件浏览器现在以一致的灰色主题显示，其中显示：

* **[!UICONTROL 组件组]**
* **[!UICONTROL 组件标题]**
* **[!UICONTROL 组件描述]**
* **[!UICONTROL 组件图标]**
   * 组件标题&#x200B;*（默认）*&#x200B;的前两个字母
   * 自定义PNG图像&#x200B;*（由开发人员配置）*
   * 自定义SVG图像&#x200B;*（由开发人员配置）*
   * CoralUI图标&#x200B;*（由开发人员配置）*

## 组件图标配置选项{#component-icon-configuration-options}

### 缩写{#abbreviations}

默认情况下，组件标题(**[cq:Component]@jcr:title**)的前2个字符用作缩写。 例如，如果&#x200B;**[cq:Component]@jcr:title=Article 列表**，则缩写将显示为“**Ar**”。

可以通过&#x200B;**[cq:Component]@abbreviation**&#x200B;属性自定义缩写。 虽然此值可以接受2个以上的字符，但建议将缩写限制为2个字符，以避免任何视觉干扰。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### CoralUI图标{#coralui-icons}

AEM提供的CoralUI图标可用于组件图标。 要配置CoralUI图标，请将&#x200B;**[cq:Component]@cq:icon**&#x200B;属性设置为所需CoralUI图标的HTML图标属性值（在[CoralUI文档](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)中枚举）。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### PNG图像{#png-images}

PNG图像可用于组件图标。 要将PNG图像配置为组件图标，请在&#x200B;**[cq:Component]**&#x200B;下将所需图像添加为名为&#x200B;**** cq:icon.png **的nt:file**。

PNG应具有透明背景或设置为&#x200B;**#707070**&#x200B;的背景颜色。

PNG图像将按20px **缩放到** 20px。 但是，最好通过&#x200B;**40px**&#x200B;容纳视网膜显示屏&#x200B;**40px**。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### SVG图像{#svg-images}

SVG图像（基于矢量）可用于组件图标。 要将SVG图像配置为组件图标，请在&#x200B;**[cq:Component]**&#x200B;下将所需的SVG添加为名为&#x200B;****&#x200B;的&#x200B;**cq:icon.svg**&#x200B;的nt:file。

SVG图像应将背景颜色设置为&#x200B;**#707070**，大小应为&#x200B;**20px x 20px。**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## 其他资源 {#additional-resources}

* [可用的CoralUI图标](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
