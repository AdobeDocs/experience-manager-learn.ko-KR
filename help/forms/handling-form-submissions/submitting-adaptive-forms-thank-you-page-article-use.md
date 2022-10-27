---
title: 감사 인사 페이지에 제출
seo-title: Submitting To Thank You Page
description: 적응형 양식 제출 시 감사 인사 페이지를 표시합니다
seo-description: Display a thank you page on submitting Adaptive Form
uuid: ec695b87-083a-47f6-92ac-c9a6dc2b85fb
feature: Adaptive Forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
topic: Development
role: Developer
level: Beginner
exl-id: 85e1b450-39c0-4bb8-be5d-d7f50b102f3d
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 0%

---

# 감사 인사 페이지에 제출 {#submitting-to-thank-you-page}

REST에 제출 끝점 옵션은 HTTP GET 요청의 일부로 양식에 입력된 데이터를 구성된 확인 페이지에 전달합니다. 요청할 필드의 이름을 추가할 수 있습니다. 요청 형식은 다음과 같습니다.

\{fieldName\} = \{parameterName\}. 예를 들어 submitterName은 적응형 양식 필드의 이름이고 submitter는 매개 변수의 이름입니다. 감사 인사 페이지에서 request.getParameter(&quot;submitter&quot;) 를 사용하여 제출자 매개 변수에 액세스하여 제출자 이름 필드의 값을 보유할 수 있습니다.

`submitterName=submitter`

아래 스크린샷에서는 /content/thank에 있는 감사 인사 페이지에 적응형 양식을 제출합니다. 감사 인사 페이지에 양식 필드 값을 포함하는 3개의 요청 속성을 전달합니다.

![감사 인사 페이지](assets/thankyoupage.gif)

POST을 통해 외부 종단점에 제출할 수도 있습니다. 이를 위해서는 &quot;post 요청 활성화&quot; 확인란을 선택하고 외부 엔드포인트에 대한 URL을 제공해야 합니다. 양식을 제출하면 감사 인사 페이지가 표시되고 POST 종단점이 동시에 호출됩니다.

![구성 캡처](assets/capture.gif)

서버에서 이 기능을 테스트하려면 아래에 언급된 지침을 따르십시오.

* 가져오기 [패키지 관리자를 사용하여 이 문서와 연결된 자산 파일을 AEM에 추가합니다](assets/submittingtorestendpoint.zip)
* 브라우저를 [요청 시간 해제 양식](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* 필수 필드를 입력하고 양식을 제출합니다
* 페이지에 정보가 채워져 감사 인사 페이지가 표시됩니다
