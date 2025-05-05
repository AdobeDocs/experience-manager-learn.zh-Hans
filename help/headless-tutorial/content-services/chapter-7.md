---
title: 第7章 — 从移动设备应用程序使用AEM Content Services - Content Services
description: 本教程的第7章将运行Android移动设备应用程序，以使用来自AEM Content Services的创作内容。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: d6b6d425-842a-43a9-9041-edf78e51d962
duration: 467
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1350'
ht-degree: 0%

---

# 第7章 — 从移动设备应用程序使用AEM Content Services

本教程的第7章使用本机Android移动设备应用程序来使用AEM Content Services中的内容。

## Android移动设备应用程序

本教程使用&#x200B;**简单的本机Android移动设备应用程序**&#x200B;来使用和显示由AEM Content Services公开的事件内容。

[Android](https://developer.android.com/)的使用基本上不重要，消费的移动应用程序可以在任何移动平台的框架中编写，例如iOS。

Android之所以用于教程，是因为它可以在Windows、macOs和Linux上运行Android模拟器，而且它流行于Java，这是AEM开发人员非常了解的一种语言。

*本教程的Android移动设备应用程序&#x200B;**不是**&#x200B;旨在指导如何构建Android移动设备应用程序或传达Android开发最佳实践，而是说明如何从移动设备应用程序使用AEM Content Services。*

### AEM Content Services如何推动移动应用程序体验

![移动应用到Content Services的映射](assets/chapter-7/content-services-mapping.png)

1. 由[!DNL Events API]页面的&#x200B;**图像组件**&#x200B;定义的&#x200B;**徽标**。
1. 在[!DNL Events API]页面的&#x200B;**文本组件**&#x200B;上定义的&#x200B;**标记行**。
1. 此&#x200B;**事件列表**&#x200B;派生自事件内容片段的序列化，通过配置的&#x200B;**内容片段列表组件**&#x200B;公开。

## 移动应用程序演示

>[!VIDEO](https://video.tv.adobe.com/v/28345?quality=12&learn=on)

### 配置移动应用程序以供非本地主机使用

如果未在&#x200B;**http://localhost:4503**&#x200B;上运行AEM Publish，则可以在移动设备应用程序的[!DNL Settings]中更新主机和端口，以指向属性AEM Publish host/port。

>[!VIDEO](https://video.tv.adobe.com/v/28344?quality=12&learn=on)

## 在本地运行移动设备应用程序

1. 下载并安装[Android Studio](https://developer.android.com/studio/install)以安装Android模拟器。
1. **下载** Android [!DNL APK]文件[GitHub > Assets > wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. 打开&#x200B;**Android Studio**
   * 初次启动Android Studio时，将显示安装[!DNL Android SDK]的提示。 接受默认值并完成安装。
1. 打开Android Studio并选择&#x200B;**配置文件或调试APK**
1. 选择在步骤2中下载的APK文件(**wknd-mobile.x.x.x.apk**)，然后单击&#x200B;**确定**
   * 如果提示您&#x200B;**创建新文件夹**，或&#x200B;**使用现有**，请选择&#x200B;**使用现有**。
1. 在Android Studio的初始启动中，右键单击“项目”列表中的&#x200B;**wknd-mobile.x.x.x**，然后选择&#x200B;**打开模块设置**。
   * 在&#x200B;**模块> wknd-mobile.x.x.x >依赖项选项卡**&#x200B;下，选择&#x200B;**Android API 29平台**。 点按确定以关闭并保存更改。
   * 如果不这样做，您将在尝试启动模拟器时看到“请选择Android SDK”错误。
1. 选择&#x200B;**工具> AVD管理器**&#x200B;或点按顶部栏中的&#x200B;**AVD管理器**&#x200B;图标，打开&#x200B;**AVD管理器**。
1. 在&#x200B;**AVD管理器**&#x200B;窗口中，单击&#x200B;**+创建虚拟设备……**（如果尚未注册设备）。
   1. 在左侧，选择&#x200B;**电话**&#x200B;类别。
   1. 选择&#x200B;**像素2**。
   1. 单击&#x200B;**下一步**&#x200B;按钮。
   1. 选择具有&#x200B;**API级别29**&#x200B;的&#x200B;**Q**。
      * AVD管理器初次启动时，系统会要求您下载版本化的API。 单击“Q”版旁边的“下载”链接，完成下载和安装。
   1. 单击&#x200B;**下一步**&#x200B;按钮。
   1. 单击&#x200B;**完成**&#x200B;按钮。
1. 关闭&#x200B;**AVD管理器**&#x200B;窗口。
1. 在顶部菜单栏中，从&#x200B;**运行/编辑配置**&#x200B;下拉列表中选择&#x200B;**wknd-mobile.x.x.x**。
1. 点按选定&#x200B;**运行/编辑配置**&#x200B;旁边的&#x200B;**运行**&#x200B;按钮
1. 在弹出窗口中，选择新创建的&#x200B;**[!DNL Pixel 2 API 29]**&#x200B;虚拟设备，然后点按&#x200B;**确定**
1. 如果[!DNL WKND Mobile]应用程序未立即加载，请在模拟器中从Android主屏幕查找并点按&#x200B;**[!DNL WKND]**&#x200B;图标。
   * 如果模拟器启动，但模拟器的屏幕保持黑色，请点按模拟器窗口旁边的“工具”窗口中的&#x200B;**电源**&#x200B;按钮。
   * 要在虚拟设备内滚动，请按住并拖动。
   * 要从AEM中刷新内容，请从顶部下拉直到刷新图标
显示和释放。

>[!VIDEO](https://video.tv.adobe.com/v/28341?quality=12&learn=on)

## 移动设备应用程序代码

此部分重点介绍交互次数最多、依赖于AEM Content Services及其JSON输出的Android移动设备应用程序代码。

加载后，移动设备应用程序将`HTTP GET`转换为`/content/wknd-mobile/en/api/events.model.json`，这是AEM Content Services的端点，配置为提供内容以驱动移动设备应用程序。

由于事件API (`/content/wknd-mobile/en/api/events.model.json`)的可编辑模板已锁定，因此可以对移动设备应用程序进行编码，以在JSON响应中的特定位置查找特定信息。

### 高级代码流

1. 打开[!DNL WKND Mobile]应用程序会调用`HTTP GET`请求（位于`/content/wknd-mobile/en/api/events.model.json`）到AEM Publish以收集内容以填充移动设备应用程序的UI。
2. 在从AEM收到内容时，移动设备应用程序的三个视图元素（**徽标、标记行和事件列表**）中的每一个都使用AEM中的内容进行初始化。
   * 要将AEM内容绑定到移动设备应用程序的视图元素，表示每个AEM组件的JSON是映射到Java POJO的对象，而该对象又绑定到Android视图元素。
      * 图像组件JSON →徽标POJO →徽标ImageView
      * 文本组件JSON → TagLine POJO →文本图像视图
      * 内容片段列表JSON →事件POJO →事件RecyclerView
   * *移动设备应用程序代码能够将JSON映射到POJO，因为存在较大JSON响应中的已知位置。 请记住，“image”、“text”和“contentfragmentlist”的JSON键由后备AEM组件的节点名称指定。 如果这些节点名称发生更改，则移动设备应用程序将中断，因为它不知道如何从JSON数据获取必需的内容。*

#### 调用AEM Content Services端点

以下是移动设备应用程序`MainActivity`中负责调用AEM Content Services以收集推动移动设备应用程序体验的内容代码的摘录。

```
protected void onCreate(Bundle savedInstanceState) {
    ...
    // Create the custom objects that will map the JSON -> POJO -> View elements
    final List<ViewBinder> viewBinders = new ArrayList<>();

    viewBinders.add(new LogoViewBinder(this, getAemHost(), (ImageView) findViewById(R.id.logo)));
    viewBinders.add(new TagLineViewBinder(this, (TextView) findViewById(R.id.tagLine)));
    viewBinders.add(new EventsViewBinder(this, getAemHost(), (RecyclerView) findViewById(R.id.eventsList)));
    ...
    initApp(viewBinders);
}

private void initApp(final List<ViewBinder> viewBinders) {
    final String aemUrl = getAemUrl(); // -> http://localhost:4503/content/wknd-mobile/en/api/events.mobile.json
    JsonObjectRequest request = new JsonObjectRequest(aemUrl, null,
        new Response.Listener<JSONObject>() {
            @Override
            public void onResponse(JSONObject response) {
                for (final ViewBinder viewBinder : viewBinders) {
                    viewBinder.bind(response);
                }
            }
        },
        ...
    );
}
```

`onCreate(..)`是移动设备应用程序的初始化挂接，并注册负责解析JSON并将值捆绑到`View`元素的3个自定义`ViewBinders`。

随后调用`initApp(...)`，这将向AEM Publish上的AEM Content Services端点发出HTTPGET请求以收集内容。 收到有效的JSON响应后，该JSON响应将传递到每个`ViewBinder`，后者负责解析JSON并将其绑定到移动设备`View`元素。

#### 解析JSON响应

接下来，我们将介绍`LogoViewBinder`，它非常简单，但重点介绍了几个重要注意事项。

```
public class LogoViewBinder implements ViewBinder {
    ...
    public void bind(JSONObject jsonResponse) throws JSONException, IOException {
        final JSONObject components = jsonResponse.getJSONObject(":items").getJSONObject("root").getJSONObject(":items");

        if (components.has("image")) {
            final Image image = objectMapper.readValue(components.getJSONObject("image").toString(), Image.class);

            final String imageUrl = aemHost + image.getSrc();
            Glide.with(context).load(imageUrl).into(logo);
        } else {
            Log.d("WKND", "Missing Logo");
        }
    }
}
```

`bind(...)`的第一行通过键&#x200B;**：items→根→：items**&#x200B;向下导航JSON响应，该键表示组件已添加到的AEM布局容器。

在此处，将对表示图像组件的名为&#x200B;**image**&#x200B;的键进行检查(同样，此节点名称→JSON键必须稳定)。 如果此对象存在，它将通过Jackson `ObjectMapper`库读取并映射到[自定义图像POJO](#image-pojo)。 下面将浏览图像POJO。

最后，徽标的`src`已使用[!DNL Glide]帮助程序库加载到Android ImageView中。

请注意，我们必须向AEM Publish实例提供AEM架构、主机和端口（通过`aemHost`），因为AEM Content Services将仅提供JCR路径(即 `/content/dam/wknd-mobile/images/wknd-logo.png`)到引用的内容。

#### 图像POJO{#image-pojo}

虽然是可选的，但使用[Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html)或其他库（如Gson）提供的类似功能有助于将复杂的JSON结构映射到Java POJO，而无需直接处理本机JSON对象本身的繁琐过程。 在此简单示例中，我们通过`@JSONProperty`注释直接将`image` JSON对象中的`src`键映射到图像POJO中的`src`属性。

```
package com.adobe.aem.guides.wknd.mobile.android.models;

import com.fasterxml.jackson.annotation.JsonProperty;

public class Image {
    @JsonProperty(value = "src")
    private String src;

    public String getSrc() {
        return src;
    }
}
```

事件POJO需要从JSON对象中选择更多数据点，比起简单的图像（我们只需要`src`），它更适合使用此技术。

## 探索移动应用程序体验

现在，您已了解AEM Content Services如何改善本机移动设备体验，请使用所学知识执行以下步骤，并查看您的更改在移动设备应用程序中反映的情况。

每个步骤后，通过拉取刷新移动设备应用程序并验证移动设备体验的更新。

1. 创建并发布&#x200B;**新[!DNL Event]内容片段**
1. 取消发布&#x200B;**现有[!DNL Event]内容片段**
1. Publish **标记行**&#x200B;的更新

## 恭喜

**您已完成AEM Headless教程！**

要了解有关AEM Content Services和AEM as a Headless CMS的更多信息，请访问Adobe的其他文档和支持材料：

* [使用内容片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [AEM WCM核心组件用户指南](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans)
* [AEM WCM核心组件组件库](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM WCM核心组件GitHub项目](https://github.com/adobe/aem-core-wcm-components)
* [组件导出程序的代码示例](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
