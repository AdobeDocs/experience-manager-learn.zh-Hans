---
title: 使用通用编辑器检测React应用程序以编辑内容
description: 了解如何使用通用编辑器检测React应用程序以编辑内容。
version: Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
source-git-commit: 14767141348d3d56c154704cc21d39722bb67aec
workflow-type: tm+mt
source-wordcount: '1606'
ht-degree: 0%

---


# 使用通用编辑器检测React应用程序以编辑内容

了解如何使用通用编辑器检测React应用程序以编辑内容。

## 先决条件

您已经按照前面所述设置了本地开发环境 [本地开发设置](./local-development-setup.md) 步骤。

## 包含通用编辑器核心库

让我们从WKND团队React应用程序中包含Universal Editor核心库开始。 它是一个JavaScript库，提供已编辑应用程序和通用编辑器之间的通信层。

有两种方法可以在React应用程序中包含Universal Editor核心库：

1. npm注册表中的节点模块依赖关系，请参见 [@adobe/universal-editor-cors](https://www.npmjs.com/package/@adobe/universal-editor-cors).
1. 脚本标记(`<script>`HTML )。

在本教程中，让我们使用脚本标记方法。

1. 安装 `react-helmet-async` 用于管理 `<script>` 标记时，不会将反向链接计算两次。

   ```bash
   $ npm install react-helmet-async
   ```

1. 更新 `src/App.js` WKND Teams React应用程序的文件以包含通用编辑器核心库。

   ```javascript
   ...
   import { Helmet, HelmetProvider } from "react-helmet-async";
   
   function App() {
   return (
       <HelmetProvider>
           <div className="App">
               <Helmet>
                   {/* AEM Universal Editor :: CORE Library
                     Loads the LATEST Universal Editor library
                   */}
                   <script
                       src="https://universal-editor-service.experiencecloud.live/corslib/LATEST"
                       async
                   />
               </Helmet>
               <Router>
                   <header>
                       <Link to={"/"}>
                       <img src={logo} className="logo" alt="WKND Logo" />
                       </Link>
                       <hr />
                   </header>
                   <Routes>
                       <Route path="/" element={<Home />} />
                       <Route path="/person/:fullName" element={<Person />} />
                   </Routes>
               </Router>
           </div>
       </HelmetProvider>
   );
   }
   
   export default App;
   ```

## 添加元数据 — 内容源

连接WKND Teams React应用程序 _使用内容源_ 要进行编辑，您需要提供连接元数据。 Universal Editor服务使用此元数据与内容源建立连接。

连接元数据存储为 `<meta>` 标签中HTML的数据。 连接元数据的语法如下：

```html
<meta name="urn:adobe:aue:<category>:<referenceName>" content="<protocol>:<url>">
```

让我们将连接元数据添加到WKND Teams React应用程序中的 `<Helmet>` 组件。 更新 `src/App.js` 包含以下内容的文件 `<meta>` 标记之前。 在此示例中，内容源是运行于的本地AEM实例 `https://localhost:8443`.

```javascript
...
function App() {
return (
    <HelmetProvider>
        <div className="App">
            <Helmet>
                {/* AEM Universal Editor :: CORE Library
                    Loads the LATEST Universal Editor library
                */}
                <script
                    src="https://universal-editor-service.experiencecloud.live/corslib/LATEST"
                    async
                />
                {/* AEM Universal Editor :: Connection metadata 
                    Connects to local AEM instance
                */}
                <meta
                    name="urn:adobe:aue:system:aemconnection"
                    content={`aem:https://localhost:8443`}
                />
            </Helmet>
            ...
    </HelmetProvider>
);
}

export default App;
```

此 `aemconnection` 提供内容源的简短名称。 后续检测使用短名称来引用内容源。

## 添加元数据 — 本地通用编辑器服务配置

与Adobe托管的通用编辑器服务不同，使用通用编辑器服务的本地副本进行本地开发。 本地服务绑定通用编辑器和AEM SDK，因此让我们将本地通用编辑器服务元数据添加到WKND Teams React应用程序。

这些配置设置还存储为 `<meta>` 标签中HTML的数据。 本地Universal Editor服务元数据的语法如下：

```html
<meta name="urn:adobe:aue:config:service" content="<url>">
```

让我们将连接元数据添加到WKND Teams React应用程序中的 `<Helmet>` 组件。 更新 `src/App.js` 包含以下内容的文件 `<meta>` 标记之前。 在此示例中，本地Universal Editor服务运行在 `https://localhost:8001`.

```javascript
...

function App() {
  return (
    <HelmetProvider>
      <div className="App">
        <Helmet>
          {/* AEM Universal Editor :: CORE Library
              Loads the LATEST Universal Editor library
          */}
          <script
            src="https://universal-editor-service.experiencecloud.live/corslib/LATEST"
            async
          />
          {/* AEM Universal Editor :: Connection metadata 
              Connects to local AEM instance
          */}
          <meta
            name="urn:adobe:aue:system:aemconnection"
            content={`aem:https://localhost:8443`}
          />
          {/* AEM Universal Editor :: Configuration for Service
              Using locally running Universal Editor service
          */}
          <meta
            name="urn:adobe:aue:config:service"
            content={`https://localhost:8001`}
          />
        </Helmet>
        ...
    </HelmetProvider>
);
}
export default App;
```

## 检测React组件

编辑WKND Teams React应用程序的内容，例如 _团队标题和团队描述_&#x200B;中，您需要检测React组件。 检测是指添加相关数据属性(`data-aue-*`)添加到要使其可使用通用编辑器编辑的HTML元素中。 有关数据属性的更多信息，请参阅 [属性和类型](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/attributes-types).

### 定义可编辑元素

让我们从定义要使用通用编辑器编辑的元素开始。 在WKND团队React应用程序中，团队标题和描述存储在AEM的团队内容片段中，因此是最佳编辑候选者。

让我们来装备它 `Teams` React组件使团队标题和描述可编辑。

1. 打开 `src/components/Teams.js` WKND Teams React应用程序的文件。
1. 添加 `data-aue-prop`， `data-aue-type` 和 `data-aue-label` “团队标题”和“描述”元素的属性。

   ```javascript
   ...
   function Teams() {
       const { teams, error } = useAllTeams();
       ...
   
       return (
           <div className="team">
               // AEM Universal Editor :: Instrumentation using data-aue-* attributes
               <h2 className="team__title" data-aue-prop="title" data-aue-type="text" data-aue-label="title">{title}</h2>
               <p className="team__description" data-aue-prop="description" data-aue-type="richtext" data-aue-label="description">{description.plaintext}</p>
               ...
           </div>
       );
   }
   
   export default Teams;
   ```

1. 在加载WKND团队React应用程序的浏览器中刷新通用编辑器页面。 您现在可以看到团队标题和描述元素是可编辑的。

   ![通用编辑器 — WKND团队标题和描述可编辑](./assets/universal-editor-wknd-teams-title-desc-editable.png)

1. 如果尝试使用内联编辑或属性边栏编辑团队标题或描述，它会显示加载进度环但不允许您编辑内容。 因为通用编辑器不知道用于加载和保存内容的AEM资源详细信息。

   ![通用编辑器 — WKND团队标题和描述加载](./assets/universal-editor-wknd-teams-title-desc-editable-loading.png)

总之，上述更改将团队标题和描述元素标记为可在通用编辑器中编辑。 但是， **您无法编辑（通过内联或属性边栏）并保存更改**，为此，您需要使用添加AEM资源详细信息 `data-aue-resource` 属性。 让我们在下一个步骤中执行这项操作。

### 定义AEM资源详细信息

要将编辑的内容保存回AEM并加载属性边栏中的内容，您需要向通用编辑器提供AEM资源详细信息。

在本例中，AEM资源是团队内容片段路径，因此让我们将资源详细信息添加到 `Teams` 顶级的React组件 `<div>` 元素。

1. 更新 `src/components/Teams.js` 文件以添加 `data-aue-resource`， `data-aue-type` 和 `data-aue-label` 属性到顶层 `<div>` 元素。

   ```javascript
   ...
   function Teams() {
       const { teams, error } = useAllTeams();
       ...
   
       // Render single Team
       function Team({ _path, title, shortName, description, teamMembers }) {
           // Must have title, shortName and at least 1 team member
           if (!_path || !title || !shortName || !teamMembers) {
               return null;
           }
   
         return (
           // AEM Universal Editor :: Instrumentation using data-aue-* attributes
           <div className="team" data-aue-resource={`urn:aemconnection:${_path}/jcr:content/data/master`} data-aue-type="reference" data-aue-label={title}>
           ...
           </div>
       );
       }
   }
   export default Teams;
   ```

   的值 `data-aue-resource` attribute是团队内容片段的AEM资源路径。 此 `urn:aemconnection:` prefix使用连接元数据中定义的内容源的短名称。

1. 在加载WKND团队React应用程序的浏览器中刷新通用编辑器页面。 现在，您可以看到顶级“团队”元素是可编辑的，但属性边栏仍未加载内容。 在浏览器的“网络”选项卡中，您可以看到 `details` 用于加载内容的请求。 它尝试使用IMS令牌进行身份验证，但本地AEM SDK不支持IMS身份验证。

   ![通用编辑器 — WKND团队可编辑](./assets/universal-editor-wknd-teams-team-editable.png)

1. 要修复401未授权错误，您需要使用向通用编辑器提供本地AEM SDK身份验证详细信息 **身份验证标头** 选项。 作为本地AEM SDK，将该值设置为 `Basic YWRtaW46YWRtaW4=` 对象 `admin:admin` 凭据。

   ![通用编辑器 — 添加身份验证标头](./assets/universal-editor-wknd-teams-team-editable-auth.png)

1. 在加载WKND团队React应用程序的浏览器中刷新通用编辑器页面。 现在，您可以看到属性边栏正在加载内容，并且可以内联或使用属性边栏编辑团队标题和描述。

   ![通用编辑器 — WKND团队可编辑](./assets/universal-editor-wknd-teams-team-editable-props.png)

#### 在幕后工作

属性边栏使用本地Universal Editor服务从AEM资源加载内容。 使用浏览器的“网络”选项卡，您可以看到对本地Universal Editor服务的POST请求(`https://localhost:8001/details`)以加载内容。

使用内联编辑或属性边栏编辑内容时，更改会使用本地Universal Editor服务保存回AEM资源。 使用浏览器的“网络”选项卡，您可以看到对本地Universal Editor服务的POST请求(`https://localhost:8001/update` 或 `https://localhost:8001/patch`)以保存内容。

![通用编辑器 — WKND团队可编辑](./assets/universal-editor-under-the-hood-request.png)

请求有效负载JSON对象包含内容服务器(`connections`)，资源路径(`target`)，以及更新的内容(`patch`)。

![通用编辑器 — WKND团队可编辑](./assets/universal-editor-under-the-hood-payload.png)

### 展开可编辑的内容

让我们展开可编辑的内容并将检测应用到 **团队成员** 以便您可以使用属性边栏编辑团队成员。

如上所述，让我们添加相关的 `data-aue-*` 中的团队成员的属性 `Teams` React组件。

1. 更新 `src/components/Teams.js` 文件以将数据属性添加到 `<li key={index} className="team__member">` 元素。

   ```javascript
   ...
   function Teams() {
       const { teams, error } = useAllTeams();
       ...
   
       <div>
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
               {/* Render the referenced Person models associated with the team */}
               {teamMembers.map((teamMember, index) => {
                   return (
                       // AEM Universal Editor :: Instrumentation using data-aue-* attributes
                       <li key={index} className="team__member" data-aue-resource={`urn:aemconnection:${teamMember?._path}/jcr:content/data/master`} data-aue-type="component" data-aue-label={teamMember.fullName}>
                       <Link to={`/person/${teamMember.fullName}`}>
                           {teamMember.fullName}
                       </Link>
                       </li>
                   );
               })}
           </ul>
       </div>
       ...
   }
   export default Teams;
   ```

   的值 `data-aue-type` 属性为 `component` 因为团队成员存储为 `Person` AEM中的内容片段，并帮助指示内容的移动/可删除部分。

1. 在加载WKND团队React应用程序的浏览器中刷新通用编辑器页面。 您现在可以看到可以使用属性边栏编辑团队成员。

   ![通用编辑器 — WKND团队成员可编辑](./assets/universal-editor-wknd-teams-team-members-editable.png)

#### 在幕后工作

如上所述，内容检索和保存由本地Universal Editor服务完成。 此 `/details`， `/update` 或 `/patch` 向本地Universal Editor服务发出加载和保存内容的请求。

### 定义添加和删除内容

到目前为止，您已使现有内容可编辑，但如果要添加新内容，该怎么做？ 让我们添加使用通用编辑器向WKND团队添加或删除团队成员的功能。 因此，内容作者无需转至AEM来添加或删除团队成员。

但是，快速回顾一下，WKND团队成员存储为 `Person` AEM中的内容片段，并使用与团队内容片段关联 `teamMembers` 属性。 要查看AEM中的模型定义，请访问 [my-project](http://localhost:4502/libs/dam/cfm/models/console/content/models.html/conf/my-project).

1. 首先，创建元件定义文件 `/public/static/component-definition.json`. 此文件包含组件的定义 `Person` 内容片段。 此 `aem/cf` 插件允许基于提供要应用的默认值的模型和模板插入内容片段。

   ```json
   {
       "groups": [
           {
           "title": "Content Fragments",
           "id": "content-fragments",
           "components": [
               {
               "title": "Person",
               "id": "person",
               "plugins": {
                   "aem": {
                       "cf": {
                           "name": "person",
                           "cfModel": "/conf/my-project/settings/dam/cfm/models/person",
                           "cfFolder": "/content/dam/my-project/en",
                           "title": "person",
                           "template": {
                               "fullName": "New Person",
                               "biographyText": "This is biography of new person"
                               }
                           }
                       }
                   }
               }
           ]
           }
       ]
   }
   ```

1. 接下来，请参阅中的上述组件定义文件 `index.html` WKND Team React应用程序的。 更新 `public/index.html` 文件 `<head>` 部分，以包括元件定义文件。

   ```html
   ...
   <script
       type="application/vnd.adobe.aue.component+json"
       src="/static/component-definition.json"
   ></script>
   <title>WKND App - Basic GraphQL Tutorial</title>
   </head>
   ...
   ```

1. 最后，更新 `src/components/Teams.js` 文件以添加数据属性。 此 **成员** 区域作为团队成员的容器，让我们添加 `data-aue-prop`， `data-aue-type`、和 `data-aue-label` 属性 `<div>` 元素。

   ```javascript
   ...
   function Teams() {
       const { teams, error } = useAllTeams();
       ...
   
       {/* AEM Universal Editor :: Team Members as container */}
       <div data-aue-prop="teamMembers" data-aue-type="container" data-aue-label="members">
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
           {/* Render the referenced Person models associated with the team */}
           {teamMembers.map((teamMember, index) => {
               return (
               // AEM Universal Editor :: Instrumentation using data-aue-* attributes
               <li key={index} className="team__member" data-aue-resource={`urn:aemconnection:${teamMember?._path}/jcr:content/data/master`} data-aue-type="component" data-aue-label={teamMember.fullName}>
                   <Link to={`/person/${teamMember.fullName}`}>
                   {teamMember.fullName}
                   </Link>
               </li>
               );
           })}
           </ul>
       </div>
       ...
   }
   export default Teams;
   ```

1. 在加载WKND团队React应用程序的浏览器中刷新通用编辑器页面。 您现在可以看到 **成员** 部分用作容器。 您可以使用属性边栏和 **+** 图标。

   ![通用编辑器 — WKND团队成员插入](./assets/universal-editor-wknd-teams-add-team-members.png)

1. 要删除团队成员，请选择该团队成员，然后单击 **删除** 图标。

   ![通用编辑器 — WKND团队成员删除](./assets/universal-editor-wknd-teams-delete-team-members.png)

#### 在幕后工作

内容添加和删除操作由本地Universal Editor服务完成。 的POST请求 `/add` 或 `/remove` 使用详细的负载向本地Universal Editor服务发出，以将内容添加到AEM或从中删除。

## 解决方案文件

要验证实施更改或无法使WKND团队React应用程序与通用编辑器配合使用，请参阅 [basic-tutorial-instructed-for-UE](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial-instrumented-for-UE) 解决方案分支。

逐个文件与正在处理的项目的比较 **基础教程** 分支可用 [此处](https://github.com/adobe/aem-guides-wknd-graphql/compare/solution/basic-tutorial...solution/basic-tutorial-instrumented-for-UE?expand=1).

## 恭喜

您已成功检测WKND Teams React应用程序是否使用通用编辑器添加、编辑和删除内容。 您已了解如何包含核心库、添加连接和本地Universal Editor服务元数据，以及使用各种数据(`data-aue-*`)属性。
