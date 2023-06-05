---
title: 单击卡以显示表单
description: 从卡片视图向下钻取表单
feature: Adaptive Forms
version: 6.5
kt: 13372
topic: Development
role: User
level: Intermediate
source-git-commit: 3bbf80d5c301953b3a34ef8256702ac7445c40da
workflow-type: tm+mt
source-wordcount: '93'
ht-degree: 0%

---

# 在卡片上显示表单，单击

以下代码用于在用户单击信息卡时显示表单。 使用useParams函数从url中提取要显示的表单路径。

```javascript
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import Panel from './components/Panel';
import { useState,useEffect } from "react";
import {Link, useParams} from 'react-router-dom';
import { AdaptiveForm } from "@aemforms/af-react-renderer";
export default function DisplayForm()
{
   const [selectedForm, setForm] = useState("");
   const params = useParams();
   const pathParam = params["*"];
   const extendMappings =
    {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
    };
    
    // Add the leading '/' back on 
   const path = '/' + pathParam;
   const getAFForm = async () =>
    {
           
        const resp = await fetch(`${path}/jcr:content/guideContainer.model.json`);
        let formJSON = await resp.json();
        console.log("The contact form json is "+formJSON);
        setForm(formJSON)
    }
    
    useEffect( ()=>{
        getAFForm()
        

    },[]);
    return(
       <div>
           <AdaptiveForm mappings={extendMappings} formJson={selectedForm}/>
        </div>
    )
}
```

在上述代码中，将从url中提取要渲染的表单的路径，并将其用于提取表单JSON的提取调用。 然后，获取的JSON将放入以下代码中

```javascript
        <div>
           <AdaptiveForm mappings={extendMappings} formJson={selectedForm}/>
        </div>
```
