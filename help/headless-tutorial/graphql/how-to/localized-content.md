---
title: 在AEM Headless中使用本地化的内容
description: 了解如何使用GraphQL查询AEM中的本地化内容。
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless
role: Developer
level: Intermediate
jira: KT-10254
thumbnail: KT-10254.jpeg
exl-id: 5e3d115b-f3a1-4edc-86ab-3e0713a36d54
duration: 130
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 0%

---

# AEM Headless的本地化内容

AEM为Headless内容提供了[翻译集成框架](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/reusing-content/translation/integration-framework.html)，允许轻松翻译内容片段和支持资源以供跨区域设置使用。 该框架与用于翻译其他AEM内容(如页面、体验片段、Assets和Forms)的框架相同。 在翻译[Headless内容](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/Headless/journeys/translation/overview.html?lang=zh-hans)并发布后，便可以供Headless应用程序使用。

## Assets文件夹结构{#assets-folder-structure}

确保AEM中的本地化内容片段遵循[建议的本地化结构](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/getting-started.html#recommended-structure)。

![本地化的AEM资源文件夹](./assets/localized-content/asset-folders.jpg)

区域设置文件夹必须是同级，并且文件夹名称（而不是标题）必须是表示文件夹中所包含内容区域设置的有效[ISO 639-1代码](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)。

区域设置代码也是用于筛选GraphQL查询返回的内容片段的值。

| 区域设置代码 | AEM路径 | 内容区域设置 |
|--------------------------------|----------|----------|
| de | /content/dam/.../**de**/... | 德语内容 |
| en | /content/dam/.../**en**/... | 英语内容 |
| es | /content/dam/.../**es**/... | 西班牙语内容 |

## GraphQL持久查询

AEM提供了一个`_locale` GraphQL筛选器，可按区域设置代码自动筛选内容。 例如，查询[WKND站点项目](https://github.com/adobe/aem-guides-wknd)中的所有英语冒险可以使用定义为：`wknd-shared/adventures-by-locale`

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

`_locale`筛选器中使用的`$locale`变量需要[AEM基于资源文件夹的本地化约定](#assets-folder-structure)中指定的区域设置代码（例如`en`、`en_us`或`de`）。

## React示例

让我们创建一个简单的React应用程序，该应用程序使用`_locale`过滤器根据区域设置选择器控制要从AEM查询哪些Adventure内容。

在区域设置选择器中选择&#x200B;__English__&#x200B;时，将返回`/content/dam/wknd/en`下的English Adventure内容片段；选择&#x200B;__Spanish__&#x200B;时，将返回`/content/dam/wknd/es`下的Spanish内容片段，依此类推。

![本地化React示例应用程序](./assets/localized-content/react-example.png)

### 创建`LocaleContext`{#locale-context}

首先，创建一个[React上下文](https://reactjs.org/docs/context.html)，以允许在React应用程序的组件中使用区域设置。

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

### 创建`LocaleSwitcher` React组件{#locale-switcher}

接下来，创建一个区域设置切换器React组件，该组件将[LocaleContext的](#locale-context)值设置为用户的选择。

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

### 使用`_locale`筛选器查询内容{#adventures}

冒险组件按区域设置查询AEM的所有冒险并列出其标题。 这是通过使用`_locale`过滤器将React上下文中存储的区域设置值传递给查询来实现的。

此方法可以扩展到应用程序中的其他查询，确保所有查询仅包含由用户的区域设置选择指定的内容。

对AEM的查询在自定义React挂接[getAdventuresByLocale中执行，有关更多详细信息，请参阅“查询AEM GraphQL”文档](./aem-headless-sdk.md)。

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

### 定义`App.js`{#app-js}

最后，通过使用`LanguageContext.Provider`封装React应用程序并设置区域设置值，将其全部连接在一起。 这允许其他React组件[LocaleSwitcher](#locale-switcher)和[Adventures](#adventures)共享区域设置选择状态。

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
