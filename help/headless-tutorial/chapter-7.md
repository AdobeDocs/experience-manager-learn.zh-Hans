---
title: AEM无头入门——第7章——使用移动应用程序中的AEM内容服务
description: 教程的第7章运行Android移动应用程序以使用AEM Content Services创作的内容。
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '1404'
ht-degree: 0%

---


# 第7章——使用移动应用程序中的AEM内容服务

本教程的第7章使用本机Android移动应用程序使用AEM Content Services中的内容。

## Android移动应用程序

本教程使用简 **单的本机Android移动应用程序** ，使用和显示AEM Content Services公开的事件内容。

Android的使 [用基本上不](https://developer.android.com/) 重要，而消费性的移动应用程序可以在任何移动平台框架中编写，例如iOS。

Android用于教程，因为能够在Windows、macOs和Linux上运行Android模拟器，其受欢迎程度以及它可以作为Java编写，Java是AEM开发人员非常了解的一种语言。

*本教程的Android移动应用程&#x200B;**序**并非旨在指导如何构建Android移动应用程序或传达Android开发最佳实践，而是说明如何从移动应用程序使用AEM Content Services。*

### AEM Content Services如何推动移动App体验

![移动应用程序到内容服务的映射](assets/chapter-7/content-services-mapping.png)

1. 由 **页面的图** 像组件定 [!DNL Events API] 义的标志 ****。
1. 页 **面的** “文本”组 [!DNL Events API] 件上定 **义的标记行**。
1. 此 **事件** 列表源于通过配置的内容片段列表组件公开的事件内 **容片段的序列化**。

## 移动应用演示

>[!VIDEO](https://video.tv.adobe.com/v/28345/?quality=12&learn=on)

### 为非本地主机使用配置移动应用程序

如果http://localhost:4503上未运行 **AEM** Publish [!DNL Settings] ，则可在移动应用程序中更新主机和端口，以指向属性AEM Publish主机／端口。

>[!VIDEO](https://video.tv.adobe.com/v/28344/?quality=12&learn=on)

## 在本地运行移动应用程序

1. 下载并安装 [Android Studio](https://developer.android.com/studio/install) ，以安装Android模拟器。
1. **下载** Android文 [!DNL APK] 件 [GitHub >资产> wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. 打开 **Android Studio**
   * 首次启动Android Studio时，将显示安装提示 [!DNL Android SDK] 消息。 接受默认值并完成安装。
1. 打开Android Studio并选择“ **用户档案”或“调试APK”**
1. 选择步骤2 **中下载的APK文件(wknd-mobile.x.x.x**.apk)，然后单击 **确定**
   * 如果提示“创 **建新文件夹**”或“ **使用现有文件夹**”，请选 **择“使用现有文件夹**”。
1. 在Android Studio的初始启动中，右键单击“项 **目”列表中的** wknd-mobile.x.x **.x，然后选择“打开模**&#x200B;块设置”。
   * 在“ **模块”>“wknd-mobile.x.x.x”>“依赖项”选项卡下**，选 **择“Android API 29 Platform**”。 点按确定以关闭并保存更改。
   * 如果不这样做，在尝试启动模拟器时将显示“请选择Android SDK”错误。
1. 通过选 **择工具** > AVD管 **理器或点按顶** 栏中的AVD **Manager** 图标，打开AVD Manager。
1. 在“AVD **管理器** ”窗口中，单 **击+创建虚拟设备……(如果尚未注册设备** )。
   1. 在左侧，选择“电话 **类别** ”。
   1. 选择一 **个像素** 2。
   1. 单击“下 **一步** ”按钮。
   1. 选择 **Q** with **API Level 29**。
      * 在AVD Manager的初始启动后，将要求您下载版本化的API。 单击“Q”版本旁的“下载”链接，然后完成下载和安装。
   1. 单击“下 **一步** ”按钮。
   1. 单击“完 **成** ”按钮。
1. 关闭“ **AVD管理器** ”窗口。
1. 在顶部菜单栏 **的“运行／编辑配置** ”下 **拉菜单中选择wknd-mobile** .x.x.x。
1. 点按选 **定** “运行／编辑配 **置”旁边的“运行”**
1. 在弹出窗口中，选择新创建的虚 **[!DNL Pixel 2 API 29]** 拟设备并点 **按确定**
1. 如果应 [!DNL WKND Mobile] 用程序未立即加载，请在模拟器的Android主 **[!DNL WKND]** 屏幕中查找并点击该图标。
   * 如果模拟器启动但模拟器屏幕保持黑色，请点 **击** “模拟器”工具窗口中模拟器窗口旁边的电源按钮。
   * 要在虚拟设备中滚动，请单击并按住并拖动。
   * 要从AEM刷新内容，请从顶部向下拉，直到“刷新”图标显示，然后释放。

>[!VIDEO](https://video.tv.adobe.com/v/28341/?quality=12&learn=on)

## 移动应用程序代码

本节重点介绍交互最多且依赖AEM Content Services及其JSON输出的Android移动应用程序代码。

加载后，移动应用程 `HTTP GET` 序将 `/content/wknd-mobile/en/api/events.model.json` 其配置为提供内容以驱动移动应用程序的AEM Content Services端点。

由于事件API()的可编辑模`/content/wknd-mobile/en/api/events.model.json`板已锁定，因此可以对移动应用程序进行编码，以在JSON响应中的特定位置查找特定信息。

### 高级代码流

1. 打开应 [!DNL WKND Mobile] 用程序 `HTTP GET` 时，将向AEM发布调用 `/content/wknd-mobile/en/api/events.model.json` 一个请求，以收集用于填充移动应用程序UI的内容。
2. 从AEM收到内容后，移动应用程序的三个视图元素(徽标、标 **签行和事件列表)中的每个元素**，都会使用AEM中的内容进行初始化。
   * 要将AEM内容绑定到移动应用程序的视图元素，表示每个AEM组件的JSON是映射到Java POJO的对象，而Java POJO又绑定到Android视图元素。
      * 图像组件JSON→徽标POJO→徽标ImageView
      * 文本组件JSON→ TagLine POJO→文本图像视图
      * 内容片段列表JSON→事件POJO→事件回收器视图
   * *由于JSON响应更大，移动应用程序代码能够将JSON映射到POJO。 请记住，“image”、“text”和“contentfragmentlist”的JSON键由支持AEM组件的节点名称指定。 如果这些节点名称发生更改，则移动应用程序将会中断，因为它不知道如何从JSON数据中源入必要的内容。*

#### 调用AEM Content Services端点

以下是移动应用程序中负责调用AEM Content Services以收 `MainActivity` 集驱动移动应用程序体验的内容的代码。

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

`onCreate(..)` 是移动应用程序的初始化挂接，并注册3个自 `ViewBinders` 定义，负责解析JSON并将值绑定到元 `View` 素。

`initApp(...)` 然后，调用它，以便在AEM发布上向AEM Content Services发送HTTPGET请求以收集内容。 收到有效的JSON响应后，JSON响应将传递给负责 `ViewBinder` 分析JSON并将其绑定到移动元素的每 `View` 个。

#### 解析JSON响应

接下来我们将看一下 `LogoViewBinder`，它很简单，但着重强调了几个重要的考虑。

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

第一行通 `bind(...)` 过以下键向下导航JSON响 **应：项目→根→** ：项目，它表示已添加组件的AEM布局容器。

从此处检查表示图像组 **件**（同样，此节点名称→JSON键是稳定的）的名称为image的键。 如果此对象存在，则它通过Jackson库读 [取并映射到自](#image-pojo) 定义图像 `ObjectMapper` POJO。 下面将探索图像POJO。

最后，使用帮 `src` 助程序库将徽标加载到 [!DNL Glide] Android ImageView中。

请注意，我们必须向AEM发布实例提供AEM模式、主机 `aemHost`和端口（通过），因为AEM内容服务将仅提供JCR路径(即 `/content/dam/wknd-mobile/images/wknd-logo.png`)。

#### 图像POJO{#image-pojo}

尽管可选，使用Jackson [ObjectMapper或Gson](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) 等其他库提供的类似功能有助于将复杂的JSON结构映射到Java POJO，而无需直接处理本机JSON对象本身。 在这种简单的情况下，我 `src` 们直接通过注 `image` 释将JSON对象的键 `src` 映射到图像POJO中的属 `@JSONProperty` 性。

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

事件POJO需要从JSON对象中选择更多数据点，它比简单图像更有优势，我们只想使用简单图像 `src`。

## 浏览移动App体验

现在，您已经了解AEM Content Services如何推动本机移动体验，请使用您学到的知识执行以下步骤并查看移动应用程序中反映的更改。

每个步骤完成后，拉动以刷新移动应用程序并验证移动体验的更新。

1. 创建和发布 **新[!DNL Event]内容片段**
1. 取消发布 **现有[!DNL Event]内容片段**
1. 将更新发布到标 **记行**

## 恭喜

**您已经完成了AEM无头教程！**

要进一步了解AEM内容服务和AEM作为无外设CMS的信息，请访问Adobe的其他文档和支持材料：

* [使用内容片段](../sites/content-fragments/understand-content-fragments-and-experience-fragments.md)
* [AEM WCM核心组件用户指南](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/introduction.html)
* [AEM WCM核心组件库](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM WCM核心组件GitHub项目](https://github.com/adobe/aem-core-wcm-components)
* [AEM WCM核心组件——咨询专家](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-content-services.html)
* [组件导出器的代码示例](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/bundle/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
