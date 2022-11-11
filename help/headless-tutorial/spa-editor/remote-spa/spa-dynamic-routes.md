---
title: 将可编辑的组件添加到远程SPA动态路由
description: 了解如何将可编辑的组件添加到远程SPA中的动态路由。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7636
thumbnail: kt-7636.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: 4accc1ca-6f4b-449e-bf2e-06f19d2fe17d
source-git-commit: ece15ba61124972bed0667738ccb37575d43de13
workflow-type: tm+mt
source-wordcount: '901'
ht-degree: 1%

---

# 动态路由和可编辑的组件

在本章中，我们启用了两条动态冒险详细信息路径，以支持可编辑的组件； __巴厘岛冲浪营__ 和 __贝尔瓦娜在波特兰__.

![动态路由和可编辑的组件](./assets/spa-dynamic-routes/intro.png)

Adventure Detail SPA路由定义为 `/adventure/:slug` where `slug` 是冒险内容片段中的唯一标识符属性。

## 将SPA URL映射到AEM页面

在前两章中，我们将可编辑的组件内容从SPA主页视图映射到AEM中相应的远程SPA根页面(位于 `/content/wknd-app/us/en/`.

为SPA动态路由的可编辑组件定义映射相似，但我们必须在路由实例和AEM页面之间提出1:1映射方案。

在本教程中，我们将WKND冒险内容片段（路径的最后一段）的名称映射到下的简单路径 `/content/wknd-app/us/en/adventure`.

| 远程SPA路由 | AEM页面路径 |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/en/home |
| /adventure/__巴厘岛冲浪营__ | /content/wknd-app/us/en/home/adventure/__巴厘岛冲浪营__ |
| /adventure/__贝尔瓦纳 — 波特兰__ | /content/wknd-app/us/en/home/adventure/__贝尔瓦纳因波特兰__ |

因此，根据此映射，我们必须在以下位置创建两个新的AEM页面：

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## 远程SPA映射

离开远程SPA的请求映射通过 `setupProxy` 配置完成 [BootstrapSPA](./spa-bootstrap.md).

## SPA编辑器映射

通过AEM SPA编辑器打开SPA时，SPA请求的映射是通过 [配置AEM](./aem-configure.md).

## 在AEM中创建内容页面

首先，创建中间人 `adventure` 页面区段：

1. 登录到AEM作者
1. 导航到 __站点> WKND应用程序>用户> en > WKND应用程序主页__
   + 此AEM页面被映射为SPA的根，因此我们开始在此构建其他SPA路由的AEM页面结构。
1. 点按 __创建__ 选择 __页面__
1. 选择 __远程SPA页__ 模板，然后点按 __下一个__
1. 填写页面属性
   + __标题__:冒险
   + __名称__: `adventure`
      + 此值定义AEM页面的URL，因此必须匹配SPA的路由区段。
1. 点按 __完成__

然后，创建与每个需要可编辑区域的SPA URL对应的AEM页面。

1. 导航到新 __冒险__ 站点管理员中的页面
1. 点按 __创建__ 选择 __页面__
1. 选择 __远程SPA页__ 模板，然后点按 __下一个__
1. 填写页面属性
   + __标题__:巴厘岛冲浪营
   + __名称__: `bali-surf-camp`
      + 此值定义AEM页面的URL，因此必须匹配SPA路径的最后一个区段
1. 点按 __完成__
1. 重复步骤3-6以创建 __贝尔瓦娜在波特兰__ 页面，其中：
   + __标题__:贝尔瓦娜在波特兰
   + __名称__: `beervana-in-portland`
      + 此值定义AEM页面的URL，因此必须匹配SPA路径的最后一个区段

这两个AEM页面包含其各自创作的内容，以供其匹配的SPA路由使用。 如果其他SPA路由需要创作，则必须在其SPA URL的远程SPA页面根页面(`/content/wknd-app/us/en/home`)。

## 更新WKND应用程序

我们把 `<ResponsiveGrid...>` 在中创建的组件 [最后一章](./spa-container-component.md)，到 `AdventureDetail` SPA组件，创建可编辑的容器。

### 放置ResponsiveGrid SPA组件

将 `<ResponsiveGrid...>` 在 `AdventureDetail` 组件会在该路径中创建一个可编辑的容器。 诀窍在于多条路线使用 `AdventureDetail` 要渲染的组件，我们必须动态调整  `<ResponsiveGrid...>'s pagePath` 属性。 的 `pagePath` 必须根据路由实例显示的历程派生，指向相应的AEM页面。

1. 打开和编辑 `react-app-/src/components/AdventureDetail.js`
1. 导入 `ResponsiveGrid` 组件并将其放在 `<h2>Itinerary</h2>` 组件。
1. 在 `<ResponsiveGrid...>` 组件。 请注意 `pagePath` 属性会添加当前 `slug` 根据上面定义的映射，该页面映射到冒险页面。
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${slug}'`
   + `itemPath = 'root/responsivegrid'`

   这会指示 `ResponsiveGrid` 用于从AEM资源检索其内容的组件：

   + `/content/wknd-app/us/en/home/adventure/${slug}/jcr:content/root/responsivegrid`


更新 `AdventureDetail.js` ，其中包含以下行：

```javascript
...
import { ResponsiveGrid } from '@adobe/aem-react-editable-components';
...

function AdventureDetailRender(props) {
    ...
    // Get the slug from the React route parameter, this will be used to specify the AEM Page to store/read editable content from
    const { slug } = useParams();

    return(
        ...
        // Pass the slug in
        function AdventureDetailRender({ title, primaryImage, activity, adventureType, tripLength, 
                groupSize, difficulty, price, description, itinerary, references, slug }) {
            ...
            return (
                ...
                <ResponsiveGrid 
                    pagePath={`/content/wknd-app/us/en/home/adventure/${slug}`}
                    itemPath="root/responsivegrid"/>
                    
                <h2>Itinerary</h2>
                ...
            )
        }
    )
}
```

的 `AdventureDetail.js` 文件应该如下所示：

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## 在AEM中创作容器

使用 `<ResponsiveGrid...>` 就位，以及 `pagePath` 我们会根据所呈现的历程进行动态设置，并尝试在其中创作内容。

1. 登录到AEM作者
1. 导航到 __站点> WKND应用程序>美国> en__
1. __编辑__ the __WKND应用程序主页__ 页面
   + 导航到 __巴厘岛冲浪营__ 在SPA中路由以对其进行编辑
1. 选择 __预览__ 从右上方的模式选择器中
1. 点按 __巴厘岛冲浪营__ 卡以导航到SPA的路线
1. 选择 __编辑__ 从模式选择器
1. 找到 __布局容器__ 可编辑区域，就在 __行程__
1. 打开 __页面编辑器的侧栏__，然后选择 __组件视图__
1. 将一些已启用的组件拖到 __布局容器__
   + 图像
   + 文本
   + 标题

   并制作一些促销营销材料。 它可能如下所示：

   ![《巴厘岛冒险细节创作》](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. __预览__ 您对AEM页面编辑器所做的更改
1. 刷新在本地运行的WKND应用程序 [http://localhost:3000](http://localhost:3000)，导航到 __巴厘岛冲浪营__ 路由以查看创作的更改！

   ![远程SPA Bali](./assets/spa-dynamic-routes/remote-spa-final.png)

当导航到没有映射AEM页面的探险详细信息路由时，该路由实例上没有创作功能。 要在这些页面上启用创作功能，只需在 __冒险__ 页面！

## 恭喜！

恭喜！您在SPA中为动态路由添加了创作功能！

+ 将AEM React可编辑组件的ResponsiveGrid组件添加到动态路由
+ 创建AEM页面，以支持在SPA中创作两条特定路线（Bali Surf Camp和Beervana，波特兰）
+ 在动态的巴厘岛冲浪营路线上创作了内容！

您现在已完成探索如何使用AEM SPA Editor向远程SPA添加特定可编辑区域的首要步骤！
