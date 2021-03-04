---
title: 第7章 — 使用移动应用程序中的AEM内容服务 — 内容服务
description: 教程的第7章运行Android移动应用程序以使用AEM Content Services中创作的内容。
feature: 内容片段、API
topic: 无头、内容管理
role: 开发人员
level: 初学者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1416'
ht-degree: 0%

---


# 第7章 — 使用移动应用程序中的AEM内容服务

本教程的第7章使用本机Android移动应用程序使用AEM Content Services中的内容。

## Android移动应用程序

本教程使用&#x200B;**简单的本机Android Mobile应用程序**&#x200B;使用和显示AEM Content Services公开的事件内容。

使用[Android](https://developer.android.com/)基本上不重要，而且消费性移动应用程序可以在任何移动平台（例如iOS）的任何框架中编写。

Android用于教程，因为能够在Windows、macOs和Linux上运行Android模拟器，其受欢迎程度以及它可以编写为Java，Java是AEM开发人员非常了解的一种语言。

*本教程的Android Mobile应用程序不&#x200B;****旨在指导如何构建Android Mobile应用程序或传达Android开发最佳实践，而是演示如何从移动应用程序使用AEM Content Services。*

### AEM Content Services如何推动移动App体验

![移动应用程序到内容服务的映射](assets/chapter-7/content-services-mapping.png)

1. 由[!DNL Events API]页面的&#x200B;**图像组件**&#x200B;定义的&#x200B;**徽标**。
1. 在[!DNL Events API]页面的&#x200B;**文本组件**&#x200B;上定义的&#x200B;**标记行**。
1. 此&#x200B;**事件列表**&#x200B;源自事件内容片段的序列化，通过配置的&#x200B;**内容片段列表组件**&#x200B;公开。

## 移动App演示

>[!VIDEO](https://video.tv.adobe.com/v/28345/?quality=12&learn=on)

### 配置移动应用程序以供非本地主机使用

如果AEM发布未在&#x200B;**http://localhost:4503**&#x200B;上运行，则可以在Mobile应用程序的[!DNL Settings]中更新主机和端口，以指向属性AEM发布主机/端口。

>[!VIDEO](https://video.tv.adobe.com/v/28344/?quality=12&learn=on)

## 在本地运行移动应用程序

1. 下载并安装[Android Studio](https://developer.android.com/studio/install)以安装Android模拟器。
1. **下** 载Android [!DNL APK] 文 [件GitHub >资产> wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. 打开&#x200B;**Android Studio**
   * 在初始启动Android Studio时，将显示安装[!DNL Android SDK]的提示。 接受默认设置并完成安装。
1. 打开Android Studio，然后选择&#x200B;**用户档案或调试APK**
1. 选择步骤2中下载的APK文件(**wknd-mobile.x.x.x.apk**)，然后单击&#x200B;**确定**
   * 如果提示&#x200B;**创建新文件夹**&#x200B;或&#x200B;**使用现有**，请选择&#x200B;**使用现有**。
1. 在初始启动Android Studio时，右键单击“项目”列表中的&#x200B;**wknd-mobile.x.x.x**，然后选择“打开模块设置”**。**
   * 在“**模块> wknd-mobile.x.x.x >依赖项选项卡**&#x200B;下，选择&#x200B;**Android API 29 Platform**。 点按确定以关闭并保存更改。
   * 如果不这样做，在尝试启动模拟器时将显示“请选择Android SDK”错误。
1. 通过选择&#x200B;**“工具”>“AVD管理器”**&#x200B;或点按顶栏中的&#x200B;**AVD管理器**&#x200B;图标，打开&#x200B;**AVD管理器**。
1. 在&#x200B;**AVD管理器**&#x200B;窗口中，单击&#x200B;**+创建虚拟设备……**（如果您尚未注册设备）。
   1. 在左侧，选择&#x200B;**电话**&#x200B;类别。
   1. 选择&#x200B;**像素2**。
   1. 单击&#x200B;**下一步**&#x200B;按钮。
   1. 选择&#x200B;**Q**（**API级别29**）。
      * 在AVD Manager的初始启动后，将要求您下载版本化的API。 单击“Q”版本旁边的“下载”链接，然后完成下载和安装。
   1. 单击&#x200B;**下一步**&#x200B;按钮。
   1. 单击&#x200B;**完成**&#x200B;按钮。
1. 关闭&#x200B;**AVD Manager**&#x200B;窗口。
1. 在顶部菜单栏中，从&#x200B;**运行/编辑配置**&#x200B;下拉菜单中选择&#x200B;**wknd-mobile.x.x.x**。
1. 点按选定&#x200B;**运行/编辑配置**&#x200B;旁边的&#x200B;**运行**&#x200B;按钮
1. 在弹出窗口中，选择新创建的&#x200B;**[!DNL Pixel 2 API 29]**&#x200B;虚拟设备，然后点按&#x200B;**确定**
1. 如果[!DNL WKND Mobile]应用程序未立即加载，请在模拟器的Android主屏幕中查找并点按&#x200B;**[!DNL WKND]**&#x200B;图标。
   * 如果模拟器启动，但模拟器屏幕保持黑色，请点按模拟器工具窗口旁边模拟器工具窗口中的&#x200B;**power**&#x200B;按钮。
   * 要在虚拟设备中滚动，请单击并按住并拖动。
   * 要从AEM刷新内容，请从顶部向下拉，直到刷新图标
显示和释放。

>[!VIDEO](https://video.tv.adobe.com/v/28341/?quality=12&learn=on)

## 移动应用程序代码

本节重点介绍交互最多且依赖AEM Content Services的Android Mobile应用程序代码及其JSON输出。

加载后，移动应用程序将`HTTP GET`设置为`/content/wknd-mobile/en/api/events.model.json`，即配置为提供内容以驱动移动应用程序的AEM Content Services端点。

由于事件API(`/content/wknd-mobile/en/api/events.model.json`)的可编辑模板已锁定，因此可以对移动应用程序进行编码，以在JSON响应中的特定位置中查找特定信息。

### 高级代码流

1. 打开[!DNL WKND Mobile]应用程序将调用`HTTP GET`请求至`/content/wknd-mobile/en/api/events.model.json`的AEM发布，以收集用于填充移动应用程序UI的内容。
2. 从AEM收到内容后，移动应用的三个视图元素、**徽标、标记行和事件列表**&#x200B;中的每个元素都使用AEM中的内容进行初始化。
   * 要将AEM内容绑定到Mobile App的视图元素，表示每个AEM组件的JSON是映射到Java POJO的对象，而Java POJO又绑定到Android视图元素。
      * 图像组件JSON →徽标POJO →徽标ImageView
      * 文本组件JSON → TagLine POJO →文本图像视图
      * 内容片段列表JSON →事件POJO →事件回收器视图
   * *由于JSON响应更大时间内的已知位置，移动应用程序代码能够将JSON映射到POJO。请记住，“image”、“text”和“contentfragmentlist”的JSON键由支持AEM组件的节点名称指定。 如果这些节点名称发生更改，则移动应用程序将断开，因为它不知道如何从JSON数据中源入必要的内容。*

#### 调用AEM Content Services端点

以下是移动应用程序`MainActivity`中负责调用AEM Content Services以收集驱动移动应用程序体验的内容的代码提取。

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

`onCreate(..)` 是移动应用程序的初始化挂接，并注册3个自 `ViewBinders` 定义，负责解析JSON并将值绑定到 `View` 元素。

`initApp(...)` 然后，调用它，使HTTPGET请求在AEM发布上对AEM Content Services的端点处于收集状态。收到有效的JSON响应后，JSON响应将传递到每个`ViewBinder`，每个负责分析JSON并将其绑定到移动`View`元素。

#### 解析JSON响应

接下来，我们将看到`LogoViewBinder`，它很简单，但强调了几个重要注意事项。

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

`bind(...)`的第一行通过&#x200B;**:items → root → :items**&#x200B;键向下导航JSON响应，该键表示组件添加到的AEM布局容器。

在此处，将检查名为&#x200B;**image**&#x200B;的键，该键表示图像组件(同样，此节点名→ JSON键是稳定的)。 如果此对象存在，则它通过Jackson `ObjectMapper`库读取并映射到[自定义图像POJO](#image-pojo)。 下面将探讨图像POJO。

最后，使用[!DNL Glide]帮助库将徽标的`src`加载到Android ImageView中。

请注意，我们必须向AEM发布实例提供AEM模式、主机和端口（通过`aemHost`），因为AEM Content Services将仅提供JCR路径(即 `/content/dam/wknd-mobile/images/wknd-logo.png`)。

#### 图像POJO{#image-pojo}

虽然可选，但使用Gson等其他库提供的[Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html)或类似功能有助于将复杂的JSON结构映射到Java POJO，而无需直接处理本机JSON对象本身。 在这种简单的情况下，我们会直接通过`@JSONProperty`注释将`image` JSON对象的`src`键映射到图像POJO中的`src`属性。

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

事件 POJO需要从JSON对象中选择更多数据点，它比简单图像更能从中受益，我们只想使用`src`。

## 浏览移动App体验

现在，您已经了解AEM Content Services如何推动本机移动体验，可以使用您学到的知识执行以下步骤并查看移动应用程序中反映的更改。

每个步骤完成后，拉动以刷新移动应用程序并验证移动体验的更新。

1. 创建并发布&#x200B;**新[!DNL Event]内容片段**
1. 取消发布&#x200B;**现有[!DNL Event]内容片段**
1. 将更新发布到&#x200B;**标记行**

## 恭喜

**您已使用AEM无头教程完成！**

要进一步了解AEM内容服务和AEM作为无外设CMS的信息，请访问Adobe的其他文档和支持材料：

* [使用内容片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [AEM WCM核心组件用户指南](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/introduction.html)
* [AEM WCM核心组件库](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM WCM核心组件GitHub项目](https://github.com/adobe/aem-core-wcm-components)
* [AEM WCM核心组件 — 咨询专家](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-content-services.html)
* [组件导出器的代码示例](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/bundle/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
