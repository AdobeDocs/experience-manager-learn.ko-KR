---
title: 클릭 가능한 이미지 구성 요소 만들기
description: AEM Forms Cloud Service에서 클릭 가능한 이미지 구성 요소 만들기
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15968
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 54344a6d-51d3-4a63-b1f1-283bddbc0f8f
source-git-commit: ba744f95f8d1f0b982cd5430860f0cb0945a4cda
workflow-type: tm+mt
source-wordcount: '83'
ht-degree: 0%

---

# 클릭 이벤트 처리

클라이언트 라이브러리를 만들고 클라이언트 라이브러리를 구성 요소와 연결합니다.

클라이언트 라이브러리의 Javascript 파일에 다음 코드를 추가하여 클릭 이벤트를 처리합니다.
선택된 상태에 따라, 종료 지점에 의해 반환되는 적절한 데이터가 표시될 수 있다. 엔드포인트 및 표시되는 데이터의 세부 정보는 사용 사례에 따라 다릅니다.



```javascript
document.addEventListener("DOMContentLoaded", function(event)
  {
    const apiUrl = 'API end point to return data based on the state selected';
       // Select all <path> elements
    const paths = document.querySelectorAll('path');
    const tooltip = document.getElementById('tooltip');
    const svg = document.getElementById('svg');
    const states = document.querySelectorAll('path');
    // Loop through each <path> element and add an event listener
    states.forEach(state =>
     {
                     state.addEventListener('click', function(event)
                  {
                     alert('path clicked:'+ this.id);
                    fetch(apiUrl+this.id)
                    .then(response =>
                        {
                            // Check if the response is ok (status code in the range 200-299)
                            if (!response.ok)
                            {
                                  throw new Error('Network response was not ok ' + response.statusText);
                            }
                            // Parse the JSON from the response
                            console.log(response);
                            return response.text();
                          })
                        .then(data => {
                                // Handle the data
                              console.log(data);
                            })
                        .catch(error => {
                            // Handle any errors
                            console.error('There was a problem with the fetch operation:', error);
                            });
                     });
        });

});
```
