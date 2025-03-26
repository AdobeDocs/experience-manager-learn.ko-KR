---
title: AEM Forms에서 출력 및 Forms 서비스를 사용하여 개발
description: AEM Forms에서 출력 및 Forms 서비스 API를 사용하여 개발하는 방법에 대해 알아봅니다.
feature: Output Service
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2024-01-29T00:00:00Z
exl-id: d268d5d6-f24f-4db9-b8e0-07dd769c6005
duration: 122
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '565'
ht-degree: 0%

---

# AEM Forms에서 출력 및 Forms 서비스를 사용하여 개발{#developing-with-output-and-forms-services-in-aem-forms}

AEM Forms에서 출력 및 Forms 서비스 API를 사용하여 개발하는 방법에 대해 알아봅니다.

이 문서에서는 다음을 살펴봅니다

* [출력 서비스](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) - 일반적으로 이 서비스는 xml 데이터를 xdp 템플릿 또는 pdf와 병합하여 병합된 pdf를 생성하는 데 사용됩니다.
* [FormsService](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/forms/api/FormsService.html) - xdp를 pdf로 렌더링하고 데이터를 PDF 파일로 내보내기/가져올 수 있는 매우 다양한 서비스입니다.


다음 코드 조각은 PDF 파일에서 데이터를 내보냅니다

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

1행에서 요청에서 pdffile 추출

Line2는 요청에서 saveLocation을 추출합니다.

5행에서 FormsService를 가져옵니다.

6행에서는 PDF 파일에서 xmlData를 내보냅니다

**시스템에서 샘플 패키지를 테스트하려면**

[AEM 패키지 관리자를 사용하여 패키지 다운로드 및 설치](assets/using-output-and-form-service-api.zip)




**패키지를 설치하면 Adobe Granite CSRF 필터에 다음 URL이 허용 목록 됩니다.**

1. 허용 목록에 추가하다 위에서 언급 한 경로에 아래의 단계를 따르십시오.
1. [configMgr에 로그인](http://localhost:4502/system/console/configMgr)
1. Adobe Granite CSRF 필터 검색
1. 제외된 섹션에 다음 3개의 경로를 추가하고 저장합니다
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. /content/AemFormsSamples/renderxdp
1. &quot;Sling Referrer filter&quot; 검색
1. &quot;비우기 허용&quot; 확인란을 선택합니다. (이 설정은 테스트 목적으로만 사용해야 함)

## 샘플 테스트

샘플 코드를 테스트하는 방법에는 여러 가지가 있습니다. 가장 빠르고 쉬운 방법은 Postman 앱을 사용하는 것입니다. Postman을 사용하면 서버에 POST 요청을 수행할 수 있습니다.

* 시스템에 Postman 앱을 설치합니다.
* 앱을 실행하고 적절한 URL을 입력합니다
* 드롭다운 목록에서 &quot;POST&quot;를 선택했는지 확인합니다.
* &quot;인증&quot;을 &quot;기본 인증&quot;으로 지정해야 합니다. AEM 서버 사용자 이름 및 암호 지정
* 본문 탭에서 요청 매개 변수 지정
* Send 단추를 클릭합니다.

패키지에는 4개의 샘플이 들어 있습니다. 다음 단락에서는 출력 서비스 또는 Forms 서비스를 사용할 시기, 서비스의 URL, 각 서비스가 기대하는 입력 매개 변수에 대해 설명합니다

## OutputService를 사용하여 데이터를 xdp 템플릿과 병합

* 출력 서비스를 사용하여 데이터를 xdp 또는 pdf 문서와 병합하여 병합된 pdf 생성
* **게시물 URL**: http://localhost:4502/content/AemFormsSamples/outputservice.html
* **요청 매개 변수 -**

   * **xdp_or_pdf_file** : 데이터를 병합할 xdp 또는 pdf 파일
   * **xmlfile**: xdp_or_pdf_file과 병합되는 xml 데이터 파일입니다.
   * **saveLocation**: 렌더링된 문서를 파일 시스템에 저장할 위치입니다. 예: c:\\documents\\sample.pdf

### FormsService API 사용

#### 데이터 가져오기

* FormsService importData를 사용하여 데이터를 PDF 파일로 가져오기
* **게시물 URL** - http://localhost:4502/content/AemFormsSamples/mergedata.html

* **요청 매개 변수:**

   * **pdffile** : 데이터를 병합할 pdf 파일입니다.
   * **xmlfile**: pdf 파일과 병합되는 xml 데이터 파일입니다.
   * **saveLocation**: 렌더링된 문서를 파일 시스템에 저장할 위치입니다. 예 `c:\\outputsample.pdf`.

#### 데이터 내보내기

* FormsService exportData API를 사용하여 PDF 파일에서 데이터 내보내기
* **게시물 URL** - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **요청 매개 변수:**

   * **pdffile** : 데이터를 내보낼 pdf 파일입니다.
   * **saveLocation**: 내보낸 데이터를 파일 시스템에 저장할 위치입니다. 예: c:\\documents\\exported_data.xml

#### XDP 렌더링

* XDP 템플릿을 정적/동적 pdf로 렌더링
* FormsService renderPDFForm API를 사용하여 xdp 템플릿을 PDF으로 렌더링
* **게시물 URL** - http://localhost:4502/content/AemFormsSamples/renderxdp?xdpName=f1040.xdp
* 요청 매개 변수:
   * xdpName: pdf로 렌더링할 xdp 파일의 이름

[이 Postman 컬렉션을 가져와서 API를 테스트할 수 있습니다.](assets/UsingDocumentServicesInAEMForms.postman_collection.json)

