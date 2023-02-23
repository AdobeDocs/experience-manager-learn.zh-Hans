---
title: 将本地化内容与AEM Headless结合使用
description: 了解如何使用GraphQL查询AEM的本地化内容。
version: Cloud Service
feature: GraphQL API
topic: Headless
role: Developer
level: Intermediate
kt: 10254
thumbnail: KT-10254.jpeg
source-git-commit: ae49fb45db6f075a34ae67475f2fcc5658cb0413
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 3%

---


# 使用AEM Headless的本地化内容

AEM提供 [翻译集成框架](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/reusing-content/translation/integration-framework.html) 对于无标题内容，允许轻松翻译内容片段和支持资产以跨区域设置使用。 这是用于翻译其他AEM内容(如页面、体验片段、资产和Forms)的相同框架。 一次 [无标题内容已翻译](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/overview.html?lang=zh-Hans)，且已发布，则可供无头应用程序使用。

## 资产文件夹结构{#assets-folder-structure}

确保AEM中的本地化内容片段在 [推荐的本地化结构](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/getting-started.html#recommended-structure).

![本地化的AEM资产文件夹](./assets/localized-content/asset-folders.jpg)

区域设置文件夹必须是同级文件夹，并且文件夹名称（而不是标题）必须有效 [ISO 639-1代码](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) 表示文件夹中包含的内容的区域设置。

区域设置代码也是用于筛选由GraphQL查询返回的内容片段的值。

| 区域设置代码 | AEM路径 | 内容区域设置 |
|--------------------------------|----------|----------|
| de | /content/dam/.../**de**/.. | 德语内容 |
| en | /content/dam/.../**en**/.. | 英语内容 |
| es | /content/dam/.../**es**/.. | 西班牙语内容 |

## GraphQL持久查询

AEM提供 `_locale` GraphQL过滤器，可按区域设置代码自动过滤内容。 例如，查询 [WKND站点项目](https://github.com/adobe/aem-guides-wknd) 可以使用新的保留查询完成 `wknd-shared/adventures-by-locale` 定义为：

```graphql
query($locale: String!) {
  adventureList(_locale: $locale) {
    items {      
      _path
      title
    }
  }
}
```

的 `$locale` 变量 `_locale` 筛选器需要区域设置代码(例如 `en`, `en_us`或 `de`) [AEM资产文件夹基础本地化约定](#assets-folder-structure).

## React示例

让我们创建一个简单的React应用程序，该应用程序可根据使用的区域设置选择器，控制要从AEM中查询的Adventure内容 `_locale` 过滤器。

When __英语__ 在区域设置选择器中进行选择，然后在 `/content/dam/wknd/en` 返回时 __西班牙语__ ，然后选择下的西班牙文内容片段 `/content/dam/wknd/es`、等等。

![本地化的React示例应用程序](./assets/localized-content/react-example.png)

### 创建 `LocaleContext`{#locale-context}

首先，创建 [React context](https://reactjs.org/docs/context.html) 以允许在React应用程序的组件中使用区域设置。

```javascript
// src/LocaleContext.js

import React from 'react'

const DEFAULT_LOCALE = 'en';

const LocaleContext = React.createContext({
    locale: DEFAULT_LOCALE, 
    setLocale: () => {}
});

export default LocaleContext;
```

### 创建 `LocaleSwitcher` React组件{#locale-switcher}

接下来，创建一个区域设置切换器React组件，该组件将 [LocaleContext的](#locale-context) 值。

此区域设置值用于驱动GraphQL查询，以确保它们只返回与选定区域设置匹配的内容。

```javascript
// src/LocaleSwitcher.js

import { useContext } from "react";
import LocaleContext from "./LocaleContext";

export default function LocaleSwitcher() {
  const { locale, setLocale } = useContext(LocaleContext);

  return (
    <select value={locale}
            onChange={e => setLocale(e.target.value)}>
      <option value="de">Deutsch</option>
      <option value="en">English</option>
      <option value="es">Español</option>
    </select>
  );
}
```

### 使用查询内容 `_locale` 过滤器{#adventures}

冒险组件按区域设置查询AEM的所有冒险，并列出其标题。 这是通过使用 `_locale` 过滤器。

此方法可以扩展到您应用程序中的其他查询，从而确保所有查询仅包含用户区域设置选择指定的内容。

对AEM的查询在自定义React挂接中执行 [getAdventuresByLocale，有关查询AEM GraphQL文档的更多详细信息](./aem-headless-sdk.md).

```javascript
// src/Adventures.js

import { useContext } from "react"
import { useAdventuresByLocale } from './api/persistedQueries'
import LocaleContext from './LocaleContext'

export default function Adventures() {
    const { locale } = useContext(LocaleContext);

    // Get data from AEM using GraphQL persisted query as defined above 
    // The details of defining a React useEffect hook are explored in How to > AEM Headless SDK
    let { data, error } = useAdventuresByLocale(locale);

    return (
        <ul>
            {data?.adventureList?.items?.map((adventure, index) => { 
                return <li key={index}>{adventure.title}</li>
            })}
        </ul>
    )
}
```

### 定义 `App.js`{#app-js}

最后，将React应用程序封装在中，并将其与 `LanguageContext.Provider` 和设置区域设置值。 这允许其他React组件， [区域设置切换器](#locale-switcher)和 [冒险](#adventures) 共享区域设置选择状态。

```javascript
// src/App.js

import { useState, useContext } from "react";
import LocaleContext from "./LocaleContext";
import LocaleSwitcher from "./LocaleSwitcher";
import Adventures from "./Adventures";

export default function App() {
  const [locale, setLocale] = useState(useContext(LocaleContext).locale);

  return (
    <LocaleContext.Provider value={{locale, setLocale}}>
      <LocaleSwitcher />
      <Adventures />
    </LocaleContext.Provider>
  );
}
```
