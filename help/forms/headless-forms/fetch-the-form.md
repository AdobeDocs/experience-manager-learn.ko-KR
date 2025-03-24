---
title: 포함할 적응형 양식의 JSON 가져오기
description: API를 사용하여 적응형 양식의 JSON을 가져옵니다
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

# 양식의 JSON 가져오기

AEM Forms 작성자 인스턴스에 로그인하고 **핵심 구성 요소가 있는 빈 항목** 템플릿을 사용하여 새 적응형 항목을 만듭니다. 양식을 게시 인스턴스에 게시합니다.

양식을 임베드하려면 먼저 게시 서버에 대해 get 호출을 수행하여 적응형 양식의 json을 가져옵니다.

다음 코드 조각은 **contactus**&#x200B;라는 적응형 양식의 json을 가져옵니다.

```javascript
const getForm = async () => {
        
        const resp = await fetch('/adobe/forms/af/L2NvbnRlbnQvZm9ybXMvYWYvZmlyc3RoZWFkbGVzcw==');
        // Get the form id manually using the listform api
        let formJSON = await resp.json();
        console.log("The contact form json is "+formJSON);
        setForm(formJSON.afModelDefinition)
      }
```

연락처 기능 구성 요소의 전체 코드는 다음과 같습니다

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

위의 코드는 적응형 양식에 사용된 구성 요소에 매핑된 기본 html 구성 요소를 사용합니다. 예를 들어 텍스트 입력 적응형 양식 구성 요소를 TextField 구성 요소에 매핑합니다. 문서 [에 사용된 기본 구성 요소는 여기에서 다운로드할 수 있습니다](./assets/native-components.zip)

## 다음 단계

[드롭다운 목록에서 양식 선택](./select-form-from-drop-down-list.md)
