---
title: AEM Forms이 포함된 Acroforms
description: Acroform을 사용하여 적응형 양식을 만들고 데이터를 병합하여 PDF을 얻는 과정을 단계별로 설명하는 자습서입니다. 그런 다음 병합된 데이터가 있는 PDF을 Acrobat Sign을 사용하여 서명하도록 전송할 수 있습니다.
feature: adaptive-forms
doc-type: Tutorial
version: Experience Manager 6.5
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 45
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 0%

---


# Acroforms에서 적응형 Forms 만들기

조직에는 매우 다양한 형태가 있습니다. 이러한 양식 중 일부는 Microsoft Word에서 만들어져 PDF으로 변환됩니다. 이러한 양식은 기본적으로 Adobe Reader 또는 Acrobat을 사용하여 채울 수 없습니다. Acrobat 또는 Reader을 사용하여 이러한 양식을 채울 수 있도록 하려면 이러한 양식을 Acroform으로 변환해야 합니다. Acroforms는 Acrobat을 사용하여 만든 양식입니다. 이 문서에서는 Acroform에서 적응형 양식을 만들고 데이터를 Acroform으로 다시 병합하여 PDF을 가져오는 과정을 안내합니다. 병합된 데이터가 있는 PDF을 Acrobat Sign을 사용한 서명을 위해 전송할 수도 있습니다.

>[!NOTE]
>
>AEM Forms 6.5를 사용 중인 경우 자동 양식 전환 기능을 사용하십시오.

## 사전 요구 사항

* AEM Forms 6.3 또는 6.4 설치 및 구성
* Adobe Acrobat 액세스
* AEM/AEM Forms에 익숙합니다.

### 시스템에서 이 기능을 사용하려면 다음 항목이 필요합니다

* [Felix 웹 콘솔](http://localhost:4502/system/console/bundles)을 사용하여 번들 다운로드 및 배포
* [문서 서비스 번들](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [이 패키지를 다운로드하여 AEM으로 가져오기](assets/acro-form-aem-form.zip). 이 패키지에는 Acroform에서 XSD를 만들 수 있는 샘플 워크플로우 및 html 페이지가 포함되어 있습니다
* [configMgr](http://localhost:4502/system/console/configMgr) 열기
   * &#39;Apache Sling 서비스 사용자 매퍼 서비스&#39;를 검색한 다음 를 클릭하여 속성을 엽니다.
   * `+` 아이콘(더하기)을 클릭하여 다음 서비스 매핑을 추가합니다
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * &#39;저장&#39; 클릭
