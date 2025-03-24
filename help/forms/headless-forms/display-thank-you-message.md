---
title: 양식 제출 시 감사 메시지 표시
description: onSubmitSuccess 핸들러를 사용하여 React 앱에 구성된 감사 메시지를 표시합니다.
feature: Adaptive Forms
version: Experience Manager 6.5
jira: KT-13490
topic: Development
role: User
level: Intermediate
exl-id: 489970a6-1b05-4616-84e8-52b8c87edcda
duration: 61
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 0%

---

# 구성된 감사 메시지 표시

양식 제출에 대한 감사 메시지는 양식을 작성하고 제출한 사용자에게 감사를 표하고 인정하는 사려 깊은 방법입니다. 이것은 그들의 제출이 접수되었고 감사되었다는 확인의 역할을 합니다. 감사 메시지는 적응형 양식의 안내서 컨테이너에 있는 제출 탭을 사용하여 구성됩니다

![감사 메시지](assets/thank-you-message.png)

구성된 감사 메시지는 AdaptiveForm 슈퍼 구성 요소의 onSuccess 이벤트 핸들러에서 액세스할 수 있습니다.
onSuccess 이벤트를 연결하는 코드와 onSuccess 이벤트 처리기의 코드가 아래에 나와 있습니다

```javascript
<AdaptiveForm mappings={extendMappings} onSubmitSuccess={onSuccess} formJson={selectedForm}/>
```

```javascript
const onSuccess=(action) =>{
        let body = action.payload?.body;
        debugger;
        setThankYouMessage(body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));
        console.log("Thank you message "+body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));

      }
```

연락처 기능 구성 요소의 전체 코드는 다음과 같습니다

```javascript
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import { useState,useEffect } from "react";
import { AdaptiveForm } from "@aemforms/af-react-renderer";
export default function Contact(){
  
    const [selectedForm, setForm] = useState("");
    const [thankYouMessage, setThankYouMessage] = useState("");
    const [formSubmitted, setFormSubmitted] = useState(false);
  
    const extendMappings = {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
      };
     const onSuccess=(action) =>{
        let body = action.payload?.body;
        debugger;
        setFormSubmitted(true);
        setThankYouMessage(body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));
        // Remove any html tags in the thank you message
        console.log("Thank you message "+body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));

      }
      
      const getForm = async () => {
        
        const resp = await fetch('/adobe/forms/af/L2NvbnRlbnQvZm9ybXMvYWYvY29udGFjdHVz');
        // Get the form id manually using the listform api
        let formJSON = await resp.json();
        setForm(formJSON.afModelDefinition)
      }
      useEffect( ()=>{
        getForm()
        

    },[]);
    
    return(
        
        <div>
           {!formSubmitted ?
            (
                <div>
                    <h1>Get in touch with us!!!!</h1>
                    <AdaptiveForm mappings={extendMappings} onSubmitSuccess={onSuccess} formJson={selectedForm}/>
                </div>
            ) :
            (
                <div>
                    <div>{thankYouMessage}</div>
                </div>
            )}
        </div>
      
          
        
    )
}
```

위의 코드는 적응형 양식에 사용된 구성 요소에 매핑된 기본 html 구성 요소를 사용합니다. 예를 들어 텍스트 입력 적응형 양식 구성 요소를 TextField 구성 요소에 매핑합니다. 문서 [에 사용된 기본 구성 요소는 여기에서 다운로드할 수 있습니다](./assets/native-components.zip)
