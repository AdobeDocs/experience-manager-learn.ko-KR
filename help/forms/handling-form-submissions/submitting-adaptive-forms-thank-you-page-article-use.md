---
title: 감사 인사 페이지 제출
description: 적응형 양식 제출 시 감사 페이지 표시
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
topic: Development
role: Developer
level: Beginner
exl-id: 85e1b450-39c0-4bb8-be5d-d7f50b102f3d
last-substantial-update: 2020-07-07T00:00:00Z
duration: 51
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 6%

---

# 감사 인사 페이지 제출 {#submitting-to-thank-you-page}

REST 끝점에 제출 옵션은 양식에 입력된 데이터를 HTTP GET 요청의 일부로 구성된 확인 페이지에 전달합니다. 요청할 필드 이름을 추가할 수 있습니다. 요청의 형식은 다음과 같습니다.

\{fieldName\} = \{parameterName\}. 예를 들어 submitterName은 적응형 양식 필드의 이름이고 submitter는 매개 변수의 이름입니다. 감사 페이지에서 request.getParameter(&quot;submitter&quot;)를 사용하여 제출자 매개 변수에 액세스하여 제출자 이름 필드의 값을 가져올 수 있습니다.

`submitterName=submitter`

아래 스크린샷에서는 /content/thankyou에 있는 감사 인사 페이지에 적응형 양식을 제출합니다. 이 감사 인사 페이지에는 양식 필드 값을 포함하는 3개의 요청 속성이 전달되었습니다.

![감사 인사 페이지](assets/thankyoupage.gif)

POST을 통해 외부 끝점에 제출할 수도 있습니다. 이렇게 하려면 &quot;게시물 요청 활성화&quot; 확인란을 선택하고 외부 끝점에 대한 URL을 제공하기만 하면 됩니다. 양식을 제출하면 감사 페이지가 표시되고 POST 끝점이 동시에 호출됩니다.

![구성 캡처](assets/capture.gif)

서버에서 이 기능을 테스트하려면 아래 지침을 따르십시오.

* 패키지 관리자를 사용하여 이 문서와 연결된 [자산 파일을 AEM으로 가져오기](assets/submittingtorestendpoint.zip)
* 브라우저를 [휴무 요청 양식](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)(으)로 지정
* 필수 필드를 입력하고 양식 제출
* 페이지에 정보가 채워진 감사 페이지가 표시됩니다
