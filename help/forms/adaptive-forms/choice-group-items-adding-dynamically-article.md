---
title: 선택 그룹 구성 요소에 항목 추가
description: 선택 그룹 구성 요소에 항목을 동적으로 추가
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
exl-id: 8fbea634-7949-417f-a4d6-9e551fff63f3
last-substantial-update: 2021-09-10T00:00:00Z
duration: 350
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '456'
ht-degree: 0%

---

# 선택 그룹 구성 요소에 동적으로 항목 추가

AEM Forms 6.5에는 CheckBox, 라디오 단추 및 이미지 목록과 같은 적응형 Forms 선택 그룹 구성 요소에 항목을 동적으로 추가하는 기능이 도입되었습니다.


사용 사례에 따라 코드 편집기와 시각적 편집기를 사용하여 항목을 추가할 수 있습니다.

**시각적 편집기 사용:** 함수 호출 또는 서비스 호출 결과에서 선택 그룹의 항목을 채울 수 있습니다. 예를 들어 REST API 호출의 응답을 사용하여 선택 그룹의 항목을 설정할 수 있습니다.

아래 스크린샷에서는 getLoanPeriods라는 서비스 호출 결과에 대해 대출 기간(년) 옵션을 설정하고 있습니다.

![규칙 편집기](assets/ruleeditor.png)

**코드 편집기 사용**: 양식에 입력한 값을 기반으로 선택 그룹의 항목을 동적으로 설정하려면 다음과 같이 하십시오. 예를 들어 다음 코드 조각은 확인란의 항목을 적응형 양식의 지원자 이름 및 배우자 필드에 입력한 값으로 설정합니다.

코드 스니펫에서 확인란 구성 요소인 WorkingMembers 항목을 설정하고 있습니다. 항목에 대한 배열은 적응형 양식의 신청자 이름 및 배우자 텍스트 필드 값을 가져와 동적으로 구축됩니다

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

시스템에서 다음을 수행하십시오.

**코드 편집기를 사용하여 항목 추가**

* [에셋 다운로드](assets/usingthecodeeditor.zip)
* [Forms 및 문서 열기](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 클릭 &quot;만들기 | File Upload(파일 업로드)&quot;를 클릭하고 이전 단계에서 다운로드한 파일을 업로드합니다
* [양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/simpleform/jcr:content?wcmmode=disabled)
* 지원자명을 입력하고 기혼으로 결혼 상태를 선택합니다.
* 배우자명 입력
* 다음 을 클릭합니다
* 혼인 상태가 기혼인 경우 지원자 이름과 배우자 이름으로 채워진 확인란이 표시됩니다

**시각적 편집기를 사용하여 항목 추가**

* [에셋 다운로드](assets/usingthevisualeditor.zip)
* 아직 Tomcat이 없는 경우 설치합니다. [tomcat 설치 지침은 여기에서 확인할 수 있습니다.](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)
* [이 zip 파일에 포함된 SampleRest.war 파일을 Tomcat에 배포합니다](assets/sample-rest.zip)
* [Forms 및 문서 열기](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 클릭 &quot;만들기 | File Upload(파일 업로드)&quot;를 클릭하고 이전 단계에서 다운로드한 파일을 업로드합니다
* [양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/amortizationschedule/jcr:content?wcmmode=disabled)
* 필드에 대출 금액을 입력하고 탭을 누릅니다. 그러면 대출 기간 필드를 표시하는 규칙이 트리거됩니다.
* 적절한 대출 기간을 선택합니다(대출 기간 항목은 나머지 호출에서 채워짐)
* 이자율을 선택하고 &quot;상각 일정 가져오기&quot;를 클릭합니다.
* 상각 테이블이 채워집니다. 상각 일정은 REST 호출을 사용하여 가져옵니다.

>[!NOTE]
> tomcat이 포트 8080에서 실행되고 AEM이 포트 4502에서 실행되고 있다고 가정합니다
