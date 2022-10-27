---
title: API를 사용하여 AEM Forms을 사용하여 레코드 문서 생성
description: 프로그래밍 방식으로 DOR(Document Of Record) 생성
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 9a3b2128-a383-46ea-bcdc-6015105c70cc
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 1%

---

# API를 사용하여 AEM Forms에서 레코드 문서 생성 {#using-api-to-generate-document-of-record-with-aem-forms}

프로그래밍 방식으로 DOR(Document Of Record) 생성

이 문서에서는 `com.adobe.aemds.guide.addon.dor.DoRService API` 생성 **기록 문서** 프로그래밍 방식으로 수행할 수 있습니다. [기록 문서](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) 적응형 양식에 캡처된 데이터의 PDF 버전입니다.

1. 다음은 코드 조각입니다. 첫 번째 줄이 DOR 서비스를 받습니다.
1. DoROptions를 설정합니다.
1. DoRService의 render 메서드를 호출하고 DoROptions 개체를 render 메서드에 전달합니다

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

로컬 시스템에서 이 작업을 수행하려면 다음 단계를 수행하십시오

1. [패키지 관리자를 사용하여 문서 자산을 다운로드하여 설치합니다](assets/dor-with-api.zip)
1. 의 일부로 제공된 DevelopingWithServiceUser 번들을 설치하고 시작했는지 확인하십시오 [서비스 사용자 만들기 문서](service-user-tutorial-develop.md)
1. [configMgr에 로그인](http://localhost:4502/system/console/configMgr)
1. Apache Sling Service User Mapper Service 검색
1. 다음 항목을 확인하십시오 _DevelopingWithServiceUser.core:getformsresourceresolver=fd-service_ 서비스 매핑 섹션에서
1. [양식을 엽니다.](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. 양식을 작성하고 &#39; PDF 보기 &#39; 를 클릭합니다.
1. 브라우저에 새 탭에 DOR가 표시됩니다


**문제 해결 팁**

PDF이 새 브라우저 탭에 표시되지 않습니다.

1. 브라우저에서 팝업을 차단하지 않는지 확인합니다
1. 이에 설명된 단계를 따르도록 하십시오 [문서](service-user-tutorial-develop.md)
1. &#39;DevelopingWithServiceUser&#39; 번들이 *활성 상태*
1. 시스템 사용자 &#39; 데이터 &#39;에 다음 노드에 대한 읽기, 수정 및 만들기 권한이 있는지 확인합니다. `/content/usergenerated/content/aemformsenablement`
