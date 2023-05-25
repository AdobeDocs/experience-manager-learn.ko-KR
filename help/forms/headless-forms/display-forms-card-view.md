---
title: 카드 보기에서 가져온 양식 표시
description: listforms API를 사용하여 양식 표시
feature: Adaptive Forms
version: 6.5
kt: 13311
topic: Development
role: User
level: Intermediate
source-git-commit: 6aa3dff44a7e6f1f8ac896e30319958d84ecf57f
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 0%

---


# 카드 형식으로 양식 가져오기 및 표시

카드 보기 형식은 정보나 데이터를 카드 형태로 제시하는 디자인 패턴이다. 각 카드는 개별 콘텐츠 또는 데이터 항목을 나타내며 일반적으로 특정 요소가 배열된 시각적으로 구분된 컨테이너로 구성됩니다. 이 문서에서는 [listforms API](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms) 양식을 가져오고 아래와 같이 카드 형식으로 표시하려면

![card-view](./assets/card-view-forms.png)

## 카드 템플릿

다음 코드는 카드 템플릿을 디자인하는 데 사용되었습니다. 카드 템플릿에는 Adobe 로고와 함께 적응형 양식의 제목 및 설명이 표시됩니다. [자료 UI 구성 요소](https://mui.com/) 이 레이아웃을 만드는 데 사용되었습니다.

```javascript
import Paper from "@mui/material/Paper";
import Grid from "@mui/material/Grid";
import Container from "@mui/material/Container";
import { Typography } from "@mui/material";
import { Box } from "@mui/system";
const FormCard =({headlessForm}) => {
    return (
              <Grid item xs={3}>
                <Paper elevation={3}>
                    <img src="/content/dam/formsanddocuments/registrationform/jcr:content/renditions/cq5dam.thumbnail.48.48.png" className="img"/>
                    <Box padding={3}>
                    <Typography variant="subtititle2" component="h2">
                        {headlessForm.title}
                    
                    </Typography>
                    <Typography variant="subtititle3" component="h4">
                        {headlessForm.description}
                    
                    </Typography>
                    </Box>
                </Paper>
                </Grid>
          


    );
    

};
export default FormCard;
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
