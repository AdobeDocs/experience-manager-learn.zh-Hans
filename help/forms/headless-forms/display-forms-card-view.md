---
title: 在卡片视图中显示获取的表单
description: 使用listforms API显示表单
feature: Adaptive Forms
version: 6.5
jira: KT-13311
topic: Development
role: User
level: Intermediate
exl-id: c01ad68e-23c9-4564-8e3e-1924af34a493
duration: 134
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 0%

---

# 获取并以卡片格式显示表单

卡片视图格式是以卡片形式显示信息或数据的设计模式。 每个信息卡表示一段单独的内容或数据条目，通常由一个直观上不同的容器组成，其中排列了特定元素。
React中的可单击卡片是类似于卡片或图块的交互式组件，用户可以单击或点按。 当用户单击或点按可单击的卡片时，会触发指定的操作或行为，例如导航到其他页面、打开模式或更新UI。

在本文中，我们将使用 [listforms API](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms) 以提取表单并以卡片格式显示表单，然后在单击事件时打开自适应表单。

![卡片视图](./assets/card-view-forms.png)

## 信息卡模板

以下代码用于设计卡片模板。 卡片模板显示自适应表单的标题和描述以及Adobe徽标。 [材料UI组件](https://mui.com/) 已用于创建此布局。



```javascript
import Container from "@mui/material/Container";
import Form from './Form';
import PlainText from './plainText'
import TextField from './TextField'
import Button from './Button';
import { AdaptiveForm } from "@aemforms/af-react-renderer";

import { CardActionArea, Typography } from "@mui/material";
import { Box } from "@mui/system";
import { useState,useEffect } from "react";
import DisplayForm from "../DisplayForm";
import { Link } from "react-router-dom";
export default function FormCard({headlessForm}) {
const extendMappings =
    {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
    };
   
    return (
        
            <Grid item xs={3}>
                <Paper elevation={3}>
                    <img src="/content/dam/formsanddocuments/registrationform/jcr:content/renditions/cq5dam.thumbnail.48.48.png" className="img"/>
                    <Box padding={3}>
                        <Link style={{ textDecoration: 'none' }} to={`/displayForm${headlessForm.id}`}>
                            <Typography variant="subtititle2" component="h2">
                                {headlessForm.title}
                            </Typography>
                            <Typography variant="subtititle3" component="h4">
                                {headlessForm.description}
                            </Typography>
                        </Link>
                
                    </Box>
                </Paper>
            </Grid>
    );
    

};
```

在Main.js中定义了以下路由以导航到DisplayForm.js

```javascript
    <Route path="/displayForm/:formID" element={<DisplayForm/>} exact/>
```

## 获取表单

listforms API用于从AEM服务器获取表单。 该API返回JSON对象数组，每个JSON对象表示表单。

```javascript
import { useState,useEffect } from "react";
import React, { Component } from "react";
import FormCard from "./components/FormCard";
import Grid from "@mui/material/Grid";
import Paper from "@mui/material/Paper";
import Container from "@mui/material/Container";
 
export default function ListForm(){
    const [fetchedForms,SetHeadlessForms] = useState([])
    const getForms=async()=>{
        const response = fetch("/adobe/forms/af/listforms")
        let headlessForms = await (await response).json();
        console.log(headlessForms.items);
        SetHeadlessForms(headlessForms.items);
    }
    useEffect( ()=>{
        getForms()
        

    },[]);
    return(
        <div>
             <div>
                <Container>
                   <Grid container spacing={3}>
                       {
                            fetchedForms.map( (afForm,index) =>
                                <FormCard headlessForm={afForm} key={index}/>
                         
                            )
                        }
                    </Grid>
                </Container>
             </div>

        </div>
    )
}
```

在上述代码中，我们使用map函数对fetchedForms进行迭代，并对fetchedForms数组中的每个项目创建一个FormCard组件并将其添加到网格容器中。 您现在可以根据要求在React应用程序中使用ListForm组件。

## 后续步骤

[用户单击信息卡时显示自适应表单](./open-form-card-view.md)
