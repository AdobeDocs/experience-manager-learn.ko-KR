---
title: 감사 인사 페이지 제출
seo-title: 감사 인사 페이지 제출
description: 적응형 양식 제출 시 감사 페이지 표시
seo-description: 적응형 양식 제출 시 감사 페이지 표시
uuid: ec695b87-083a-47f6-92ac-c9a6dc2b85fb
feature: adaptive-forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
translation-type: tm+mt
source-git-commit: 0bccdea82f6db391cbca3ab06009a7b4420a38bf
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 0%

---


# 감사 인사 페이지 제출 {#submitting-to-thank-you-page}

REST에 제출 끝점 옵션은 양식에 입력된 데이터를 HTTP GET 요청의 일부로 구성된 확인 페이지로 전달합니다. 요청할 필드 이름을 추가할 수 있습니다. 요청의 형식은 다음과 같습니다.

\{fieldName\} = \{parameterName\}. 예를 들어 submitterName은 적응형 양식 필드의 이름이고 제출자는 매개 변수의 이름입니다. 감사 인사 페이지에서 request.getParameter(&quot;submitter&quot;)를 사용하여 제출자 이름 필드의 값을 유지할 수 있습니다.

submitterName=submitter

아래 스크린샷에서는 /content/thanks에 있는 감사 페이지에 적응형 양식을 제출합니다. 감사 인사 페이지에 양식 필드 값을 포함하는 3개의 요청 속성을 전달했습니다.

![thank](assets/thankyoupage.gif)

POST을 통해 외부 종단점에 제출할 수도 있습니다. 이를 위해서는 &quot;게시 요청 활성화&quot; 확인란을 선택하고 외부 끝점에 대한 URL을 제공해야 합니다. 양식을 제출하면 감사 인사 페이지가 표시되고 POST 끝점이 동시에 호출됩니다.

![capture](assets/capture.gif)


서버에서 이 기능을 테스트하려면 아래 지침을 따르십시오.

* 패키지 관리자를 사용하여 이 아티클과 연결된 [에셋 파일을 AEM으로 가져옵니다.](assets/submittingtorestendpoint.zip)
* 브라우저를 요청 [시간 초과 양식으로 지정합니다.](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* 필요한 필드를 입력하고 양식을 제출합니다.
* 페이지에 정보가 채워진 감사 페이지를 받으십시오

