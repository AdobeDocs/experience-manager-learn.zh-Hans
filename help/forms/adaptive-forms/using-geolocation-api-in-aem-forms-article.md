---
title: 在自适应Forms中使用地理位置API
description: 使用地理位置API填充表单上的地址字段
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 50db6155-ee83-4ddb-9e3a-56e8709222db
last-substantial-update: 2020-03-20T00:00:00Z
duration: 88
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 0%

---

# 在自适应Forms中使用地理位置API{#using-geolocation-api-s-in-adaptive-forms}

在本篇文章中，我们将了解如何使用Google的地理位置API填充自适应表单的字段。 当您要填充表单上的当前地址字段时，通常使用此用例。

在自适应Forms中使用地理位置API时，需要执行以下步骤。

1. [获取API密钥](https://developers.google.com/maps/documentation/javascript/get-api-key) 从Google使用Google地图平台。 您可以获得有效期为1年的试用密钥。

1. 创建自适应表单片段时添加了用于保存当前地址的字段

1. 在自适应表单的图像对象的单击事件上调用了地理位置API

1. 对API调用返回的JSON数据进行解析，并相应地设置自适应表单字段值。

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

![使用地理位置api填充的字段](assets/capture-4.gif)

在第1行，我们使用HTML地理位置API来获取当前位置。 获得当前位置后，我们将当前位置传递给showPosition函数。

在showPosition函数中，我们使用Google API获取给定纬度和经度的地址详细信息。

然后，将解析API返回的JSON以设置自适应表单字段。

>[!NOTE]
>
>出于测试目的，您可以对URL中的localhost使用HTTP协议。
>
>对于生产服务器，您需要为AEM服务器启用SSL才能获取此功能。
>
>已使用美国地址对与本文相关的示例进行测试。 如果要在其他地理位置使用此功能，则可能必须调整JSON解析。

要将此功能添加到您的服务器上，请执行以下步骤

* 安装并启动AEM Forms服务器。
> 此功能已在AEM Forms 6.3及更高版本上测试
* [获取Google API密钥](https://developers.google.com/maps/documentation/javascript/get-api-key).
* [将与本文相关的资源导入AEM。](assets/geolocationapi.zip)
* [在编辑模式下打开自适应表单片段。](http://localhost:4502/editor.html/content/forms/af/currentaddressfragment.html)
* 打开图像选择组件的规则编辑器。
* 替换 &lt;your_api_key> Google API密钥。
* 保存更改。
* [预览表单](http://localhost:4502/content/dam/formsanddocuments/currentaddressfragment/jcr:content?wcmmode=disabled).
* 单击“地理位置”图标。
* 应使用当前位置填充您的表单。
