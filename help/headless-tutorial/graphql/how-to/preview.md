---
title: 内容片段预览
description: 了解如何向所有作者使用内容片段预览来快速了解内容更改对AEM无头体验有何影响。
version: Cloud Service
feature: Content Fragments
topic: Headless, Content Management, Development
role: Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-03-17T00:00:00Z
jira: KT-10841
thumbnail: 3416906.jpeg
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 0%

---


# 内容片段预览

AEM无头应用程序支持集成的创作预览。 预览体验可将AEM作者的内容片段编辑器与您的自定义应用程序（可通过HTTP寻址）链接起来，从而允许在应用程序中提供一个深层链接，以呈现正在预览的内容片段。

>[!VIDEO](https://video.tv.adobe.com/v/3416906?quality=12&learn=on)

要使用内容片段预览，必须满足以下几个条件：

1. 必须将应用程序部署到作者可访问的URL
1. 必须将应用程序配置为连接到AEM创作服务（而不是AEM发布服务）
1. 应用程序必须使用可使用的URL或路由进行设计 [内容片段路径或ID](#url-expressions) ，以选择要在应用程序体验中进行预览的内容片段。

## 预览URL

预览URL，使用 [URL表达式](#url-expressions)，在内容片段模型的属性中设置。

![内容片段模型预览URL](./assets/preview/cf-model-preview-url.png)

1. 以管理员身份登录到AEM创作服务
1. 导航到 __工具>常规>内容片段模型__
1. 选择 __内容片段模型__ 选择 __属性__ 按钮。
1. 使用 [URL表达式](#url-expressions)
   + 预览URL必须指向连接到AEM创作服务的应用程序部署。

### URL表达式

每个内容片段模型都可以设置一个预览URL。 可以使用下表中列出的URL表达式，为每个内容片段参数化预览URL。 可在单个预览URL中使用多个URL表达式。

|  | URL表达式 | 价值 |
| --------------------------------------- | ----------------------------------- | ----------- |
| 内容片段路径 | `${contentFragment.path}` | `/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali` |
| 内容片段ID | `${contentFragment.id}` | `12c34567-8901-2aa3-45b6-d7890aa1c23c` |
| 内容片段变量 | `${contentFragment.variation}` | `main` |
| 内容片段模型路径 | `${contentFragment.model.path}` | `/conf/wknd-shared/settings/dam/cfm/models/adventure` |
| 内容片段模型名称 | `${contentFragment.model.name}` | `adventure` |

预览URL示例：

+ 上的预览URL __冒险__ 模型看起来 `https://preview.app.wknd.site/adventure${contentFragment.path}` 解析为 `https://preview.app.wknd.site/adventure/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali`
+ 上的预览URL __文章__ 模型看起来 `https://preview.news.wknd.site/${contentFragment.model.name}/${contentFragment.id}.html?variation=${contentFragment.variation}` 解析 `https://preview.news.wknd.site/article/99c34317-1901-2ab3-35b6-d7890aa1c23c.html?variation=main`

## 应用程序内预览

使用配置的内容片段模型的任何内容片段都具有“预览”按钮。 “预览”按钮可打开内容片段模型的预览URL，并将打开的内容片段的值插入到 [URL表达式](#url-expressions).

![“预览”按钮](./assets/preview/preview-button.png)

在应用程序中预览内容片段更改时，执行硬刷新（清除浏览器的本地缓存）。

## React示例

让我们来探索WKND应用程序，这是一个简单的React应用程序，可显示AEM在使用AEM Headless GraphQL API时的冒险经历。

示例代码在 [Github.com](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/preview-tutorial).

## URL和路由

用于预览内容片段的URL或路由必须可以使用 [URL表达式](#url-expressions). 在此WKND应用程序的启用预览的版本中，冒险内容片段通过 `AdventureDetail` 绑定到路线的组件 `/adventure<CONTENT FRAGMENT PATH>`. 因此，必须将WKND Adventure模型的预览URL设置为 `https://preview.app.wknd.site:3000/adventure${contentFragment.path}` 来解析此路线。

仅当应用程序具有可寻址路由（可以填充）时，内容片段预览才起作用 [URL表达式](#url-expressions) 以可预览的方式在应用程序中呈现该内容片段。

+ `src/App.js`

```javascript
...
function App() {
  return (
    <Router>
      <div className="App">
        <header>
            <Link to={"/"}>
                <img src={logo} className="logo" alt="WKND Logo"/>
            </Link>        
            <hr />
        </header>
        <Switch>
          {/* The route's path must match the Adventure Model's Preview URL expression. In React since the path has `/` you must use wildcards to match instead of the usual `:path` */}
          <Route path='/adventure/*'>
            <AdventureDetail />
          </Route>
          <Route path="/">
            <Home />
          </Route>
        </Switch>
      </div>
    </Router>
  );
}

export default App;
```

### 显示创作内容

的 `AdventureDetail` 组件只会解析通过 `${contentFragment.path}` [URL表达式](#url-expressions)，并使用它收集和渲染WKND冒险。

+ `src/components/AdventureDetail.js`

```javascript
...
function AdventureDetail() {

    // Read the content fragment path value which is the parameter used to query for the adventure's details
    
    // Add the leading '/' back on since the params value captures the `*` wildcard in `/adventure/*`, or everything after the first `/` in the Content Fragment path.
    const path = '/' + useParams()[0];

    // Query AEM for the Adventures's details, using the Content Fragment's `path`
    const { adventure, references, error } = useAdventureByPath(path);

    // Handle error and loading conditions
    if (error) {
        return <Error errorMessage={error} />;
    } else if (!adventure) {
        return <Loading />;
    }

    return (<div className="adventure-detail">
        ...
        <AdventureDetailRender {...adventure} references={references} />
    </div>);
}
...
```
