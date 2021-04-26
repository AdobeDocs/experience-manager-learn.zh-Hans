---
title: 将可编辑组件添加到远程SPA动态路由
description: 了解如何在远程SPA中将可编辑组件添加到动态路由。
topic: 无外设、SPA、开发
feature: SPA编辑器，核心组件， API，开发
role: Developer, Architect
level: Beginner
kt: 7636
thumbnail: kt-7636.jpeg
translation-type: tm+mt
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '958'
ht-degree: 0%

---


# 动态路由和可编辑组件

在本章中，我们支持两条动态Adventure Detail路由以支持可编辑组件；波特兰的巴厘冲浪营&#x200B;__和__&#x200B;贝尔瓦纳。____

![动态路由和可编辑组件](./assets/spa-dynamic-routes/intro.png)

冒险详细信息SPA路由定义为`/adventure:path`，其中`path`是WKND冒险（内容片段）的路径，用于显示有关的详细信息。

## 将SPA URL映射到AEM页面

在前两章中，我们将可编辑的组件内容从SPA主视图映射到AEM中位于`/content/wknd-app/us/en/`的相应Remote SPA根页面。

为SPA动态路由的可编辑组件定义映射很相似，但是，在路由实例和AEM页面之间，我们必须提出1:1映射方案。

在本教程中，我们将采用WKND Adventure Content Fragment的名称，它是路径的最后一段，并将其映射到`/content/wknd-app/us/en/adventure`下的简单路径。

| 远程SPA路由 | AEM页面路径 |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/en/home |
| /adventure:/content/dam/wknd/en/adventures/bali-surf-camp/__bali-surf-camp__ | /content/wknd-app/us/cn/home/adventure/__bali-surf-camp__ |
| /adventure:/content/dam/wknd/en/adventures/beervana-portland/__beervana-portland__ | /content/wknd-app/us/en/home/adventure/__beervana-in-portland__ |

因此，根据此映射，我们必须在以下位置创建两个新的AEM页面：

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## 远程SPA映射

通过在SPA](./spa-bootstrap.md)Bootstrap中完成的[配置配置离开远程SPA的请求的映射。`setupProxy`

## SPA Editor映射

通过AEM SPA编辑器打开SPA时的SPA请求映射是通过在[Configure AEM](./aem-configure.md)中完成的Sling映射配置进行配置的。

## 在AEM中创建内容页面

首先，创建中间`adventure`页面区段：

1. 登录到AEM作者
1. 导航到&#x200B;__站点> WKND应用程序>我们> en > WKND应用程序主页__
   + 此AEM页面将映射为SPA的根，因此我们将开始在此构建其他SPA路由的AEM页面结构。
1. 点按&#x200B;__创建__&#x200B;并选择&#x200B;__页面__
1. 选择&#x200B;__远程SPA页面__&#x200B;模板，然后点按&#x200B;__下一步__
1. 填写页面属性
   + __标题__:冒险
   + __名称__: `adventure`
      + 此值定义AEM页面的URL，因此必须与SPA的路由区段匹配。
1. 点按&#x200B;__完成__

然后，创建与每个需要可编辑区域的SPA URL相对应的AEM页面。

1. 导航到站点管理员中新的&#x200B;__Adventure__&#x200B;页面
1. 点按&#x200B;__创建__&#x200B;并选择&#x200B;__页面__
1. 选择&#x200B;__远程SPA页面__&#x200B;模板，然后点按&#x200B;__下一步__
1. 填写页面属性
   + __标题__:巴厘岛冲浪营
   + __名称__: `bali-surf-camp`
      + 此值定义AEM页面的URL，因此必须与SPA路由的最后一个段匹配
1. 点按&#x200B;__完成__
1. 重复步骤3-6，在Portland __中创建__ Beervana页面，其中包括：
   + __标题__:贝尔瓦娜在波特兰
   + __名称__: `beervana-in-portland`
      + 此值定义AEM页面的URL，因此必须与SPA路由的最后一个段匹配

这两个AEM页面分别包含为其匹配的SPA路由创作的内容。 如果其他SPA路由需要创作，则必须在AEM中远程SPA页面的根页面(`/content/wknd-app/us/en/home`)下在其SPA URL中创建新的AEM页面。

## 更新WKND应用程序

让我们将在[最后一章](./spa-container-component.md)中创建的`<AEMResponsiveGrid...>`组件放入我们的`AdventureDetail` SPA组件中，创建可编辑的容器。

### 放置AEMResponsiveGrid SPA组件

将`<AEMResponsiveGrid...>`放置到`AdventureDetail`组件会在该路由中创建可编辑容器。 技巧是因为多条路由使用`AdventureDetail`组件进行渲染，因此我们必须动态调整`<AEMResponsiveGrid...>'s pagePath`属性。 `pagePath`必须根据路由实例显示的探险次数派生到相应的AEM页面。

1. 打开并编辑`react-app/src/components/AdventureDetail.js`
1. 在`AdventureDetail(..)'s`第二个`return(..)`语句之前添加以下行，该语句从内容片段路径派生冒险名称。

   ```
   ...
   // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
   const adventureName = adventureData._path.split('/').pop();
   ...
   ```

1. 导入`AEMResponsiveGrid`组件并将其放在`<h2>Itinerary</h2>`组件上方。
1. 在`<AEMResponsiveGrid...>`组件上设置以下属性
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${adventureName}'`
   + `itemPath = 'root/responsivegrid'`

   这指示`AEMResponsiveGrid`组件从AEM资源检索其内容：

   + `/content/wknd-app/us/en/home/adventure/${adventureName}/jcr:content/root/responsivegrid`


使用以下行更新`AdventureDetail.js`:

```
...
import AEMResponsiveGrid from '../components/aem/AEMResponsiveGrid';
...

function AdventureDetail(props) {
    ...
    // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
    const adventureName = adventureData._path.split('/').pop();

    return(
        ...
        <AEMResponsiveGrid 
            pagePath={`/content/wknd-app/us/en/home/adventure/${adventureName}`}
            itemPath="root/responsivegrid"/>
            
        <h2>Itinerary</h2>
        ...
    )
}
```

`AdventureDetail.js`文件应该如下：

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## 在AEM中创作容器

将`<AEMResponsiveGrid...>`放置到位，并根据渲染的冒险事件动态设置其`pagePath`后，我们会尝试在其中创作内容。

1. 登录到AEM作者
1. 导航到&#x200B;__站点> WKND应用程序>我们> en__
1. __编__ 辑WKND应 __用程序主__ 页
   + 导航到SPA中的&#x200B;__巴厘岛冲浪营__&#x200B;路线进行编辑
1. 从右上方的mode-selector中选择&#x200B;__预览__
1. 点击SPA中的&#x200B;__Bali Surf Camp__&#x200B;卡以导航至其路由
1. 从模式选择器中选择&#x200B;__编辑__
1. 找到&#x200B;__布局容器__&#x200B;可编辑区域，位于&#x200B;__行程表__&#x200B;的正上方
1. 打开&#x200B;__页面编辑器的侧栏__，然后选择&#x200B;__组件视图__
1. 将某些已启用的组件拖入&#x200B;__布局容器__
   + 图像
   + 文本
   + 标题

   并创建一些促销营销资料。 它可能是这样的：

   ![Bali Adventure Detail Authoring](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. __在AEM__ Page Editor中预览您所做的更改
1. 刷新在[http://localhost:3000](http://localhost:3000)上本地运行的WKND应用程序，导航到&#x200B;__Bali Surf Camp__&#x200B;路由以查看创作的更改！

   ![远程SPA Bali](./assets/spa-dynamic-routes/remote-spa-final.png)

当导航到没有映射的AEM页面的冒险详细信息路由时，该路由实例将不具备创作功能。 要在这些页面上启用创作功能，只需在&#x200B;__Adventure__&#x200B;页面下制作一个具有匹配名称的AEM页面！

## 恭喜！

恭喜！ 您已在SPA中为动态路由添加了创作功能！

+ 已将AEM React Editable组件的ResponsiveGrid组件添加到动态路由
+ 创建AEM页以支持在SPA中创作两条特定路线（Bali Surf Camp和Beervana，波特兰）
+ 在动态的巴厘岛冲浪夏令营路线上创作内容！

您现在已完成了对如何使用AEM SPA编辑器将特定可编辑区域添加到远程SPA的第一步的探索！


>[!NOTE]
>
>让我们拭目以待！ 本教程将扩展，以涵盖Adobe关于如何将SPA Editor解决方案部署到AEM作为Cloud Service和生产环境的最佳实践和建议。