---
title: 将可编辑的组件添加到远程SPA动态路由
description: 了解如何将可编辑的组件添加到远程SPA中的动态路由。
topic: 无头、SPA、开发
feature: SPA编辑器，核心组件， API，开发
role: Developer, Architect
level: Beginner
kt: 7636
thumbnail: kt-7636.jpeg
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '958'
ht-degree: 0%

---


# 动态路由和可编辑的组件

在本章中，我们启用了两条动态冒险详细信息路径，以支持可编辑的组件；__波特兰的巴厘岛冲浪营__&#x200B;和&#x200B;__贝尔瓦纳__。

![动态路由和可编辑的组件](./assets/spa-dynamic-routes/intro.png)

冒险详细信息SPA路由定义为`/adventure:path`，其中`path`是WKND冒险（内容片段）的路径，用于显示有关的详细信息。

## 将SPA URL映射到AEM页面

在前两章中，我们将可编辑的组件内容从SPA主页视图映射到AEM中`/content/wknd-app/us/en/`的相应Remote SPA根页面。

为SPA动态路由的可编辑组件定义映射相似，但我们必须在路由实例和AEM页面之间提出1:1映射方案。

在本教程中，我们将采用WKND冒险内容片段的名称（路径的最后一段），并将其映射到`/content/wknd-app/us/en/adventure`下的简单路径。

| 远程SPA路由 | AEM页面路径 |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/en/home |
| /adventure:/content/dam/wknd/en/adventures/bali-surf-camp/__bali-surf-camp__ | /content/wknd-app/us/en/home/adventure/__bali-surf-camp__ |
| /adventure:/content/dam/wknd/en/adventures/beervana-portland/__beervana-portland__ | /content/wknd-app/us/en/home/adventure/__beervana-in-portland__ |

因此，根据此映射，我们必须在以下位置创建两个新的AEM页面：

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## 远程SPA映射

离开远程SPA的请求的映射是通过在[中BootstrapSPA](./spa-bootstrap.md)中完成的`setupProxy`配置进行的。

## SPA编辑器映射

通过AEM SPA编辑器打开SPA时，SPA请求的映射是通过在[Configure AEM](./aem-configure.md)中完成的Sling映射配置来配置的。

## 在AEM中创建内容页面

首先，创建中间`adventure`页面区段：

1. 登录到AEM作者
1. 导航到&#x200B;__站点> WKND应用程序>使用> en > WKND应用程序主页__
   + 此AEM页面被映射为SPA的根，因此我们开始在此构建其他SPA路由的AEM页面结构。
1. 点按&#x200B;__创建__&#x200B;并选择&#x200B;__页面__
1. 选择&#x200B;__远程SPA页面__&#x200B;模板，然后点按&#x200B;__下一步__
1. 填写页面属性
   + __标题__:冒险
   + __名称__: `adventure`
      + 此值定义AEM页面的URL，因此必须匹配SPA的路由区段。
1. 点按&#x200B;__Done__

然后，创建与每个需要可编辑区域的SPA URL对应的AEM页面。

1. 导航到站点管理员中新的&#x200B;__Adventure__&#x200B;页面
1. 点按&#x200B;__创建__&#x200B;并选择&#x200B;__页面__
1. 选择&#x200B;__远程SPA页面__&#x200B;模板，然后点按&#x200B;__下一步__
1. 填写页面属性
   + __标题__:巴厘岛冲浪营
   + __名称__: `bali-surf-camp`
      + 此值定义AEM页面的URL，因此必须匹配SPA路径的最后一个区段
1. 点按&#x200B;__Done__
1. 重复步骤3-6以在Portland __中创建__ Beervana页面，其中：
   + __标题__:贝尔瓦娜在波特兰
   + __名称__: `beervana-in-portland`
      + 此值定义AEM页面的URL，因此必须匹配SPA路径的最后一个区段

这两个AEM页面分别保存其匹配SPA路由的创作内容。 如果其他SPA路由需要创作，则必须在其SPA URL的AEM远程SPA页面的根页面(`/content/wknd-app/us/en/home`)下创建新的AEM页面。

## 更新WKND应用程序

让我们将在[最后一章](./spa-container-component.md)中创建的`<AEMResponsiveGrid...>`组件放入我们的`AdventureDetail` SPA组件中，创建一个可编辑的容器。

### 放置AEMResponsiveGrid SPA组件

将`<AEMResponsiveGrid...>`放置在`AdventureDetail`组件中，将在该路由中创建一个可编辑的容器。 诀窍在于，由于多条路由使用`AdventureDetail`组件进行渲染，因此我们必须动态调整`<AEMResponsiveGrid...>'s pagePath`属性。 必须根据路由实例显示的历程，派生`pagePath`以指向相应的AEM页面。

1. 打开并编辑`react-app/src/components/AdventureDetail.js`
1. 在`AdventureDetail(..)'s`第二个`return(..)`语句之前添加以下行，该语句从内容片段路径派生冒险名称。

   ```
   ...
   // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
   const adventureName = adventureData._path.split('/').pop();
   ...
   ```

1. 导入`AEMResponsiveGrid`组件，并将其放在`<h2>Itinerary</h2>`组件上方。
1. 在`<AEMResponsiveGrid...>`组件中设置以下属性
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${adventureName}'`
   + `itemPath = 'root/responsivegrid'`

   它会指示`AEMResponsiveGrid`组件从AEM资源中检索其内容：

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

`AdventureDetail.js`文件应该如下所示：

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## 在AEM中创作容器

在`<AEMResponsiveGrid...>`到位且其`pagePath`根据所呈现的历程动态设置后，我们会尝试在其中创作内容。

1. 登录到AEM作者
1. 导航至&#x200B;__站点> WKND应用程序>使用> en__
1. ____ 编辑WKND __应用程序主__ 页
   + 导航到SPA中的&#x200B;__Bali Surf Camp__&#x200B;路由以对其进行编辑
1. 从右上方的模式选择器中选择&#x200B;__预览__
1. 点按SPA中的&#x200B;__Bali Surf Camp__&#x200B;卡以导航到其路径
1. 从模式选择器中选择&#x200B;__编辑__
1. 在&#x200B;__行程__&#x200B;的正上方找到&#x200B;__布局容器__&#x200B;可编辑区域
1. 打开&#x200B;__页面编辑器的侧栏__，然后选择&#x200B;__组件视图__
1. 将一些已启用的组件拖到&#x200B;__布局容器__&#x200B;中
   + 图像
   + 文本
   + 标题

   并制作一些促销营销材料。 它可能如下所示：

   ![《巴厘岛冒险细节创作》](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. ____ 在AEM页面编辑器中预览所做的更改
1. 刷新在[http://localhost:3000](http://localhost:3000)上本地运行的WKND应用程序，导航到&#x200B;__Bali Surf Camp__&#x200B;路由以查看创作的更改！

   ![远程SPA Bali](./assets/spa-dynamic-routes/remote-spa-final.png)

当导航到没有映射AEM页面的探险详细信息路由时，该路由实例将不具备创作功能。 要在这些页面上启用创作功能，只需在&#x200B;__Adventure__&#x200B;页面下创建一个具有匹配名称的AEM页面！

## 恭喜！

恭喜！ 您在SPA中为动态路由添加了创作功能！

+ 将AEM React可编辑组件的ResponsiveGrid组件添加到动态路由
+ 创建AEM页面，以支持在SPA中创作两条特定路线（Bali Surf Camp和Beervana，波特兰）
+ 在动态的巴厘岛冲浪营路线上创作了内容！

您现在已完成探索如何使用AEM SPA Editor向远程SPA添加特定可编辑区域的首要步骤！


>[!NOTE]
>
>继续看！ 本教程将扩展，涵盖Adobe有关如何将SPA Editor解决方案部署到AEM as a Cloud Service和生产环境的最佳实践和建议。