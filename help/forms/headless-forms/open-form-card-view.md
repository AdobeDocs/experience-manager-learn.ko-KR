---
title: 카드를 클릭하여 양식 표시
description: 카드 보기에서 양식 드릴다운
feature: Adaptive Forms
version: Experience Manager 6.5
jira: KT-13372
topic: Development
role: User
level: Intermediate
exl-id: c8684cd9-b9c5-4b5b-b990-27c5700cea9f
duration: 28
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '61'
ht-degree: 3%

---

# 카드 클릭에 양식 표시

다음 코드는 사용자가 카드를 클릭할 때 양식을 표시하는 데 사용되었습니다. 표시할 양식의 경로는 useParams 함수를 사용하여 url에서 추출됩니다.

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
   const extendMappings =
    {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
    };
    
    
   const getAFForm = async () =>
    {
           
        const resp = await fetch(`/adobe/forms/af/${params.formID}`);
        let formJSON = await resp.json();
        console.log("The contact form json is "+formJSON);
        setForm(formJSON.afModelDefinition)
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

## 다음 단계

[양식 제출 시 감사 메시지 표시](./display-thank-you-message.md)
