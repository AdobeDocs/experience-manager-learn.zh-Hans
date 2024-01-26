---
title: 将可编辑的组件添加到远程SPA动态路由
description: 了解如何将可编辑的组件添加到远程SPA中的动态路由。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7636
thumbnail: kt-7636.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: 4accc1ca-6f4b-449e-bf2e-06f19d2fe17d
duration: 260
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '903'
ht-degree: 0%

---

# 动态路由和可编辑组件

在本章中，我们启用两种动态冒险详细信息路由来支持可编辑组件； __巴厘岛冲浪营__ 和 __贝尔瓦纳在波特兰__.

![动态路由和可编辑组件](./assets/spa-dynamic-routes/intro.png)

Adventure Detail SPA路由定义为 `/adventure/:slug` 位置 `slug` 是冒险内容片段上的唯一标识符属性。

## 将SPA URL映射到AEM页面

在前两章中，我们将可编辑的组件内容从SPA主页视图映射到AEM中对应的远程SPA根页面，网址为 `/content/wknd-app/us/en/`.

为SPA动态路由的可编辑组件定义映射类似，但我们必须制定路由实例与AEM页面之间的一对一映射方案。

在本教程中，我们将WKND冒险内容片段的名称作为路径的最后一个区段，并将其映射到下的简单路径 `/content/wknd-app/us/en/adventure`.

| 远程SPA路由 | AEM页面路径 |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/en/home |
| /adventure/__巴厘岛冲浪营__ | /content/wknd-app/us/en/home/adventure/__巴厘岛冲浪营__ |
| /adventure/__贝尔瓦纳 — 波特兰__ | /content/wknd-app/us/en/home/adventure/__波特兰贝尔瓦纳__ |

因此，基于此映射，我们必须在以下位置创建两个新的AEM页面：

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## 远程SPA映射

对于离开远程SPA的请求，可通过以下方式配置映射： `setupProxy` 配置完成于 [BootstrapSPA](./spa-bootstrap.md).

## SPA编辑器映射

通过SPA SPA Editor打开SPA时AEM请求的映射可通过在中完成的Sling映射配置进行配置 [配置AEM](./aem-configure.md).

## 在AEM中创建内容页面

首先，创建中介 `adventure` 页面区段：

1. 登录AEM Author
1. 导航到 __Sites > WKND应用程序>我们> en > WKND应用程序主页__
   + 此AEM页面被映射为SPA的根，因此我们将在此处开始为其他SPA路由构建AEM页面结构。
1. 点按 __创建__ 并选择 __页面__
1. 选择 __远程SPA页__ 模板，然后点按 __下一个__
1. 填写页面属性
   + __标题__：冒险
   + __名称__：`adventure`
      + 此值定义AEM页面的URL，因此必须匹配SPA的路由段。
1. 点按 __完成__

然后，创建与每个需要可编辑区域的SPA URL相对应的AEM页面。

1. 导航到新的 __冒险__ “网站管理员”中的页面
1. 点按 __创建__ 并选择 __页面__
1. 选择 __远程SPA页__ 模板，然后点按 __下一个__
1. 填写页面属性
   + __标题__：巴厘岛冲浪营
   + __名称__：`bali-surf-camp`
      + 此值定义AEM页面的URL，因此必须匹配SPA路由的最后一个区段
1. 点按 __完成__
1. 重复步骤3 - 6以创建 __贝尔瓦纳在波特兰__ 页面，替换为：
   + __标题__：贝尔瓦纳在波特兰
   + __名称__：`beervana-in-portland`
      + 此值定义AEM页面的URL，因此必须匹配SPA路由的最后一个区段

这两个AEM页包含其匹配的SPA路由的各自编写的内容。 如果其他SPA路由需要创作，则必须在远程AEMSPA SPA页面的根页面(`/content/wknd-app/us/en/home`AEM )。

## 更新WKND应用程序

我们来放置 `<ResponsiveGrid...>` 在中创建的组件 [上一章](./spa-container-component.md)，进入我们的 `AdventureDetail` SPA组件，创建可编辑的容器。

### 放置ResponsiveGrid SPA组件

将 `<ResponsiveGrid...>` 在 `AdventureDetail` 组件在该工艺路线中创建一个可编辑的容器。 诀窍在于多条路由都使用 `AdventureDetail` 要呈现的组件，我们必须动态调整  `<ResponsiveGrid...>'s pagePath` 属性。 此 `pagePath` 必须派生以根据路由实例显示的冒险指向相应的AEM页面。

1. 打开并编辑 `react-app-/src/components/AdventureDetail.js`
1. 导入 `ResponsiveGrid` 组件并将其放在 `<h2>Itinerary</h2>` 组件。
1. 在 `<ResponsiveGrid...>` 组件。 请注意 `pagePath` 属性添加当前 `slug` 会按照上面定义的映射来映射到冒险页面。
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${slug}'`
   + `itemPath = 'root/responsivegrid'`

   这说明 `ResponsiveGrid` 从AEM资源检索其内容的组件：

   + `/content/wknd-app/us/en/home/adventure/${slug}/jcr:content/root/responsivegrid`

更新 `AdventureDetail.js` 包含以下行：

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

此 `AdventureDetail.js` 文件应如下所示：

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## 在AEM中创作容器

使用 `<ResponsiveGrid...>` 就位，及其 `pagePath` 根据呈现的冒险动态设置，我们尝试在其中创作内容。

1. 登录AEM Author
1. 导航到 __Sites > WKND应用程序>我们> en__
1. __编辑__ 该 __WKND应用程序主页__ 页面
   + 导航至 __巴厘岛冲浪营__ 在SPA中路由以编辑它
1. 选择 __预览__ 从右上角的模式选择器中
1. 点击 __巴厘岛冲浪营__ SPA中用于导航到其路由的卡
1. 选择 __编辑__ 从模式选择器中
1. 找到 __布局容器__ 上方的可编辑区域 __日程表__
1. 打开 __页面编辑器的侧栏__，然后选择 __“组件”视图__
1. 将某些启用的组件拖动到 __布局容器__
   + 图像
   + 文本
   + 标题

   并创建一些促销营销材料。 它可能会看起来像这样：

   ![Bali Adventure Detail创作](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. __预览__ 在AEM页面编辑器中进行的更改
1. 刷新本地运行的WKND应用程序 [http://localhost:3000](http://localhost:3000)，导航到 __巴厘岛冲浪营__ 查看已编写更改的路由！

   ![远程SPA巴厘岛](./assets/spa-dynamic-routes/remote-spa-final.png)

当导航到没有映射AEM页面的冒险详细信息路由时，该路由实例没有创作功能。 AEM要在这些页面上启用创作功能，只需在 __冒险__ 页！

## 恭喜！

恭喜！您已添加在SPA中动态路由的创作功能！

+ 将AEM React可编辑组件的ResponsiveGrid组件添加到动态路由
+ 创建了AEM页面，以支持在SPA中创作两条特定路径（波特兰的Bali Surf Camp和Beervana）
+ 编写了关于巴厘岛冲浪营线路的内容！

您现在已经完成了探索如何使用AEM SPA Editor将特定可编辑区域添加到远程SPA的第一步！
