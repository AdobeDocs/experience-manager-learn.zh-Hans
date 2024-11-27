---
title: 为组件创建Sling模型
description: 为组件创建Sling模型
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Formsas a Cloud Service" before-title="false"
jira: KT-16517
source-git-commit: f9a1fb40aabb6fdc1157e1f2576f9c0d9cf1b099
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 0%

---


# 为组件创建Sling模型

AEM中的Sling模型是一个基于Java的框架，用于简化组件后端逻辑的开发。 它允许开发人员使用注释将数据从AEM资源（JCR节点）映射到Java对象，从而为组件处理动态数据提供了一种干净高效的方法。
这个类CountriesDropDownImpl是AEM (Adobe Experience Manager)项目中CountriesDropDown接口的一个实现。 它支持一个下拉组件，用户可以在其中根据所选大陆选择国家/地区。 下拉数据是从存储在AEM DAM (Digital Asset Manager)中的JSON文件动态加载的。

**类中的字段**

* **multiSelect**：指示下拉列表是否允许多项选择。
使用默认值false的@ValueMapValue从组件属性中插入。
* **请求**：表示当前HTTP请求。 对于访问上下文特定的信息很有用。
* **大陆**：为下拉列表存储选定的大陆（例如，“亚洲”、“欧洲”）。
从组件的属性对话框中插入，如果未提供任何内容，则默认值为“asia”。
* **resourceResolver**：用于访问和处理AEM存储库中的资源。
* **jsonData**： JSONObject，用于存储与选定大陆对应的JSON文件中的已解析数据。

类中的&#x200B;**方法**

* **getContinent()**返回continent字段值的简单方法。
记录返回的值以进行调试。
* **init()**使用@PostConstruct注释的生命周期方法，在构造类并插入依赖项后执行。根据大陆值动态构造JSON文件的路径。
使用resourceResolver从AEM DAM获取JSON文件。
将文件适应资产、读取其内容并将其解析为JSONObject。
记录在此过程中出现的任何错误或警告。
* **getEnums()**从解析后的JSON数据中检索所有键（国家/地区代码）。
按字母顺序对键进行排序，然后按数组形式返回它们。
记录要返回的国家/地区代码数。
* **getEnumNames()**从解析后的JSON数据中提取所有国家/地区名称。
按字母顺序对名称进行排序并将它们作为数组返回。
记录国家/地区总数和每个检索到的国家/地区名称。
* **isMultiSelect()**&#x200B;返回multiSelect字段的值以指示下拉列表是否允许多项选择。



```java
package com.aem.customcorecomponent.core.models.impl;
import com.adobe.cq.export.json.ComponentExporter;
import com.adobe.cq.export.json.ExporterConstants;
import com.adobe.cq.forms.core.components.internal.form.ReservedProperties;
import com.adobe.cq.forms.core.components.util.AbstractOptionsFieldImpl;
import com.aem.customcorecomponent.core.models.CountriesDropDown;
import com.day.cq.dam.api.Asset;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.apache.sling.models.annotations.Default;
import org.apache.sling.models.annotations.Exporter;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.injectorspecific.InjectionStrategy;
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
import org.json.JSONObject;
import javax.annotation.PostConstruct;
import javax.inject.Inject;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.nio.charset.StandardCharsets;
import java.util.*;
import java.util.stream.Collectors;

//@Model(adaptables = SlingHttpServletRequest.class,adapters = CountriesDropDown.class,defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)

@Model(
        adaptables = { SlingHttpServletRequest.class, Resource.class },
        adapters = { CountriesDropDown.class,
                ComponentExporter.class },
        resourceType = { "corecomponent/components/adaptiveForm/countries" })
@Exporter(name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, extensions = ExporterConstants.SLING_MODEL_EXTENSION)

public class CountriesDropDownImpl extends AbstractOptionsFieldImpl implements CountriesDropDown {
    @ValueMapValue(injectionStrategy = InjectionStrategy.OPTIONAL, name = ReservedProperties.PN_MULTISELECT)
    @Default(booleanValues = false)
    protected boolean multiSelect;

    private static final Logger logger = LoggerFactory.getLogger(CountriesDropDownImpl.class);
    @Inject
    SlingHttpServletRequest request;

    @Inject
    @Default(values = "asia")
    public String continent;
    @Inject
    private ResourceResolver resourceResolver;
    private JSONObject jsonData;
    public String getContinent()
    {
        logger.info("Returning continent");
        return continent;
    }
    @PostConstruct
    public void init() {

        String jsonPath = "/content/dam/corecomponent/" + getContinent() + ".json"; // Update path as needed
        logger.info("Initializing JSON data for continent: {}", getContinent());

        try {
            // Fetch the JSON resource
            Resource jsonResource = resourceResolver.getResource(jsonPath);
            if (jsonResource != null) {
                // Adapt the resource to an Asset
                Asset asset = jsonResource.adaptTo(Asset.class);
                if (asset != null) {
                    // Get the original rendition and parse it as JSON
                    try (InputStream inputStream = asset.getOriginal().adaptTo(InputStream.class);
                         BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream, StandardCharsets.UTF_8))) {

                        String jsonString = reader.lines().collect(Collectors.joining());
                        jsonData = new JSONObject(jsonString);
                        logger.info("Successfully loaded JSON data for path: {}", jsonPath);
                    }
                } else {
                    logger.warn("Failed to adapt resource to Asset at path: {}", jsonPath);
                }
            } else {
                logger.warn("Resource not found at path: {}", jsonPath);
            }
        } catch (Exception e) {
            logger.error("An error occurred while initializing JSON data for path: {}", jsonPath, e);
        }
    }

    @Override
    public Object[] getEnums() {
        Set<String> keySet = jsonData.keySet();


// Convert the set of keys to a sorted array
        String[] countryCodes = keySet.toArray(new String[0]);
        Arrays.sort(countryCodes);

        logger.debug("Returning sorted country codes: " + countryCodes.length);

        return countryCodes;


    }

    @Override
    public String[] getEnumNames() {
        String[] countries = new String[jsonData.keySet().size()];
        logger.info("Fetching countries - Total number: " + countries.length);

// Populate the array with country names
        int index = 0;
        for (String code : jsonData.keySet()) {
            String country = jsonData.getString(code);
            logger.debug("Retrieved country: " + country);
            countries[index++] = country;
        }

// Sort the array alphabetically
        Arrays.sort(countries);
        logger.debug("Returning " + countries.length + " sorted countries");

        return countries;

    }

    @Override
    public Boolean isMultiSelect() {
        return multiSelect;
    }
}
```

## 后续步骤

[生成、部署和测试](./build.md)
