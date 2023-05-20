---
title: 포함할 적응형 양식의 JSON 가져오기
description: API를 사용하여 적응형 양식의 JSON을 가져옵니다
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


# 양식의 JSON 가져오기

AEM Forms 작성자 인스턴스에 로그인하고 를 사용하여 새 적응형 양식을 만듭니다. **핵심 구성 요소가 포함된 빈 항목** 템플릿. 양식을 게시 인스턴스에 게시합니다.

양식을 임베드하려면 먼저 게시 서버에 대해 get 호출을 수행하여 적응형 양식의 json을 가져옵니다.

다음 코드 조각은 이라는 적응형 양식의 json을 가져옵니다. **접촉선**

```javascript
const getForm = async () => {
        const resp = await fetch('/content/forms/af/contactus/jcr:content/guideContainer.model.json');
        let formJSON = await resp.json();
        console.log(formJSON);
        setForm(formJSON);
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

위의 코드는 적응형 양식에 사용된 구성 요소에 매핑된 기본 html 구성 요소를 사용합니다. 예를 들어 텍스트 입력 적응형 양식 구성 요소를 TextField 구성 요소에 매핑합니다. 문서에 사용된 기본 구성 요소 [여기에서 다운로드할 수 있음](./assets/native-components.zip)