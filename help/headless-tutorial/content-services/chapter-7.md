---
title: 第7章 — 从移动设备应用程序使用AEM内容服务 — 内容服务
description: 本教程的第7章运行Android移动设备应用程序以使用AEM Content Services中的创作内容。
feature: 内容片段、 API
topic: 无外设、内容管理
role: Developer
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '1412'
ht-degree: 0%

---


# 第7章 — 从移动设备应用程序使用AEM内容服务

本教程的第7章使用本机Android移动设备应用程序从AEM Content Services中使用内容。

## Android移动设备应用程序

本教程使用&#x200B;**简单的本机Android移动设备应用程序**&#x200B;来使用和显示由AEM Content Services公开的事件内容。

使用[Android](https://developer.android.com/)在很大程度上不重要，任何移动平台（例如iOS）的任何框架都可以编写消耗性的移动设备应用程序。

Android用于教程，因为能够在Windows、macOs和Linux上运行Android模拟器，其受欢迎程度，并且可以以Java(AEM开发人员非常了解的一种语言)编写。

*本教程的Android移动设备应用程序&#x200B;****不旨在指导如何构建Android移动设备应用程序或传达Android开发最佳实践，而是用于说明如何从移动设备应用程序中使用AEM Content Services。*

### AEM Content Services如何促进移动设备应用程序体验

![移动设备应用程序到内容服务的映射](assets/chapter-7/content-services-mapping.png)

1. **徽标**，由[!DNL Events API]页面的&#x200B;**图像组件**&#x200B;定义。
1. 在[!DNL Events API]页面的&#x200B;**文本组件**&#x200B;中定义的&#x200B;**标记行**。
1. 此&#x200B;**事件列表**&#x200B;是从事件内容片段的序列化派生的，该序列化通过配置的&#x200B;**内容片段列表组件**&#x200B;公开。

## 移动设备应用程序演示

>[!VIDEO](https://video.tv.adobe.com/v/28345/?quality=12&learn=on)

### 为非本地主机使用配置移动设备应用程序

如果AEM Publish未在&#x200B;**http://localhost:4503**&#x200B;上运行，则可以在移动设备应用程序的[!DNL Settings]中更新主机和端口，以指向属性AEM发布主机/端口。

>[!VIDEO](https://video.tv.adobe.com/v/28344/?quality=12&learn=on)

## 在本地运行移动设备应用程序

1. 下载并安装[Android Studio](https://developer.android.com/studio/install)以安装Android模拟器。
1. **** 下载Android [!DNL APK] 文 [件GitHub >资产> wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. 打开&#x200B;**Android Studio**
   * 首次启动Android Studio时，将出现安装[!DNL Android SDK]的提示。 接受默认值并完成安装。
1. 打开Android Studio并选择&#x200B;**配置文件或调试APK**
1. 选择在步骤2中下载的APK文件(**wknd-mobile.x.x.apk**)，然后单击&#x200B;**确定**
   * 如果提示&#x200B;**创建新文件夹**&#x200B;或&#x200B;**使用现有**，请选择&#x200B;**使用现有**。
1. 首次启动Android Studio时，右键单击“项目”列表中的&#x200B;**wknd-mobile.x.x.x**，然后选择&#x200B;**打开模块设置**。
   * 在&#x200B;**模块> wknd-mobile.x.x.x >依赖项选项卡**&#x200B;下，选择&#x200B;**Android API 29 Platform**。 点按确定以关闭并保存更改。
   * 如果您没有执行此操作，则在尝试启动模拟器时将看到“请选择Android SDK”错误。
1. 通过选择&#x200B;**工具> AVD管理器**&#x200B;或点按顶部栏中的&#x200B;**AVD管理器**&#x200B;图标，打开&#x200B;**AVD管理器**。
1. 在&#x200B;**AVD管理器**&#x200B;窗口中，单击&#x200B;**+创建虚拟设备……**（如果尚未注册设备）。
   1. 在左侧，选择&#x200B;**Phone**&#x200B;类别。
   1. 选择&#x200B;**像素2**。
   1. 单击&#x200B;**Next**&#x200B;按钮。
   1. 选择&#x200B;**Q**，其中&#x200B;**API级别29**。
      * 首次启动AVD管理器后，将要求您下载版本控制的API。 单击“Q”版本旁边的下载链接，然后完成下载和安装。
   1. 单击&#x200B;**Next**&#x200B;按钮。
   1. 单击&#x200B;**Finish**&#x200B;按钮。
1. 关闭&#x200B;**AVD管理器**&#x200B;窗口。
1. 在顶部菜单栏的&#x200B;**运行/编辑配置**&#x200B;下拉菜单中，选择&#x200B;**wknd-mobile.x.x.x**。
1. 点按选定&#x200B;**运行/编辑配置**&#x200B;旁边的&#x200B;**运行**&#x200B;按钮
1. 在弹出窗口中，选择新创建的&#x200B;**[!DNL Pixel 2 API 29]**&#x200B;虚拟设备，然后点按&#x200B;**确定**
1. 如果[!DNL WKND Mobile]应用程序未立即加载，请在模拟器的Android主屏幕中找到并点按&#x200B;**[!DNL WKND]**&#x200B;图标。
   * 如果模拟器启动，但模拟器屏幕保持黑色，请点按模拟器工具窗口中模拟器窗口旁边的&#x200B;**power**&#x200B;按钮。
   * 要在虚拟设备中滚动，请单击并按住并拖动。
   * 要从AEM刷新内容，请从顶部向下拉，直到出现“刷新”图标
显示和发布。

>[!VIDEO](https://video.tv.adobe.com/v/28341/?quality=12&learn=on)

## 移动设备应用程序代码

此部分重点介绍最能交互且依赖于AEM Content Services及其JSON输出的Android移动设备应用程序代码。

加载时，移动设备应用程序会将`HTTP GET`发布到`/content/wknd-mobile/en/api/events.model.json`，即AEM Content Services端点，该端点配置为提供内容以驱动移动设备应用程序。

由于事件API(`/content/wknd-mobile/en/api/events.model.json`)的可编辑模板已锁定，因此可以对移动设备应用程序进行编码，以在JSON响应中的特定位置查找特定信息。

### 高级代码流

1. 打开[!DNL WKND Mobile]应用程序时，会向位于`/content/wknd-mobile/en/api/events.model.json`的AEM发布中调用`HTTP GET`请求，以收集用于填充移动设备应用程序UI的内容。
2. 从AEM收到内容后，移动设备应用程序的三个视图元素、**徽标、标记行和事件列表**&#x200B;中的每个元素都会使用AEM中的内容进行初始化。
   * 要将AEM内容绑定到移动设备应用程序的视图元素，则表示每个AEM组件的JSON是映射到Java POJO的对象，Java POJO又绑定到Android视图元素。
      * 图像组件JSON →徽标POJO →徽标ImageView
      * 文本组件JSON → TagLine POJO →文本ImageView
      * 内容片段列表JSON →事件POJO →事件回收器视图
   * *由于JSON响应中的已知位置，移动设备应用程序代码能够将JSON映射到POJO。请记住，“image”、“text”和“contentfragmentlist”的JSON键由支持AEM组件的节点名称指定。 如果这些节点名称发生更改，则移动设备应用程序将会中断，因为它不知道如何从JSON数据中源出必需的内容。*

#### 调用AEM Content Services端点

以下是移动设备应用程序`MainActivity`中负责调用AEM Content Services以收集驱动移动设备应用程序体验的内容的代码蒸馏。

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

`onCreate(..)` 是移动设备应用程序的初始化挂接，并注册3个自定 `ViewBinders` 义，负责解析JSON并将值绑定到 `View` 元素。

`initApp(...)` 随后将调用，以便在AEM发布中向AEM Content Services端点发出HTTPGET请求，以收集内容。收到有效的JSON响应后，JSON响应将传递到每个`ViewBinder`，每个负责解析JSON并将其绑定到移动设备`View`元素。

#### 解析JSON响应

接下来，我们将看到`LogoViewBinder`，它很简单，但着重介绍了几个重要注意事项。

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

`bind(...)`的第一行通过键&#x200B;**:items → root → :items**&#x200B;向下浏览JSON响应，该键表示组件添加到的AEM布局容器。

在此，将检查名为&#x200B;**image**&#x200B;的键，该键表示图像组件(同样，此节点名称→ JSON键值是稳定的)。 如果此对象存在，则它会通过Jackson `ObjectMapper`库读取并映射到[自定义图像POJO](#image-pojo)。 图像POJO将在下面进行探索。

最后，徽标的`src`将使用[!DNL Glide]帮助程序库加载到Android ImageView中。

请注意，我们必须向AEM发布实例提供AEM架构、主机和端口（通过`aemHost`），因为AEM Content Services将仅提供JCR路径(即， `/content/dam/wknd-mobile/images/wknd-logo.png`)到引用的内容。

#### 图像POJO{#image-pojo}

虽然可选，但使用[Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html)或其他库（如Gson）提供的类似功能，有助于将复杂的JSON结构映射到Java POJO，而无需直接处理本机JSON对象本身。 在这个简单的示例中，我们会直接通过`@JSONProperty`注释将`src`键从`image` JSON对象映射到图像POJO中的`src`属性。

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

事件POJO需要从JSON对象中选择更多数据点，它比简单的图像（我们只希望使用`src`）更能从此技术中受益。

## 探索移动设备应用程序体验

现在，您已经了解AEM Content Services如何驱动本机移动设备体验，接下来可以使用您学到的知识执行以下步骤，并查看移动设备应用程序中反映的更改。

在每个步骤之后，通过提取来刷新移动设备应用程序，并验证移动设备体验的更新。

1. 创建和发布&#x200B;**新[!DNL Event]内容片段**
1. 取消发布&#x200B;**现有[!DNL Event]内容片段**
1. 发布对&#x200B;**标记行**&#x200B;的更新

## 恭喜

**您已完成AEM Headless教程！**

要进一步了解AEM内容服务和AEM as a Headless CMS，请访问Adobe的其他文档和支持材料：

* [使用内容片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [AEM WCM核心组件用户指南](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans)
* [AEM WCM核心组件库](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM WCM核心组件GitHub项目](https://github.com/adobe/aem-core-wcm-components)
* [AEM WCM核心组件 — 咨询专家](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-content-services.html)
* [组件导出程序的代码示例](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/bundle/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
