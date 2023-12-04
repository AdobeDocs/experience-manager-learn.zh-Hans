---
title: OSGi服务开发基础
description: 了解开发OSGi服务的基础知识
role: Developer
level: Beginner
topic: Development
feature: OSGI
jira: KT-8227
thumbnail: 335476.jpeg
last-substantial-update: 2022-09-16T00:00:00Z
exl-id: a3a9bf59-e9a2-4322-ac93-9c12c70b9a75
duration: 516
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '57'
ht-degree: 1%

---

# OSGi服务

了解OSGi服务开发的基础知识，包括：

+ 如何将Java POJO转换为OSGi服务
+ 如何将OSGi服务绑定到Java接口

>[!VIDEO](https://video.tv.adobe.com/v/335476?quality=12&learn=on)

## 资源

+ [@Component JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/osgi/service/component/annotations/Component.html)
+ [@ProviderType JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/osgi/annotation/versioning/ProviderType.html)
+ [@Version JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/osgi/annotation/versioning/Version.html)

## 代码

### Activities.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/adventures/Activities.java`

```java
package com.adobe.aem.wknd.examples.core.adventures;

import org.osgi.annotation.versioning.ProviderType;

@ProviderType
public interface Activities {    
    String getRandomActivity();
}
```

### ActivitiesImpl.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/adventures/impl/ActivitiesImpl.java`

```java
package com.adobe.aem.wknd.examples.core.adventures.impl;

import java.util.Random;

import com.adobe.aem.wknd.examples.core.adventures.Activities;

import org.osgi.service.component.annotations.Component;

@Component(
    service = { Activities.class }
)
public class ActivitiesImpl implements Activities {

    private static final String[] ACTIVITIES = new String[] { 
        "Camping", "Skiing",  "Skateboarding"
    };

    //private final int randomIndex = new Random().nextInt(ACTIVITIES.length);
    private final Random random = new Random();

    /**
     * @return the name of a random WKND adventure activity
     */
    public String getRandomActivity() {
        int randomIndex = random.nextInt(ACTIVITIES.length);
        return ACTIVITIES[randomIndex];
    }    
}
```

### package-info.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/adventures/package-info.java`

```java
@Version("1.0")
package com.adobe.aem.wknd.examples.core.adventures;

import org.osgi.annotation.versioning.Version;
```
