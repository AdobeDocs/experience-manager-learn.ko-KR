---
title: 선택 그룹 구성 요소에 항목 추가
description: 동적으로 선택 그룹 구성 요소에 항목 추가
feature: 적응형 양식
version: 6.5
topic: 개발
role: User
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '515'
ht-degree: 1%

---



# 선택 그룹 구성 요소에 동적으로 항목 추가

AEM Forms 6.5에서는 CheckBox, 라디오 단추 및 이미지 목록과 같은 적응형 Forms 선택 그룹 구성 요소에 항목을 동적으로 추가하는 기능을 도입했습니다.

[이 기능은 샘플 서버에서 실시간으로 사용할 수 있습니다](https://forms.enablementadobe.com/content/samples/samples.html?query=0). 동적 확인란 항목 카드를 검색하고 &quot;시도&quot;를 클릭합니다.


사용 사례에 따라 시각적 편집기와 코드 편집기를 사용하여 항목을 추가할 수 있습니다.

**시각적 편집기 사용:**  함수 호출 또는 서비스 호출 결과에서 선택 그룹 항목을 채울 수 있습니다. 예를 들어 REST API 호출의 응답을 사용하여 선택 그룹의 항목을 설정할 수 있습니다.

아래 스크린샷에서는 getLoanPeriods라는 서비스 호출 결과로 대출 기간(년) 옵션을 설정하고 있습니다.

![규칙 편집기](assets/ruleeditor.png)

**코드 편집기 사용**: 선택한 그룹의 항목을 양식에 입력한 값에 따라 동적으로 설정하려면 예를 들어, 다음 코드 조각은 확인란의 항목을 적응형 양식의 지원자 이름 및 배우자 필드에 입력한 값으로 설정합니다.

코드 조각에서 확인란 구성 요소인 WorkingMembers 항목을 설정하고 있습니다. 적응형 양식의 aprierName 및 배우자 텍스트 필드의 값을 가져오므로 항목의 배열이 동적으로 빌드되고 있습니다

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

시스템에서 사용하려면 다음을 수행하십시오.

**코드 편집기를 사용하여 항목 추가**

* [자산 다운로드](assets/usingthecodeeditor.zip)
* [Forms 및 문서 열기](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 만들기 를 클릭합니다. | 파일 업로드&quot; 및 이전 단계에서 다운로드한 파일을 업로드합니다
* [양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/simpleform/jcr:content?wcmmode=disabled)
* 지원자명을 입력하고 혼인상태를 선택합니다
* 배우자 이름을 입력합니다
* 다음을 클릭합니다
* 혼인 상태가 결혼한 경우 지원자명과 배우자 이름으로 채워진 확인란이 표시됩니다

**시각적 편집기를 사용하여 항목 추가**

* [자산 다운로드](assets/usingthevisualeditor.zip)
* Tomcat이 없는 경우 설치합니다. [tomcat 설치 지침은 여기에서 확인할 수 있습니다.](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)
* [Tomcat에서 SampleRest.war 파일 배포](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)
* [Forms 및 문서 열기](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 만들기 를 클릭합니다. | 파일 업로드&quot; 및 이전 단계에서 다운로드한 파일을 업로드합니다
* [양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/amortizationschedule/jcr:content?wcmmode=disabled)
* 대출 금액을 입력하고 필드 바깥으로 탭합니다. 이렇게 하면 대출 기간 필드를 표시하는 규칙이 트리거됩니다.
* 적절한 대출 기간을 선택합니다(대출 기간 항목은 나머지 호출에서 채워짐).
* 이자율을 선택하고 &quot;상환 일정 가져오기&quot;를 클릭합니다
* 상각 테이블을 채워야 합니다. REST 호출을 사용하여 상환 일정을 가져옵니다.

>[!NOTE]
> tomcat이 포트 8080 및 포트 4502의 AEM에서 실행되고 있다고 가정합니다
