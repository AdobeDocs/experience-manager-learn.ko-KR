---
title: DAM에 AEM Forms DoR 태깅 및 저장
description: 이 문서는 AEM DAM에서 AEM Forms에서 생성한 DoR을 저장하고 태깅하는 사용 사례를 안내합니다. 문서의 태깅은 제출된 양식 데이터를 기반으로 수행됩니다.
feature: 적응형 양식
version: 6.4,6.5
topic: 개발
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 0%

---


# DAM에 AEM Forms DoR 태깅 및 저장 {#tagging-and-storing-aem-forms-dor-in-dam}

이 문서는 AEM DAM에서 AEM Forms에서 생성한 DoR을 저장하고 태깅하는 사용 사례를 안내합니다. 문서의 태깅은 제출된 양식 데이터를 기반으로 수행됩니다.

AEM DAM에서 AEM Forms에서 생성한 Document of Record(DoR)를 저장하고 태깅하는 것이 고객의 일반적인 질문입니다. 문서의 태깅은 적응형 Forms의 제출된 데이터를 기반으로 해야 합니다. 예를 들어, 제출된 데이터의 고용 상태가 &quot;사용 중단&quot;인 경우, &quot;사용 중단&quot; 태그를 사용하여 문서에 태그를 지정하고 문서를 DAM에 저장하려고 합니다.

사용 사례는 다음과 같습니다.

* 사용자가 적응형 양식을 채웁니다. 적응형 양식에서는 사용자의 혼인 상태(단일 사용)와 고용 상태(퇴직 전)가 캡처됩니다.
* 양식 제출 시 AEM 워크플로우가 트리거됩니다. 이 워크플로우에서는 혼인 상태(단일) 및 고용 상태(퇴직)로 문서에 태그를 지정하고 문서를 DAM에 저장합니다.
* 문서가 DAM에 저장되면 관리자는 이러한 태그로 문서를 검색할 수 있어야 합니다. 예를 들어, 단일 또는 폐기된 항목에 대한 검색은 적절한 DoR을 가져옵니다.

이 사용 사례를 충족하기 위해 사용자 지정 프로세스 단계가 작성됩니다. 이 단계에서는 제출된 데이터에서 적절한 데이터 요소의 값을 가져옵니다. 그런 다음 이 값을 사용하여 태그 타일을 구성합니다. 예를 들어 혼인 상태 요소의 값이 &quot;단일&quot;이면 태그 제목은 **Peak:EmploymentStatus/Single이 됩니다. **TagManager API 를 사용하여 태그를 찾아 DoR에 태그를 적용합니다.

다음 코드 조각은 태그를 찾고 태그를 문서에 적용하는 방법을 보여 줍니다.

```java
Tag tagFound = tagManager.resolveByTitle(tagTitle+xmlElement.getTextContent());
//tagTitle is "Peak:EmploymentStatus/" and the xmlElement.getTextContent() will return the value Single. So the tag title becomes Peak:EmploymentStatus/Single. Once the tag is found we put the tag in array and apply the tags to the resource as shown below
tagArray[i] = tagFound;
tagManager.setTags(metadata, tagArray, true);
```

시스템에서 이 샘플을 사용하려면 아래 나열된 단계를 수행하십시오.
* [Developingwithserviceuser 번들 배포](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [setvalue 번들을 다운로드하여 배포합니다](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). 제출된 양식 데이터에서 태그를 설정하는 사용자 지정 OSGI 번들입니다.

* [샘플 적응형 양식 다운로드](assets/tag-and-store-in-dam-assets.zip)

* [Forms 및 문서로 이동합니다.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* 만들기 를 클릭합니다. | 파일 업로드 및 samplleaptiveform.zip 업로드

* [AEM 패키지 관리자](assets/tag-and-store-in-dam-assets.zip) 를 사용하여 문서 자산 가져오기
* 미리 보기 모드](http://localhost:4502/content/dam/formsanddocuments/summit/peakform/jcr:content?wcmmode=disabled)에서 [샘플 양식을 엽니다. 사람 섹션을 입력하고 양식을 제출합니다.
* [DAM의 Peak 폴더로 이동합니다](http://localhost:4502/assets.html/content/dam/Peak). Peak 폴더에 DoR이 표시됩니다. 문서의 속성을 확인합니다. 태깅을 적절히 해야 합니다.
축하합니다!! 시스템에 샘플을 설치했습니다.

* 양식 제출 시 트리거되는 [워크플로우](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html)를 살펴보겠습니다.
* 워크플로우의 첫 번째 단계는 지원자명과 거주군을 연결하여 고유한 파일 이름을 생성합니다.
* 워크플로우의 두 번째 단계는 태그 계층 구조 및 태그를 지정해야 하는 양식 필드 요소를 전달합니다. 프로세스 단계에서는 제출된 데이터에서 값을 추출하고 문서에 태그를 지정해야 하는 태그 제목을 구성합니다.
* DAM의 다른 폴더에 DoR을 저장하려면 아래 스크린샷에 지정된 구성 속성을 사용하여 폴더 위치를 지정합니다.

다른 두 매개 변수는 적응형 양식 제출 옵션에 지정된 대로 DoR 및 데이터 파일 경로에 따라 다릅니다. 여기서 지정하는 값이 적응형 양식 제출 옵션에서 지정한 값과 일치하는지 확인하십시오.

![태그 도르](assets/tag_dor_service_configuration.gif)

