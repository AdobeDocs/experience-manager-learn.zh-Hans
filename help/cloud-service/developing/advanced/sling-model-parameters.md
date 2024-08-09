---
title: 从HTL参数化Sling模型
description: 了解如何将参数从HTL传递到AEM中的Sling模型。
version: Cloud Service
topic: Development
feature: Sling Model
role: Developer
jira: KT-15923
level: Intermediate, Experienced
source-git-commit: f45d04b489771b9d3613cb4ce5e7b94d531ac783
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---


# 从HTL参数化Sling模型

Adobe Experience Manager (AEM)为构建动态且适应性强的Web应用程序提供了一个强大的框架。 其强大的功能之一是能够参数化Sling模型，增强其灵活性和可重用性。 本教程将指导您创建参数化Sling模型并在HTL(HTML模板语言)中使用它来呈现动态内容。

## HTL脚本

在此示例中，HTL脚本向`ParameterizedModel` Sling模型发送了两个参数。 模型在其`getValue()`方法中操作这些参数，并返回显示结果。

此示例传递两个String参数，但您可以将任何类型的值或对象传递到Sling模型，只要[Sling模型字段类型（用`@RequestAttribute`](#sling-model-implementation)进行注释）与从HTL传递的对象类型或值匹配。

```html
<sly data-sly-use.myModel="${'com.example.core.models.ParameterizedModel' @ myParameterOne='Hello', myParameterTwo='World'}"
     data-sly-test.isReady="${myModel.isReady()}">

    <p>
        ${myModel.value}
    </p>

</sly>

<sly data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
     data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!isReady}">
</sly>
```

- **参数化：** `data-sly-use`语句使用`myParameterOne`和`myParameterTwo`创建了`ParameterizedModel`的实例。
- **条件渲染：** `data-sly-test`在显示内容之前检查模型是否已就绪。
- **占位符调用：** `placeholderTemplate`处理模型未就绪的情况。

## Sling模型实施

以下是实施Sling模型的方式：

```java
package com.example.core.models.impl;

import org.apache.commons.lang3.StringUtils;
import org.apache.sling.api.SlingHttpServletRequest;
import org.osgi.service.component.annotations.Model;
import org.osgi.service.component.annotations.RequestAttribute;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Model(
    adaptables = { SlingHttpServletRequest.class },
    adapters = { ParameterizedModel.class }
)
public class ParameterizedModelImpl implements ParameterizedModel {
    private static final Logger log = LoggerFactory.getLogger(ParameterizedModelImpl.class);

    // Use the @RequestAttribute annotation to inject the parameter set in the HTL.
    // Note that the Sling Model field can be any type, but must match the type of object or value passed from HTL.
    @RequestAttribute
    private String myParameterOne;

    // If the HTL parameter name is different from the Sling Model field name, use the name attribute to specify the HTL parameter name
    @RequestAttribute(name = "myParameterTwo")
    private String mySecondParameter;

    // Do some work with the parameter values
    @Override
    public String getValue() {
        return myParameterOne + " " + mySecondParameter + ", from the parameterized Sling Model!";
    }

    @Override
    public boolean isReady() {
        return StringUtils.isNotBlank(myParameterOne) && StringUtils.isNotBlank(mySecondParameter);
    }
}
```

- **模型注释：** `@Model`注释将此类指定为Sling模型，可从`SlingHttpServletRequest`改写并实现`ParameterizedModel`接口。
- **请求属性：** `@RequestAttribute`注释将HTL参数注入模型中。
- **方法：** `getValue()`连接参数，并且`isReady()`检查参数是否不为空。

`ParameterizedModel`接口定义如下：

```java
package com.example.core.models;

import org.osgi.annotation.versioning.ConsumerType;

@ConsumerType
public interface ParameterizedModel {
    /**
     * Get an example string value from the model. This value is the concatenation of the two parameters.
     * 
     * @return the value of the model
     */
    String getValue();

    /**
     * Check if the model is ready to be used.
     *
     * @return {@code true} if the model is ready, {@code false} otherwise
     */
    boolean isReady();
}
```

## 示例输出

使用参数`"Hello"`和`"World"`，HTL脚本生成以下输出：

```html
<p>
    Hello World, from the parameterized Sling Model!
</p>
```

这显示了如何基于通过HTL提供的输入参数来影响AEM中的参数化Sling模型。

