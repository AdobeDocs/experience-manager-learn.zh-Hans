---
title: 获取要嵌入的自适应表单的JSON
description: 使用API获取自适应表单的json
feature: Adaptive Forms
version: 6.5
kt: 13285
topic: Development
role: User
level: Intermediate
source-git-commit: c6e83a627743c40355559d9cdbca2b70db7f23ed
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---


# 获取表单的JSON

登录AEM Forms创作实例，然后使用 **包含核心组件的空白** 模板。 将表单发布到发布实例。

要嵌入表单，我们首先会通过对发布服务器发起get调用，来获取自适应表单的json。

以下代码片段将获取名为的自适应表单的json **contactus**

```javascript
const getForm = async () => {
        const resp = await fetch('/content/forms/af/contactus/jcr:content/guideContainer.model.json');
        let formJSON = await resp.json();
        console.log(formJSON);
        setForm(formJSON);
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
        
        const resp = await fetch('/content/forms/af/contactus/jcr:content/guideContainer.model.json');
        let formJSON = await resp.json();
        console.log(formJSON);
        setForm(formJSON);
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

上述代码使用本机html组件，这些组件会映射到自适应表单中使用的组件。 例如，我们将文本输入的自适应表单组件映射到文本字段组件。 文章中使用的本机组件 [可从此处下载](./assets/native-components.zip)