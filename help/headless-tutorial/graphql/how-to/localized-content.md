---
title: 在AEM Headless中使用本地化内容
description: 了解如何使用GraphQL查询AEM的本地化内容。
version: Cloud Service
feature: GraphQL API
topic: Headless
role: Developer
level: Intermediate
jira: KT-10254
thumbnail: KT-10254.jpeg
exl-id: 5e3d115b-f3a1-4edc-86ab-3e0713a36d54
duration: 192
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 0%

---

# AEM Headless的本地化内容

AEM提供 [翻译集成框架](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/reusing-content/translation/integration-framework.html) 对于Headless内容，允许轻松翻译内容片段和支持资产，以便跨区域设置使用。 该框架与用于翻译其他AEM内容(如页面、体验片段、资源和Forms)的框架相同。 一次 [headless内容已翻译](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/overview.html?lang=zh-Hans)和发布后，它便可用于headless应用程序使用。

## 资产文件夹结构{#assets-folder-structure}

确保AEM中的本地化内容片段遵循 [推荐的本地化结构](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/getting-started.html#recommended-structure).

![本地化的AEM资源文件夹](./assets/localized-content/asset-folders.jpg)

区域设置文件夹必须是同级，并且文件夹名称而不是标题必须是有效的 [ISO 639-1代码](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) 表示文件夹中所包含内容的区域设置。

区域设置代码也是用于筛选GraphQL查询返回的内容片段的值。

| 区域设置代码 | AEM路径 | 内容区域设置 |
|--------------------------------|----------|----------|
| de | /content/dam/.../**de**/... | 德语内容 |
| en | /content/dam/.../**en**/... | 英语内容 |
| es | /content/dam/.../**es**/... | 西班牙语内容 |

## GraphQL持久查询

AEM提供 `_locale` 可按区域设置代码自动筛选内容的GraphQL筛选器。 例如，查询 [WKND站点项目](https://github.com/adobe/aem-guides-wknd) 可以使用新的持久查询完成 `wknd-shared/adventures-by-locale` 定义为：

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

此 `$locale` 中使用的变量 `_locale` 筛选器需要区域设置代码(例如 `en`， `en_us`，或 `de`)，如中所指定 [基于资产文件夹的AEM本地化惯例](#assets-folder-structure).

## React示例

让我们创建一个简单的React应用程序，该应用程序通过使用区域设置选择器控制要从AEM查询哪些Adventure内容 `_locale` 筛选。

时间 __英语__ 在区域设置选择器中选中，然后在 `/content/dam/wknd/en` 返回值，当 __西班牙语__ 选择，然后选择下的西班牙语内容片段 `/content/dam/wknd/es`，等等。

![本地化React示例应用程序](./assets/localized-content/react-example.png)

### 创建 `LocaleContext`{#locale-context}

首先，创建 [React上下文](https://reactjs.org/docs/context.html) 允许在React应用程序的组件中使用区域设置。

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

接下来，创建一个区域设置切换器React组件，该组件将 [LocaleContext](#locale-context) 用户的选择的值。

此区域设置值用于驱动GraphQL查询，确保它们仅返回与所选区域设置相匹配的内容。

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

### 使用查询内容 `_locale` 筛选{#adventures}

Adventures组件按区域设置查询AEM的所有冒险并列出其标题。 这是通过使用将存储在React上下文中的区域设置值传递到查询来实现的 `_locale` 筛选。

此方法可以扩展到应用程序中的其他查询，确保所有查询仅包含由用户的区域设置选择指定的内容。

在自定义React挂接中执行针对AEM的查询 [getAdventuresByLocale，有关查询AEM GraphQL文档的更多详细信息](./aem-headless-sdk.md).

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

最后，通过将React应用程序与封装在一起 `LanguageContext.Provider` 并设置区域设置值。 这允许其他React组件， [区域设置切换器](#locale-switcher)、和 [冒险](#adventures) 以共享区域设置选择状态。

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
