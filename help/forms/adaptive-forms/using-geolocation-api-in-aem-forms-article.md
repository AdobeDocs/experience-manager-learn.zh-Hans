---
title: 在自适应Forms中使用Geolocation API
seo-title: 在自适应Forms中使用Geolocation API
description: 使用geolocation api的
seo-description: 使用geolocation api的
uuid: 5a461659-6873-4ea1-9f37-8296e5a9d895
feature: adaptive-forms,
topics: integrations
audience: developer
doc-type: article
activity: develop
version: 6.3,6.4,6.5
discoiquuid: 3400251b-aee0-4d69-994b-e1643fabc868
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '429'
ht-degree: 0%

---


# 在自适应Forms中使用地理位置API{#using-geolocation-api-s-in-adaptive-forms}

请访问[AEM Forms示例](https://forms.enablementadobe.com/content/samples/samples.html?query=0)页面，获取此功能的实时演示的链接。

在本文中，我们将了解如何使用Google的Geolocation API填充自适应表单的字段。 当您要在表单上填充当前地址字段时，通常使用此用例。

按照以下步骤在Adaptive Forms中使用Geolocation API。

1. [从Google](https://developers.google.com/maps/documentation/javascript/get-api-key) 获取API密钥以使用Google Maps平台。您可以获得有效期为1年的试用密钥。

1. 自适应表单片段已创建，其中包含用于保存当前地址的字段

1. 在自适应表单的图像对象的单击事件中调用了Geolocation API

1. 已分析API调用返回的JSON数据，并相应地设置自适应表单字段值。

```javascript
navigator.geolocation.getCurrentPosition(showPosition);
function showPosition(position) 
{
console.log(" I am inside the showPosition in fragment");
console.log("Latitude: " + position.coords.latitude + "Longitude " + position.coords.longitude);
var url = "https://maps.googleapis.com/maps/api/geocode/json?latlng="+position.coords.latitude+","+position.coords.longitude+"&key=<your_api_key>";
  console.log(url);
  
  $.getJSON(url,function (data, textStatus){
    
    var location=data.results[0].formatted_address;
    console.log(location);
    
    for(i=0;i<data.results[0].address_components.length;i++)
        {
          if(data.results[0].address_components[i].types[0] == "street_number")
            {
              streetNumber.value = data.results[0].address_components[i].long_name;
            }
          if(data.results[0].address_components[i].types[0] == "route")
            {
              streetName.value = data.results[0].address_components[i].long_name;
            }
            if(data.results[0].address_components[i].types[0] == "postal_code")
            {
              
              zipCode.value = data.results[0].address_components[i].long_name;
            }
            if(data.results[0].address_components[i].types[0] == "locality")
            {
              
              city.value = data.results[0].address_components[i].long_name;
            }
          if(data.results[0].address_components[i].types[0] == "administrative_area_level_1")
            {
              
              state.value = data.results[0].address_components[i].long_name;
            }
        }
    
  });
}
```

![填充有地理作用api的字段](assets/capture-4.gif)

在第1行中，我们使用HTML地理位置API来获取当前位置。 获得当前位置后，我们将当前位置传递给showPosition函数。

在showPosition函数中，我们使用Google API获取给定经度和纬度的地址详细信息。

随后将分析API返回的JSON以设置自适应表单字段。

>[!NOTE]
>
>为了进行测试，您可以将HTTP协议与URL中的localhost一起使用。
>
>对于生产服务器，您需要为AEM服务器启用SSL才能获得此功能。
>
>与本文相关的示例已使用美国地址进行测试。 如果要在其他地理位置使用此功能，您可能必须调整JSON分析。

要将此功能安装到您的服务器上，请执行以下步骤

* 安装和开始AEM Forms服务器。

>!![NOTE] 此功能已在AEM Forms6.3及更高版本上进行了测试
* [获取Google API密钥](https://developers.google.com/maps/documentation/javascript/get-api-key)。
* [将与此文章相关的资产导入AEM。](assets/geolocationapi.zip)
* [在编辑模式下打开自适应表单片段。](http://localhost:4502/editor.html/content/forms/af/currentaddressfragment.html)
* 打开图像选择组件的规则编辑器。
* 将&lt;your_api_key>替换为Google API Key。
* 保存更改。
* [预览表单](http://localhost:4502/content/dam/formsanddocuments/currentaddressfragment/jcr:content?wcmmode=disabled)。
* 单击“地理位置”图标。
* 您的表单应填充当前位置。
