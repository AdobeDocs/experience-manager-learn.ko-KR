---
title: API를 사용하여 AEM Forms에서 기록 문서 생성
seo-title: API를 사용하여 AEM Forms에서 기록 문서 생성
description: 프로그래밍 방식으로 기록 문서(DOR) 생성
seo-description: API를 사용하여 AEM Forms에서 기록 문서 생성
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 94ac3b13-01b4-4198-af81-e5609c80324c
discoiquuid: ba91d9df-dc61-47d8-8e0a-e3f66cae6a87
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 1%

---


# API를 사용하여 AEM Forms {#using-api-to-generate-document-of-record-with-aem-forms}에서 기록 문서 생성

프로그래밍 방식으로 기록 문서(DOR) 생성

이 문서에서는 `com.adobe.aemds.guide.addon.dor.DoRService API`을 사용하여 **레코드 문서**&#x200B;를 프로그래밍 방식으로 생성하는 방법을 설명합니다. [기록 문서](https://docs.adobe.com/content/help/en/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) 는 적응형 양식으로 캡처된 데이터의 PDF 버전입니다.

1. 다음은 코드 조각입니다. 첫 번째 줄은 DOR 서비스를 받습니다.
1. DoROptions를 설정합니다.
1. DoRsService의 render 메서드를 호출하고 DoROptions 객체를 렌더링 메서드에 전달합니다

```java
com.adobe.aemds.guide.addon.dor.DoRService dorService = sling.getService(com.adobe.aemds.guide.addon.dor.DoRService.class);
com.adobe.aemds.guide.addon.dor.DoROptions dorOptions =  new com.adobe.aemds.guide.addon.dor.DoROptions();
 dorOptions.setData(dataXml);
 dorOptions.setFormResource(resource);
 java.util.Locale locale = new java.util.Locale("en");
 dorOptions.setLocale(locale);
 com.adobe.aemds.guide.addon.dor.DoRResult dorResult = dorService.render(dorOptions);
 byte[] fileBytes = dorResult.getContent();
 com.adobe.aemfd.docmanager.Document dorDocument = new com.adobe.aemfd.docmanager.Document(fileBytes);
```

로컬 시스템에서 사용하려면 다음 단계를 따르십시오

1. [패키지 관리자를 사용하여 아티클 에셋 다운로드 및 설치](assets/dor-with-api.zip)
1. [서비스 사용자 만들기 아티클](service-user-tutorial-develop.md)의 일부로 제공된 DevelopingWithServiceUser 번들을 설치하고 시작했는지 확인하십시오.
1. [configMgr에 로그인](http://localhost:4502/system/console/configMgr)
1. Apache Sling 서비스 사용자 매퍼 서비스 검색
1. 서비스 매핑 섹션의 _DevelopingWithServiceUser.core:getformsresourissolver=fd-service_ 항목을 확인하십시오.
1. [양식 열기](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. 양식을 작성하고 &#39; PDF 보기 &#39; 를 클릭합니다.
1. 브라우저에서 새 탭에 DOR가 표시됩니다.


**문제 해결 팁**

새 브라우저 탭에 PDF가 표시되지 않습니다.

1. 브라우저의 팝업 차단 안 함
1. 이 [article](service-user-tutorial-develop.md)에 설명된 단계를 따랐는지 확인합니다.
1. &#39;DevelopingWithServiceUser&#39; 번들이 *활성 상태*&#x200B;인지 확인하십시오.
1. 시스템 사용자 &#39; 데이터 &#39; 의 노드 `/content/usergenerated/content/aemformsenablement`에 대한 읽기, 수정 및 만들기 권한이 있는지 확인하십시오.

