---
title: 第7章 — 从移动设备应用程序使用AEM内容服务 — 内容服务
description: 本教程的第7章运行Android移动设备应用程序以使用AEM Content Services中的创作内容。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d6b6d425-842a-43a9-9041-edf78e51d962
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '1391'
ht-degree: 0%

---

# 第7章 — 从移动设备应用程序使用AEM内容服务

本教程的第7章使用本机Android移动设备应用程序从AEM Content Services中使用内容。

## Android移动设备应用程序

本教程使用 **简单的本机Android移动设备应用程序** 以使用和显示由AEM Content Services公开的事件内容。

的使用 [Android](https://developer.android.com/) 基本上不重要，消费性移动应用程序可以在任何框架中编写，适用于任何移动平台，例如iOS。

Android用于教程，因为能够在Windows、macOs和Linux上运行Android模拟器，其受欢迎程度，并且可以以Java(AEM开发人员非常了解的一种语言)编写。

*本教程的Android移动设备应用程序是&#x200B;**not**旨在指导如何构建Android Mobile应用程序或传达Android开发最佳实践，但是却是为了说明如何从移动设备应用程序中使用AEM Content Services。*

### AEM Content Services如何促进移动设备应用程序体验

![移动设备应用程序到内容服务的映射](assets/chapter-7/content-services-mapping.png)

1. 的 **徽标** 定义 [!DNL Events API] 页面的 **图像组件**.
1. 的 **标记行** 定义 [!DNL Events API] 页面的 **文本组件**.
1. 此 **事件列表** 派生自事件内容片段的序列化，通过配置的 **内容片段列表组件**.

## 移动设备应用程序演示

>[!VIDEO](https://video.tv.adobe.com/v/28345?quality=12&learn=on)

### 为非本地主机使用配置移动设备应用程序

如果未在上运行AEM发布 **http://localhost:4503** 可以在移动设备应用程序的 [!DNL Settings] 指向属性AEM发布主机/端口。

>[!VIDEO](https://video.tv.adobe.com/v/28344?quality=12&learn=on)

## 在本地运行移动设备应用程序

1. 下载并安装 [Android Studio](https://developer.android.com/studio/install) 以安装Android模拟器。
1. **下载** Android [!DNL APK] 文件 [GitHub >资产> wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. 打开 **Android Studio**
   * 首次启动Android Studio时，会提示安装 [!DNL Android SDK] 将会出现。 接受默认值并完成安装。
1. 打开Android Studio并选择 **配置文件或调试APK**
1. 选择APK文件(**wknd-mobile.x.x.x.apk**)，然后单击 **确定**
   * 如果系统提示 **创建新文件夹**&#x200B;或 **使用现有**，选择 **使用现有**.
1. 首次启动Android Studio时，右键单击 **wknd-mobile.x.x.x** 在“项目”列表中，选择 **打开模块设置**.
   * 在 **“模块”>“wknd-mobile.x.x.x”>“依赖项”选项卡**，选择 **Android API 29平台**. 点按确定以关闭并保存更改。
   * 如果您没有执行此操作，则在尝试启动模拟器时将看到“请选择Android SDK”错误。
1. 打开 **AVD管理器** 选择 **工具> AVD管理器** 或点按 **AVD管理器** 图标。
1. 在 **AVD管理器** 窗口，单击 **+创建虚拟设备……** 如果您尚未注册设备。
   1. 在左侧，选择 **电话** 类别。
   1. 选择 **像素2**.
   1. 单击 **下一个** 按钮。
   1. 选择 **Q** with **API级别29**.
      * 首次启动AVD管理器后，系统会要求您下载版本控制的API。 单击“Q”版本旁边的下载链接，然后完成下载和安装。
   1. 单击 **下一个** 按钮。
   1. 单击 **完成** 按钮。
1. 关闭 **AVD管理器** 窗口。
1. 在顶部菜单栏中，选择 **wknd-mobile.x.x.x** 从 **运行/编辑配置** 下拉。
1. 点按 **运行** 按钮 **运行/编辑配置**
1. 在弹出窗口中，选择新创建的 **[!DNL Pixel 2 API 29]** 虚拟设备和点按 **确定**
1. 如果 [!DNL WKND Mobile] 应用程序不会立即加载、查找并点按 **[!DNL WKND]** 图标。
   * 如果模拟器启动但模拟器屏幕保持黑色，请点按 **电源** 按钮。
   * 要在虚拟设备中滚动，请单击并按住并拖动。
   * 要从AEM刷新内容，请从顶部向下拉出，直到显示“刷新”图标，然后释放。

>[!VIDEO](https://video.tv.adobe.com/v/28341?quality=12&learn=on)

## 移动设备应用程序代码

此部分重点介绍最能交互且依赖于AEM Content Services及其JSON输出的Android移动设备应用程序代码。

加载后，移动设备应用程序将 `HTTP GET` to `/content/wknd-mobile/en/api/events.model.json` 即AEM Content Services端点，配置为提供内容以驱动移动设备应用程序。

因为事件API的可编辑模板(`/content/wknd-mobile/en/api/events.model.json`)，则可以对移动设备应用程序进行编码，以在JSON响应中的特定位置查找特定信息。

### 高级代码流

1. 打开 [!DNL WKND Mobile] 应用程序会调用 `HTTP GET` 请求AEM在以下位置发布 `/content/wknd-mobile/en/api/events.model.json` 以收集用于填充移动设备应用程序UI的内容。
2. 从AEM接收内容后，移动设备应用程序的三个视图元素中的每个元素： **徽标、标记行和事件列表**，将使用AEM中的内容进行初始化。
   * 要将AEM内容绑定到移动设备应用程序的视图元素，则表示每个AEM组件的JSON是映射到Java POJO的对象，Java POJO又绑定到Android视图元素。
      * 图像组件JSON →徽标POJO →徽标ImageView
      * 文本组件JSON → TagLine POJO →文本ImageView
      * 内容片段列表JSON →事件POJO →事件回收器视图
   * *由于JSON响应中的已知位置，移动设备应用程序代码能够将JSON映射到POJO。 请记住，“image”、“text”和“contentfragmentlist”的JSON键由支持AEM组件的节点名称指定。 如果这些节点名称发生更改，则移动设备应用程序将会中断，因为它不知道如何从JSON数据中源出必需的内容。*

#### 调用AEM Content Services端点

以下是移动应用程序中代码的提取 `MainActivity` 负责调用AEM Content Services来收集驱动移动设备应用程序体验的内容。

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

`onCreate(..)` 是移动设备应用程序的初始化挂接，并注册3个自定义 `ViewBinders` 负责解析JSON并将值绑定到 `View` 元素。

`initApp(...)` 随后将调用，以便在AEM发布中向AEM Content Services端点发出HTTPGET请求，以收集内容。 收到有效的JSON响应后，JSON响应将传递到每个 `ViewBinder` 负责解析JSON并将其绑定到移动设备 `View` 元素。

#### 解析JSON响应

接下来，我们将查看 `LogoViewBinder`，虽然这很简单，但也强调了一些重要注意事项。

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

第一行 `bind(...)` 通过键向下导航JSON响应 **：项→根→ ：项** 它表示已添加组件的AEM布局容器。

从此处将检查名为 **图像**，表示图像组件(同样，此节点名称→ JSON键值是稳定的)。 如果此对象存在，则会读取并映射到 [自定义图像POJO](#image-pojo) 通过杰克逊 `ObjectMapper` 库。 图像POJO将在下面进行探索。

最后，标志的 `src` 会使用 [!DNL Glide] 帮助程序库。

请注意，我们必须提供AEM架构、主机和端口(通过 `aemHost`)到AEM发布实例，因为AEM Content Services将仅提供JCR路径(即 `/content/dam/wknd-mobile/images/wknd-logo.png`)到引用的内容。

#### 图像POJO{#image-pojo}

但是，使用的是 [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) 或Gson等其他库提供的类似功能，有助于将复杂的JSON结构映射到Java POJO，而无需直接处理本机JSON对象本身。 在这个简单的例子中，我们将 `src` 键 `image` JSON对象，到 `src` 属性(通过 `@JSONProperty` 注释。

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

事件POJO需要从JSON对象中选择更多数据点，它比简单的图像更能从中受益，我们只想从 `src`.

## 探索移动设备应用程序体验

现在，您已经了解AEM Content Services如何驱动本机移动设备体验，接下来可以使用您学到的知识执行以下步骤，并查看移动设备应用程序中反映的更改。

在每个步骤之后，通过提取来刷新移动设备应用程序，并验证移动设备体验的更新。

1. 创建和发布 **新建 [!DNL Event] 内容片段**
1. 取消发布 **现有 [!DNL Event] 内容片段**
1. 将更新发布到 **标记行**

## 恭喜

**您已完成AEM Headless教程！**

要进一步了解AEM内容服务和AEM as a Headless CMS，请访问Adobe的其他文档和支持材料：

* [使用内容片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [AEM WCM核心组件用户指南](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans)
* [AEM WCM核心组件库](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM WCM核心组件GitHub项目](https://github.com/adobe/aem-core-wcm-components)
* [组件导出程序的代码示例](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
