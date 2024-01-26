---
title: 第7章 — 从移动设备应用程序使用AEM Content Services - Content Services
description: 本教程的第7章将运行Android移动设备应用程序，以使用来自AEM Content Services的创作内容。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: d6b6d425-842a-43a9-9041-edf78e51d962
duration: 532
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1350'
ht-degree: 0%

---

# 第7章 — 从移动设备应用程序使用AEM Content Services

本教程的第7章使用本机Android移动设备应用程序来使用AEM Content Services中的内容。

## Android移动设备应用程序

本教程使用 **简单的本机Android移动设备应用程序** 以使用和显示由AEM Content Services公开的事件内容。

使用 [Android](https://developer.android.com/) 这在很大程度上并不重要，而且消费型移动应用程序可以在任何移动平台(例如iOS)的框架中编写。

Android之所以用作教程，是因为它可以在Windows、macOs和Linux上运行Android模拟器，而且它颇受欢迎，并且它可以编写为Java语言，这是AEM开发人员非常了解的一种语言。

*本教程的Android移动设备应用程序为&#x200B;**非**旨在指导如何构建Android移动设备应用程序或传达Android开发最佳实践，而是说明如何从移动设备应用程序使用AEM Content Services。*

### AEM Content Services如何推动移动应用程序体验

![移动应用程序到Content Services的映射](assets/chapter-7/content-services-mapping.png)

1. 此 **徽标** 由定义 [!DNL Events API] 页面的 **图像组件**.
1. 此 **标记行** 如 [!DNL Events API] 页面的 **文本组件**.
1. 此 **事件列表** 派生自事件内容片段的序列化，通过配置的公开 **内容片段列表组件**.

## 移动应用程序演示

>[!VIDEO](https://video.tv.adobe.com/v/28345?quality=12&learn=on)

### 配置移动应用程序以供非本地主机使用

如果AEM Publish未在上运行 **http://localhost:4503** 主机和端口可以在移动设备应用程序的 [!DNL Settings] 指向AEM发布主机/端口属性。

>[!VIDEO](https://video.tv.adobe.com/v/28344?quality=12&learn=on)

## 在本地运行移动设备应用程序

1. 下载并安装 [Android Studio](https://developer.android.com/studio/install) 安装Android模拟器。
1. **下载** android [!DNL APK] 文件 [GitHub >资产> wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. 打开 **Android Studio**
   * 在Android Studio初次启动时，系统提示安装 [!DNL Android SDK] 将出席。 接受默认值并完成安装。
1. 打开Android Studio并选择 **配置文件或调试APK**
1. 选择APK文件(**wknd-mobile.x.x.x.apk**)下载，然后单击 **确定**
   * 如果提示 **创建新文件夹**，或 **使用现有**，选择 **使用现有**.
1. 在Android Studio初次启动时，右键单击 **wknd-mobile.x.x.x** 在“项目”列表中，然后选择 **打开模块设置**.
   * 下 **“模块”>“wknd-mobile.x.x.x”>“依赖项”选项卡**，选择 **Android API 29平台**. 点按确定以关闭并保存更改。
   * 如果不这样做，您将在尝试启动模拟器时看到“请选择Android SDK”错误。
1. 打开 **AVD管理器** 通过选择 **“工具”>“AVD管理器”** 或点按 **AVD管理器** 图标。
1. 在 **AVD管理器** 窗口，单击 **+创建虚拟设备……** 如果您尚未注册设备。
   1. 在左侧，选择 **电话** 类别。
   1. 选择 **像素2**.
   1. 单击 **下一个** 按钮。
   1. 选择 **Q** 替换为 **API级别29**.
      * AVD管理器初次启动时，系统会要求您下载版本化的API。 单击“Q”版旁边的“下载”链接，完成下载和安装。
   1. 单击 **下一个** 按钮。
   1. 单击 **完成** 按钮。
1. 关闭 **AVD管理器** 窗口。
1. 在顶部菜单栏中，选择 **wknd-mobile.x.x.x** 从 **运行/编辑配置** 下拉菜单。
1. 点按 **运行** 按钮（位于选定内容旁） **运行/编辑配置**
1. 在弹出窗口中，选择新创建的 **[!DNL Pixel 2 API 29]** 虚拟设备和点击 **确定**
1. 如果 [!DNL WKND Mobile] 应用程序不会立即加载、查找并点按 **[!DNL WKND]** 图标（位于模拟器的Android主屏幕中）。
   * 如果模拟器启动，但模拟器的屏幕保持黑色，请点按 **幂** “模拟器”窗口旁边的“工具”窗口中的按钮。
   * 要在虚拟设备内滚动，请按住并拖动。
   * 要从AEM中刷新内容，请从顶部下拉直到显示“刷新”图标，然后释放。

>[!VIDEO](https://video.tv.adobe.com/v/28341?quality=12&learn=on)

## 移动设备应用程序代码

此部分重点介绍交互次数最多、依赖于AEM Content Services及其JSON输出的Android移动设备应用程序代码。

加载后，移动设备应用程序会发出 `HTTP GET` 到 `/content/wknd-mobile/en/api/events.model.json` ，这是AEM Content Services端点，配置为提供内容以驱动移动设备应用程序。

因为事件API的可编辑模板(`/content/wknd-mobile/en/api/events.model.json`)锁定，则可以对移动设备应用程序进行编码，以在JSON响应中的特定位置查找特定信息。

### 高级代码流

1. 打开 [!DNL WKND Mobile] 应用程序调用 `HTTP GET` 请求发布至AEM `/content/wknd-mobile/en/api/events.model.json` 收集内容以填充移动设备应用程序的UI。
2. 从AEM收到内容后，移动设备应用程序的三个视图元素中的每一个都会将 **徽标、标签行和活动列表**，使用AEM中的内容进行初始化。
   * 要将AEM内容绑定到移动设备应用程序的视图元素，表示每个AEM组件的JSON是映射到Java POJO的对象，而该对象又绑定到Android视图元素。
      * 图像组件JSON →徽标POJO →徽标ImageView
      * 文本组件JSON → TagLine POJO →文本图像视图
      * 内容片段列表JSON →事件POJO →事件RecyclerView
   * *由于较大JSON响应中的已知位置，移动设备应用程序代码能够将JSON映射到POJO。 请记住，“image”、“text”和“contentfragmentlist”的JSON键由后备AEM组件的节点名称指定。 如果这些节点名称发生更改，则移动设备应用程序将中断，因为它不知道如何从JSON数据获取所需内容。*

#### 调用AEM Content Services端点

以下是移动设备应用程序 `MainActivity` 负责调用AEM Content Services以收集推动移动设备应用程序体验的内容。

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

`onCreate(..)` 是移动设备应用程序的初始化挂接，并注册3个自定义项 `ViewBinders` 负责解析JSON并将值捆绑到 `View` 元素。

`initApp(...)` 随后调用，这将向AEM Publish上的AEM Content Services端点发出HTTPGET请求以收集内容。 收到有效的JSON响应后，JSON响应将传递到每个JSON `ViewBinder` 负责解析JSON并将其绑定到移动设备 `View` 元素。

#### 解析JSON响应

接下来，我们将查看 `LogoViewBinder`，这很简单，但会强调几个重要注意事项。

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

第一行 `bind(...)` 通过键向下导航JSON响应 **：items → root → ：items** ，表示将组件添加到的AEM布局容器。

从此处，将对名为的密钥进行检查 **图像**，表示图像组件(同样，此节点名称→JSON键稳定也很重要)。 如果此对象存在，则它读取并映射到 [自定义图像POJO](#image-pojo) 通过杰克逊 `ObjectMapper` 库。 下面将浏览图像POJO。

最后，徽标的 `src` 使用加载到Android ImageView中 [!DNL Glide] 辅助函数库。

请注意，我们必须提供AEM架构、主机和端口(通过 `aemHost`)到AEM发布实例，因为AEM Content Services将仅提供JCR路径(即 `/content/dam/wknd-mobile/images/wknd-logo.png`)到引用的内容。

#### 图像POJO{#image-pojo}

虽然可选，但使用 [Jackson对象映射器](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) 或其他库（如Gson）提供的类似功能有助于将复杂的JSON结构映射到Java POJO，而无需直接处理本机JSON对象本身的繁琐过程。 在这个简单的例子中，我们将 `src` 键自 `image` JSON对象，到 `src` 属性直接通过 `@JSONProperty` 注释。

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

事件POJO需要从JSON对象中选择更多数据点，比起简单的图像(我们只希望 `src`.

## 探索移动应用程序体验

现在，您已了解AEM Content Services如何改善本机移动设备体验，请使用所学知识执行以下步骤，并查看您的更改在移动设备应用程序中反映的情况。

每个步骤后，通过拉取刷新移动设备应用程序并验证移动设备体验的更新。

1. 创建并发布 **新建 [!DNL Event] 内容片段**
1. 取消发布 **现有 [!DNL Event] 内容片段**
1. 将更新发布到 **标记行**

## 恭喜

**您已完成AEM Headless教程！**

要了解有关AEM Content Services和AEM as a Headless CMS的更多信息，请访问Adobe的其他文档和支持材料：

* [使用内容片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [AEM WCM核心组件用户指南](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)
* [AEM WCM核心组件组件库](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM WCM核心组件GitHub项目](https://github.com/adobe/aem-core-wcm-components)
* [组件导出器的代码示例](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
