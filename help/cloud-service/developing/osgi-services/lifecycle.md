---
title: OSGi元件生命週期
description: 瞭解OSGi元件生命週期，包括如何將OSGi服務繫結至啟用、修改和停用生命週期事件。
role: Developer
level: Beginner
topic: Development
feature: OSGI
kt: 8228
thumbnail: 335475.jpeg
exl-id: 5a65dbcd-649b-464c-9c78-d31c2b6c49c3
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 3%

---

# OSGi元件生命週期

瞭解OSGi元件生命週期，包括如何將OSGi服務繫結至：

+ 激活
+ 修改时间
+ 和停用

...生命週期事件。

>[!VIDEO](https://video.tv.adobe.com/v/335475?quality=12&learn=on)

## 资源

+ [@Activate JavaDocs](https://javadoc.io/static/com.adobe.aem/aem-sdk-api/2021.7.5658.20210723T140305Z-210600/org/osgi/service/component/annotations/Activate.html)
+ [@Modified JavaDocs](https://javadoc.io/static/com.adobe.aem/aem-sdk-api/2021.7.5658.20210723T140305Z-210600/org/osgi/service/component/annotations/Modified.html)
+ [@Deactivate JavaDocs](https://javadoc.io/static/com.adobe.aem/aem-sdk-api/2021.7.5658.20210723T140305Z-210600/org/osgi/service/component/annotations/Deactivate.html)

## 程式碼

### ActivitiesImpl.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/adventures/impl/ActivitiesImpl.java`

```java
package com.adobe.aem.wknd.examples.core.adventures.impl;

import java.util.Random;

import com.adobe.aem.wknd.examples.core.adventures.Activities;

import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Deactivate;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(
    service = { Activities.class }
)
public class ActivitiesImpl implements Activities {
    private static final Logger log = LoggerFactory.getLogger(ActivitiesImpl.class);

    private String[] activities;

    private final Random random = new Random();

    /**
     * @return the name of a random WKND adventure activity
     */
    public String getRandomActivity() {
        int randomIndex = random.nextInt(activities.length);
        return activities[randomIndex];
    }    

    @Activate
    protected void activate() {
        this.activities = new String[] { 
            "Running", "Cycling",  "Skateboarding"
        };

        log.info("Activated ActivitiesImpl with activities [ {} ]", String.join(", ", this.activities));
    }

    @Deactivate
    protected void deactivate() {
        log.info("ActivitiesImpl has been deactivated!");
    }
}
```
