---
title: AEM Forms을 사용한 Acrobat
description: Acrobat을 사용하여 적응형 양식을 만들고 데이터를 병합하여 PDF를 가져오는 과정을 단계별로 설명하는 자습서입니다. 병합된 데이터가 있는 PDF를 Adobe Sign을 사용하여 서명을 위해 보낼 수 있습니다.
feature: 적응형 양식
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 2%

---


# Acrobat에서 적응형 Forms 만들기

조직에는 다양한 형태가 있습니다. 이러한 양식 중 일부는 Microsoft Word에서 작성하여 PDF로 변환됩니다. 이러한 양식은 기본적으로 Adobe Reader 또는 Acrobat을 사용하여 채울 수 없습니다. Acrobat 또는 Reader을 사용하여 이러한 양식을 채울 수 있도록 하려면 이러한 양식을 Acroform으로 변환해야 합니다. Acrobat은 Acrobat을 사용하여 만든 양식입니다. 이 문서에서는 Acrobat에서 적응형 양식을 만들고 데이터를 다시 Acrobat으로 병합하여 PDF를 가져오는 과정을 안내합니다. 병합된 데이터가 있는 PDF를 Adobe Sign을 사용하여 서명을 위해 보낼 수도 있습니다.

>[!NOTE]
>
>AEM Forms 6.5를 사용하는 경우 Automated forms conversion 기능을 사용하십시오.

## 전제 조건

* AEM Forms 6.3 또는 6.4 설치 및 구성
* Adobe Acrobat 액세스
* AEM/AEM Forms에 익숙합니다.

### 시스템에서 이 기능을 사용하려면 다음을 수행해야 합니다

* [Felix Web Console](http://localhost:4502/system/console/bundles)을 사용하여 번들을 다운로드하여 배포합니다.
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [이 패키지를 다운로드하여 AEM에 가져옵니다](assets/acro-form-aem-form.zip). 이 패키지에는 양식에 XSD를 만들 수 있는 샘플 워크플로우 및 html 페이지가 포함되어 있습니다
* [configMgr](http://localhost:4502/system/console/configMgr) 열기
   * Apache Sling Service User Mapper Service&#39;를 검색하고 클릭하여 속성을 엽니다.
   * `+` 아이콘(더하기)을 클릭하여 다음 서비스 매핑을 추가합니다
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * &#39;저장&#39;을 클릭합니다
