---
title: AEM Forms을 사용한 Acrobat
seo-title: Acrobat과 적응형 양식 데이터 병합
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 5eeeb197f9a2ee4216e1f9220c830751c36f01ab
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 0%

---


# Acrobat에서 응용 Forms 만들기

조직에는 다양한 양식이 있습니다. 이러한 양식 중 일부는 Microsoft Word에서 작성하여 PDF로 변환됩니다. 이러한 양식은 기본적으로 Adobe Reader 또는 Acrobat을 사용하여 채울 수 없습니다. Acrobat 또는 Reader을 사용하여 이러한 양식을 입력 가능한 양식으로 만들려면 이러한 양식을 Acrobat으로 변환해야 합니다. Acrobat은 Acrobat을 사용하여 만든 양식입니다. 이 문서에서는 Acrobat에서 적응형 양식을 만들고 데이터를 다시 Acrobat으로 병합하여 PDF를 가져오는 방법을 설명합니다. 병합된 데이터가 포함된 PDF를 Adobe Sign을 사용하여 서명을 위해 전송할 수도 있습니다.

>[!NOTE]
>
>AEM Forms 6.5를 사용하는 경우 Automated forms conversion 기능을 사용하십시오.

## 전제 조건

* AEM Forms 6.3 또는 6.4 설치 및 구성
* Adobe Acrobat 이용
* AEM/AEM Forms에 익숙하세요.

### 시스템에서 이 기능을 사용하려면 다음이 필요합니다.

* [Felix Web Console](http://localhost:4502/system/console/bundles)을 사용하여 번들 다운로드 및 배포
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [이 패키지를 다운로드하여 AEM으로 가져옵니다](assets/acro-form-aem-form.zip). 이 패키지에는 Acrobat에서 XSD를 만드는 샘플 작업 과정 및 html 페이지가 포함되어 있습니다
* [configMgr](http://localhost:4502/system/console/configMgr) 열기
   * &#39;Apache Sling Service User Mapper Service&#39;를 검색하고 클릭하여 속성을 엽니다.
   * 다음 서비스 매핑을 추가하려면 `+` 아이콘(더하기)을 클릭합니다
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * &#39;저장&#39;을 클릭합니다.
