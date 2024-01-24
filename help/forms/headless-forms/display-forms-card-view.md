---
title: 카드 보기에서 가져온 양식 표시
description: listforms API를 사용하여 양식 표시
feature: Adaptive Forms
version: 6.5
jira: KT-13311
topic: Development
role: User
level: Intermediate
exl-id: c01ad68e-23c9-4564-8e3e-1924af34a493
duration: 110
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 0%

---

# 카드 형식으로 양식 가져오기 및 표시

카드 보기 형식은 정보나 데이터를 카드 형태로 제시하는 디자인 패턴이다. 각 카드는 개별 콘텐츠 또는 데이터 항목을 나타내며 일반적으로 특정 요소가 배열된 시각적으로 구분된 컨테이너로 구성됩니다.
React에서 클릭 가능한 카드는 카드 또는 타일과 유사하며 사용자가 클릭하거나 누를 수 있는 대화형 구성 요소입니다. 사용자가 클릭 가능한 카드를 클릭하거나 탭하면 다른 페이지로 이동하거나, 모달을 열거나, UI를 업데이트하는 등의 지정된 동작이나 동작이 트리거됩니다.

이 문서에서는 [listforms API](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms) 양식을 가져와서 카드 형식으로 표시하고 클릭 이벤트에서 적응형 양식을 엽니다.

![card-view](./assets/card-view-forms.png)

## 카드 템플릿

다음 코드는 카드 템플릿을 디자인하는 데 사용되었습니다. 카드 템플릿에는 Adobe 로고와 함께 적응형 양식의 제목 및 설명이 표시됩니다. [자료 UI 구성 요소](https://mui.com/) 이 레이아웃을 만드는 데 사용되었습니다.



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

DisplayForm.js로 이동하기 위해 Main.js에 다음 경로를 정의했습니다

```javascript
    <Route path="/displayForm/:formID" element={<DisplayForm/>} exact/>
```

## 양식 가져오기

listforms API는 AEM 서버에서 양식을 가져오는 데 사용되었습니다. API는 JSON 오브젝트 배열을 반환하며, 각 JSON 오브젝트는 양식을 나타냅니다.

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

위의 코드에서는 map 함수를 사용하여 fetchForms를 반복하고 fetchForms 배열의 모든 항목에 대해 FormCard 구성 요소가 생성되어 Grid 컨테이너에 추가됩니다. 이제 요구 사항에 따라 React 앱에서 ListForm 구성 요소를 사용할 수 있습니다.

## 다음 단계

[사용자가 카드를 클릭하면 적응형 양식 표시](./open-form-card-view.md)
