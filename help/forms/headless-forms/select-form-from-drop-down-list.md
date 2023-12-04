---
title: 从可用表单列表中选择表单
description: 使用listforms API填充下拉列表
feature: Adaptive Forms
version: 6.5
jira: KT-13346
topic: Development
role: User
level: Intermediate
exl-id: 49b6a172-8c96-4fc6-8d31-c2109f65faac
duration: 134
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 1%

---

# 从下拉列表中选择要填充的表单

下拉列表提供了一种简洁而有序的方式，向用户显示选项列表。 下拉列表中的项目将填充以下项的结果 [listforms API](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms)

![卡片视图](./assets/forms-drop-down.png)

## 下拉列表

以下代码用于使用listforms API调用的结果填充下拉列表。 根据用户选择，显示自适应表单以供用户填写和提交。 [材料UI组件](https://mui.com/) 已用于创建此界面

```javascript
import * as React from 'react';
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import Box from '@mui/material/Box';
import InputLabel from '@mui/material/InputLabel';
import MenuItem from '@mui/material/MenuItem';
import FormControl from '@mui/material/FormControl';
import Select, { SelectChangeEvent } from '@mui/material/Select';
import { AdaptiveForm } from "@aemforms/af-react-renderer";

import { useState,useEffect } from "react";
export default function SelectFormFromDropDownList()
 {
    const extendMappings =
    {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
    };

const[formID, setFormID] = useState('');
const[afForms,SetOptions] = useState([]);
const [selectedForm, setForm] = useState('');
const HandleChange = (event) =>
     {
        console.log("The form id is "+event.target.value) 
    
        setFormID(event.target.value)
        
     };
const getForm = async () =>
     {
        
        console.log("The formID in getForm"+ formID);
        const resp = await fetch(`/adobe/forms/af/${formID}`);
        let formJSON = await resp.json();
        console.log(formJSON.afModelDefinition);
        setForm(formJSON.afModelDefinition);
     }
const getAFForms =async()=>
     {
        const response = await fetch("/adobe/forms/af/listforms")
        //let myresp = await response.status;
        let myForms = await response.json();
        console.log("Got response"+myForms.items[0].title);
        console.log(myForms.items)
        
        //setFormID('test');
        SetOptions(myForms.items)

        
     }
     useEffect( ()=>{
        getAFForms()
        

    },[]);
    useEffect( ()=>{
        getForm()
        

    },[formID]);

  return (
    <Box sx={{ minWidth: 120 }}>
      <FormControl fullWidth>
        <InputLabel id="demo-simple-select-label">Please select the form</InputLabel>
        <Select
          labelId="demo-simple-select-label"
          id="demo-simple-select"
          value={formID}
          label="Please select a form"
          onChange={HandleChange}
          
        >
       {afForms.map((afForm,index) => (
    
        
          <MenuItem  key={index} value={afForm.id}>{afForm.title}</MenuItem>
        ))}
        
       
        </Select>
      </FormControl>
      <div><AdaptiveForm mappings={extendMappings} formJson={selectedForm}/></div>
    </Box>
    

  );
  

}
```

创建此用户界面时使用了以下两个API调用

* [列表表单](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms). 在呈现组件时，仅会发出一次获取表单的调用。 API调用的结果存储在afForms变量中。
在上面的代码中，我们使用map函数对afForms进行迭代，对于afForms数组中的每个项，将创建一个MenuItem组件并将其添加到Select组件中。

* 获取表单 — 对 [getForm](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/Get-Form-Definition)，其中id是用户在下拉列表中选择的自适应表单的id。 此GET调用的结果存储在selectedForm中。

```
const resp = await fetch(`/adobe/forms/af/${formID}`);
let formJSON = await resp.json();
console.log(formJSON.afModelDefinition);
setForm(formJSON.afModelDefinition);
```

* 显示所选表单。 以下代码用于显示所选的表单。 AdaptiveForm元素在aemforms/af-react-renderer npm包中提供，它需要映射和formJson作为其属性

```
<div><AdaptiveForm mappings={extendMappings} formJson={selectedForm}/></div>
```

## 后续步骤

[以卡片布局显示表单](./display-forms-card-view.md)
