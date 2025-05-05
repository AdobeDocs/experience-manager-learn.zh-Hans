---
title: 获取要嵌入的自适应表单的JSON
description: 使用API获取自适应表单的json
feature: Adaptive Forms
version: Experience Manager 6.5
jira: KT-13285
topic: Development
role: User
level: Intermediate
exl-id: ee534724-54ea-48e1-8c92-de1c56a928d4
duration: 50
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 1%

---

# 获取表单的JSON

登录到AEM Forms创作实例，并使用&#x200B;**Blank with Core Components**&#x200B;模板创建新的自适应组件。 将表单发布到发布实例。

要嵌入表单，我们首先通过针对发布服务器进行get调用来获取自适应表单的json。

以下代码片段提取名为&#x200B;**contactus**&#x200B;的自适应表单的json

```javascript
const getForm = async () => {
        
        const resp = await fetch('/adobe/forms/af/L2NvbnRlbnQvZm9ybXMvYWYvZmlyc3RoZWFkbGVzcw==');
        // Get the form id manually using the listform api
        let formJSON = await resp.json();
        console.log("The contact form json is "+formJSON);
        setForm(formJSON.afModelDefinition)
      }
```

Contact函数组件的完整代码如下所示

```javascript
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import Panel from './components/Panel';
import { useState,useEffect } from "react";
import { AdaptiveForm } from "@aemforms/af-react-renderer";

export default function Contact(){
   const [selectedForm, setForm] = useState("");
   const extendMappings = {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
      };
    const getForm = async () => {
        
        const resp = await fetch('/adobe/forms/af/dor/L2NvbnRlbnQvZm9ybXMvYWYvcmlzaGk=');
        let formJSON = await resp.json();
        setForm(formJSON.afModelDefinition)
      
      }
      useEffect( ()=>{
        getForm();
        

    },[]);
    return(
        
        <div>
            <h1>Get in touch with us!!!!</h1>
            <AdaptiveForm mappings={extendMappings} formJson={selectedForm} />
      
          
        </div>
    )
}
```

上述代码使用本机html组件，这些组件映射到自适应表单中使用的组件。 例如，我们将文本输入自适应表单组件映射到TextField组件。 可以从此处[&#128279;](./assets/native-components.zip)下载文章中使用的本机组件

## 后续步骤

[从下拉列表中选择表单](./select-form-from-drop-down-list.md)
