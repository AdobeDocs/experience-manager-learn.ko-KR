---
title: 선택 그룹 구성 요소에 항목 추가
seo-title: 선택 그룹 구성 요소에 항목 추가
description: 선택 그룹 구성 요소에 동적으로 항목 추가
seo-description: 선택 그룹 구성 요소에 동적으로 항목 추가
feature: adaptive-forms
topics: authoring
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: ecbd4d21c5f41b2bc6db3b409767b767f00cc5d1
workflow-type: tm+mt
source-wordcount: '527'
ht-degree: 0%

---



# 선택 그룹 구성 요소에 동적으로 항목 추가

AEM Forms 6.5는 확인란, 라디오 단추 및 이미지 목록과 같은 적응형 Forms 선택 그룹 구성 요소에 항목을 동적으로 추가하는 기능을 도입했습니다.

[이 기능은 샘플 서버에서 실시간으로 사용할 수 있습니다](https://forms.enablementadobe.com/content/samples/samples.html?query=0). 동적 확인란 항목 카드를 검색하고 &quot;사용해 보기&quot;를 클릭합니다.


사용 사례에 따라 시각적인 편집기와 코드 편집기를 사용하여 항목을 추가할 수 있습니다.

**시각적 편집기 사용:** 함수 호출 또는 서비스 호출 결과에서 선택 그룹의 항목을 채울 수 있습니다. 예를 들어 REST API 호출의 응답을 받아 선택 그룹의 항목을 설정할 수 있습니다.

아래 스크린샷에서는 대출 기간(년) 옵션을 getRoanPeriods라는 서비스 호출 결과로 설정합니다.

![규칙 편집기](assets/ruleeditor.png)

**코드 편집기**&#x200B;사용:양식에 입력한 값을 기반으로 선택 그룹의 항목을 동적으로 설정하려는 경우 예를 들어, 다음 코드 조각은 확인란 항목을 응용 양식의 지원자 이름 및 배우자 필드에 입력된 값으로 설정합니다.

코드 조각에서 확인란 구성 요소인 WorkingMembers 항목을 설정합니다. 적응형 양식의 applicientName 및 배우자 텍스트 필드 값을 가져와 항목의 배열이 동적으로 만들어지고 있습니다

```javascript
 
 if(MaritalStatus.value=="Married")
  {
WorkingMembers.items =["spouse="+spouse.value,"applicant="+applicantName.value];
  }
else
  {
    WorkingMembers.items =["applicant="+applicantName.value];
  }
```

제출된 데이터는 다음과 같습니다

```xml
<afUnboundData>

<data>

<applicantName>John Jacobs</applicantName>

<MaritalStatus>Married</MaritalStatus>

<spouse>Gloria Rios</spouse>

<WorkingMembers>spouse,applicant</WorkingMembers>

</data>

</afUnboundData>
```

**규칙 편집기를 사용하여 항목 추가**

>[!VIDEO](https://video.tv.adobe.com/v/26847?quality=12&learn=on)

**코드 편집기를 사용하여 항목 추가**

>[!VIDEO](https://video.tv.adobe.com/v/26848?quality=12&learn=on)

시스템에서 이 작업을 시도하려면:

**코드 편집기를 사용하여 항목 추가**

* [자산 다운로드](assets/usingthecodeeditor.zip)
* [Forms 및 문서 열기](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* &quot;만들기 | 파일 업로드&quot;를 클릭하고 이전 단계에서 다운로드한 파일을 업로드합니다
* [양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/simpleform/jcr:content?wcmmode=disabled)
* 신청자 이름을 입력하고 결혼할 혼인 상태를 선택합니다.
* 배우자 이름 입력
* 다음을 클릭합니다
* 혼인 상태가 결혼한 경우 신청자 이름과 배우자 이름이 채워진 확인란을 선택해야 합니다

**시각적 편집기를 사용하여 항목 추가**

* [자산 다운로드](assets/usingthevisualeditor.zip)
* 아직 설치하지 않은 경우 Tomcat을 설치합니다. [tomcat 설치 지침은](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)
* [Tomcat에서 SampleRest.war 파일 배포](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)
* [Forms 및 문서 열기](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* &quot;만들기 | 파일 업로드&quot;를 클릭하고 이전 단계에서 다운로드한 파일을 업로드합니다
* [양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/amortizationschedule/jcr:content?wcmmode=disabled)
* 대출 금액을 입력하고 필드 밖으로 탭을 입력합니다. 그러면 대출 기간 필드가 표시되는 규칙이 트리거됩니다.
* 적절한 대출 기간 선택(대출 기간의 항목이 나머지 호출에서 채워짐)
* 이자율을 선택하고 &quot;Get Amortization Schedule&quot;을 클릭합니다.
* 상용화 테이블을 채워야 합니다. REST 호출을 사용하여 상환 일정을 가져옵니다.

>[!NOTE]
> 포트 4502의 포트 8080 및 AEM에서 tomcat이 실행 중이라고 가정합니다.
