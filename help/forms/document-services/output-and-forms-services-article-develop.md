---
title: AEM Forms에서 출력 및 Forms 서비스를 사용한 개발
seo-title: AEM Forms에서 출력 및 Forms 서비스를 사용한 개발
description: AEM Forms에서 출력 및 Forms 서비스 API 사용
seo-description: AEM Forms에서 출력 및 Forms 서비스 API 사용
uuid: be018eb5-dbe7-4101-a1a9-bee11ac97273
feature: 출력 서비스
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 57f478a9-8495-469e-8a06-ce1251172fda
topic: 개발
role: 개발자
level: 중간
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 1%

---


# AEM Forms{#developing-with-output-and-forms-services-in-aem-forms}에서 출력 및 Forms 서비스로 개발

AEM Forms에서 출력 및 Forms 서비스 API 사용

이 문서에서는 다음 사항을 살펴보겠습니다.

* 출력 서비스 - 일반적으로 이 서비스는 xml 데이터를 xdp 템플릿 또는 pdf와 병합하여 병합된 pdf를 생성하는 데 사용됩니다
* FormsService - PDF 파일에서 데이터를 가져오거나 내보낼 수 있는 다목적 서비스입니다

AEM Forms API에 대한 공식 javadoc는 [여기에](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html) 나열됩니다.

다음 코드 조각은 PDF 파일의 데이터를 내보냅니다

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

1행에서 파일 추출

Line2는 요청에서 saveLocation을 추출합니다.

FormsService를 이용할 수 있는 5행

6행에서 PDF 파일에서 xmlData 내보내기

**시스템에서 샘플 패키지를 테스트하려면**

[AEM 패키지 관리자를 사용하여 패키지 다운로드 및 설치](assets/outputandformsservice.zip)




**패키지를 설치한 후에는 [Granite CSRF 필터]에 다음 URL을 허용 목록에 추가하다 표시해야 합니다.**

1. 위에 언급된 경로를허용 목록에 추가하다하려면 아래 단계를 따르십시오.
1. [configMgr에 로그인](http://localhost:4502/system/console/configMgr)
1. Adobe Granite CSRF 필터 검색
1. 제외된 섹션에 다음 3개의 패스를 추가하고 저장합니다.
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. &quot;Sling 레퍼러 필터&quot; 검색
1. &quot;비어 있음&quot; 확인란을 선택합니다. (이 설정은 테스트용으로만 사용해야 합니다.)
샘플 코드를 테스트하는 여러 가지 방법이 있습니다. 가장 빠르고 쉬운 것은 Postman 앱을 사용하는 것입니다. Postman을 사용하면 서버에 POST 요청을 수행할 수 있습니다. 시스템에 Postman 앱을 설치합니다.
앱을 실행하고 다음 URL을 입력하여 내보내기 데이터 API를 테스트합니다

드롭다운 목록에서 &quot;POST&quot;을 선택했는지 확인합니다.
http://localhost:4502/content/AemFormsSamples/exportdata.html
&quot;인증&quot;을 &quot;기본 인증&quot;으로 지정해야 합니다. AEM 서버 사용자 이름 및 암호 지정
&quot;본문&quot; 탭으로 이동하여 아래 이미지에 표시된 대로 요청 매개 변수를 지정합니다
![내보내기](assets/postexport.png)
전송 단추를 클릭합니다.

패키지에 3개의 샘플이 포함되어 있습니다. 다음 단락은 출력 서비스 또는 Forms 서비스, 서비스 URL을 사용해야 하는 시기에 대해 설명하며 각 서비스에서 필요로 하는 입력 매개 변수입니다

**데이터 병합 및 출력 병합:**

* 출력 서비스를 사용하여 xdp 또는 pdf 문서와 데이터를 병합하여 병합된 pdf 생성
* **POST URL**:http://localhost:4502/content/AemFormsSamples/outputservice.html
* **요청 매개 변수 -**

   * xdp_or_pdf_file :데이터를 병합할 xdp 또는 pdf 파일
   * xmlfile:xdp_or_pdf_file과 병합할 xml 데이터 파일
   * saveLocation:렌더링된 문서를 파일 시스템에 저장할 위치입니다.

**데이터를 PDF 파일로 가져오기:**
* FormsService를 사용하여 데이터를 PDF 파일로 가져오기
* **POST URL**  - http://localhost:4502/content/AemFormsSamples/mergedata.html
* **요청 매개 변수:**

   * pdf :데이터를 병합할 pdf 파일
   * xmlfile:pdf 파일과 병합할 xml 데이터 파일
   * saveLocation:렌더링된 문서를 파일 시스템에 저장할 위치입니다. 예: c:\\\outputsample.pdf.

**PDF 파일에서 데이터 내보내기**
* FormsService를 사용하여 PDF 파일에서 데이터 내보내기
* **POST** URL - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **요청 매개 변수:**

   * pdf :데이터를 내보낼 pdf 파일
   * saveLocation:내보낸 데이터를 파일 시스템에 저장할 위치입니다.

[이 Postman 컬렉션을 가져와 API를 테스트할 수 있습니다](assets/document-services-postman-collection.json)

