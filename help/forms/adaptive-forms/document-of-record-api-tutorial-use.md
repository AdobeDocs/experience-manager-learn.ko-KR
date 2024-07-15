---
title: API를 사용하여 AEM Forms으로 기록 문서 생성
description: 프로그래밍 방식으로 DOR(Document Of Record) 생성
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 9a3b2128-a383-46ea-bcdc-6015105c70cc
last-substantial-update: 2023-01-26T00:00:00Z
duration: 67
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 0%

---

# API를 사용하여 AEM Forms에서 기록 문서 생성 {#using-api-to-generate-document-of-record-with-aem-forms}

프로그래밍 방식으로 DOR(Document Of Record) 생성

이 문서에서는 `com.adobe.aemds.guide.addon.dor.DoRService API`을(를) 사용하여 프로그래밍 방식으로 **기록 문서**&#x200B;를 생성하는 방법을 보여 줍니다. [기록 문서](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html)은(는) 적응형 양식으로 캡처된 데이터의 PDF 버전입니다.

1. 다음은 코드 조각입니다. 첫 번째 라인은 DOR 서비스를 받습니다.
1. DoROptions를 설정합니다.
1. DoRService의 렌더링 메서드를 호출하고 DoROptions 개체를 렌더링 메서드에 전달합니다.

```java
String dataXml = request.getParameter("data");
System.out.println("Got " + dataXml);
Session session;
com.adobe.aemds.guide.addon.dor.DoRService dorService = sling.getService(com.adobe.aemds.guide.addon.dor.DoRService.class);
System.out.println("Got ... DOR Service");
com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
System.out.println("Got aem DemoListings");
resourceResolver = aemDemoListings.getFormsServiceResolver();
session = resourceResolver.adaptTo(Session.class);
resource = resourceResolver.getResource("/content/forms/af/sandbox/1201-borrower-payments");
com.adobe.aemds.guide.addon.dor.DoROptions dorOptions = new com.adobe.aemds.guide.addon.dor.DoROptions();
dorOptions.setData(dataXml);
dorOptions.setFormResource(resource);
java.util.Locale locale = new java.util.Locale("en");
dorOptions.setLocale(locale);
com.adobe.aemds.guide.addon.dor.DoRResult dorResult = dorService.render(dorOptions);
byte[] fileBytes = dorResult.getContent();
com.adobe.aemfd.docmanager.Document dorDocument = new com.adobe.aemfd.docmanager.Document(fileBytes);
resource = resourceResolver.getResource("/content/usergenerated/content/aemformsenablement");
Node paydotgov = resource.adaptTo(Node.class);
java.util.Random r = new java.util.Random();
String nodeName = Long.toString(Math.abs(r.nextLong()), 36);
Node fileNode = paydotgov.addNode(nodeName + ".pdf", "nt:file");

System.out.println("Created file Node...." + fileNode.getPath());
Node contentNode = fileNode.addNode("jcr:content", "nt:resource");
Binary binary = session.getValueFactory().createBinary(dorDocument.getInputStream());
contentNode.setProperty("jcr:data", binary);
JSONWriter writer = new JSONWriter(response.getWriter());
writer.object();
writer.key("filePath");
writer.value(fileNode.getPath());
writer.endObject();
session.save();
```

로컬 시스템에서 시도하려면 다음 단계를 따르십시오

1. [패키지 관리자를 사용하여 문서 에셋 다운로드 및 설치](assets/dor-with-api.zip)
1. [서비스 사용자 만들기 문서](service-user-tutorial-develop.md)의 일부로 제공된 DevelopingWithServiceUser 번들을 설치하고 시작했는지 확인하십시오
1. [configMgr에 로그인](http://localhost:4502/system/console/configMgr)
1. Apache Sling Service User Mapper 서비스 검색
1. 서비스 매핑 섹션에서 _DevelopingWithServiceUser.core:getformsresourceresolver=fd-service_ 항목을 확인하십시오
1. [양식 열기](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. 양식을 작성하고 &#39; PDF 보기 &#39; 를 클릭합니다.
1. 브라우저의 새 탭에 DOR이 표시됩니다


**문제 해결 팁**

PDF이 새 브라우저 탭에 표시되지 않음:

1. 브라우저에서 팝업을 차단하지 않도록 하십시오
1. AEM 서버를 관리자(적어도 Windows에서)로 시작하는지 확인하십시오.
1. &#39;DevelopingWithServiceUser&#39; 번들이 *활성 상태*&#x200B;에 있는지 확인하십시오.
1. [시스템 사용자](http://localhost:4502/useradmin) &#39; fd-service&#39;에게 다음 노드 `/content/usergenerated/content/aemformsenablement`에 대한 읽기, 수정 및 만들기 권한이 있는지 확인하십시오.
