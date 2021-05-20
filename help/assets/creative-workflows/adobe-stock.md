---
title: AEM Assets에서 Adobe Stock 자산 사용
description: 'AEM은 AEM에서 직접 Adobe Stock 자산을 검색, 미리 보기, 저장 및 라이선스를 제공할 수 있는 기능을 제공합니다. 이제 조직은 Adobe Stock Enterprise 플랜을 AEM Assets과 통합하여 라이선스가 있는 자산이 이제 AEM의 강력한 자산 관리 기능과 함께 광고 및 마케팅 프로젝트에 폭넓게 사용 가능한지 확인할 수 있습니다. '
feature: Adobe Stock
version: 6.4, 6.5
topic: 컨텐츠 관리
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '970'
ht-degree: 6%

---


# AEM Assets에서 Adobe Stock 사용{#using-adobe-stock-assets-with-aem-assets}

AEM 6.4.2는 AEM에서 직접 Adobe Stock 자산을 검색, 미리 보기, 저장 및 라이선스를 제공할 수 있는 기능을 제공합니다. 이제 조직은 Adobe Stock Enterprise 플랜을 AEM Assets과 통합하여 라이선스가 있는 자산이 이제 AEM의 강력한 자산 관리 기능과 함께 광고 및 마케팅 프로젝트에 폭넓게 사용 가능한지 확인할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/24678/?quality=9&learn=on)

>[!NOTE]
>
>통합하려면 [엔터프라이즈 Adobe Stock 계획](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) 및 서비스 팩 2 이상이 배포된 AEM 6.4가 필요합니다. AEM 6.4 서비스 팩 세부 정보에 대해서는 이 [릴리스 노트](https://helpx.adobe.com/kr/experience-manager/6-4/release-notes/sp-release-notes.html)를 참조하십시오.

Adobe Stock과 AEM Assets 통합을 통해 컨텐츠 작성자와 마케터는 크리에이티브 또는 마케팅 목적으로 주식 자산의 라이선스를 쉽게 취득하고 사용할 수 있습니다. 옴니 검색 을 사용하거나, 위치 필터를 Adobe Stock으로 추가하거나, AEM Assets 기본 탐색을 탐색하고 Adobe Stock Coral UI 검색 아이콘을 클릭하여 스톡 자산 검색을 수행할 수 있습니다.

## 기능

### 검색 및 저장

* AEM 작업 영역을 종료하지 않고 Adobe Stock 자산 검색을 수행합니다.
* 자산을 라이선스 없이 미리 볼 수 있도록 Adobe Stock 자산을 저장합니다.
* AEM Assets에 Adobe Stock 자산의 라이선스 및 저장 기능
* AEM Assets UI 내에서 Adobe Stock에서 유사한 자산을 검색하는 기능
* Adobe Stock 웹 사이트의 AEM Assets 내 Stock Search에서 선택한 자산 보기
* 라이선스가 있는 자산 파일은 쉽게 식별할 수 있도록 파란색 라이선스가 있는 배지로 표시됩니다

### 자산 메타데이터

* 라이선스가 있는 자산은 AEM Assets 내에 저장됩니다. 자산 속성은 별도의 자산 메타데이터 탭 아래에 스톡 메타데이터를 포함합니다
* 자산 메타데이터에 라이선스 참조를 추가하는 기능

### Asset Stock 프로필

* 사용자는 *사용자 > 내 환경 설정 > Stock 구성*&#x200B;에서 Adobe Stock 프로필을 선택할 수 있습니다
* 필수 및 선택적 참조를 자산 라이선스 창에 추가할 수 있습니다.
* 지역을 기반으로 자산 라이선스 창에 대한 언어 기본 설정을 선택하는 기능.

### 필터

* 사용자는 자산 유형, 방향 및 유사한 보기에 따라 스톡 자산을 필터링할 수 있습니다
* 자산 유형에는 사진, 그림, 벡터, 비디오, 템플릿, 3D, Premium, 편집 등이 있습니다
* 방향에는 가로, 세로 및 정사각형이 포함됩니다.
* 유사 보기 필터에는 Adobe Stock 파일 번호가 필요합니다.

### 액세스 제어

* 관리자는 Adobe Stock 클라우드 서비스 구성을 설정할 때 특정 사용자/그룹에 스톡 자산의 라이선스를 제공할 수 있는 권한을 제공할 수 있습니다.
* 특정 사용자/그룹에 주식 자산의 라이선스를 제공할 권한이 없는 경우 *주식 자산 검색/자산 라이선스* 기능이 비활성화됩니다.

## AEM Assets{#set-up-adobe-stock-with-aem-assets}으로 Adobe Stock 설정

AEM 6.4.2는 AEM에서 직접 Adobe Stock 자산을 검색, 미리 보기, 저장 및 라이선스를 제공할 수 있는 기능을 제공합니다. 이 비디오에서는 Adobe I/O 콘솔을 사용하여 AEM Assets으로 Adobe 주식을 설정하는 방법에 대한 간단한 연습을 다룹니다.

>[!VIDEO](https://video.tv.adobe.com/v/25043/?quality=12&learn=on)

>[!NOTE]
>
>Adobe Stock 클라우드 서비스 구성의 경우 PROD Environment 및 Licensed 자산 경로 점이 /content/dam을 가리키도록 선택해야 합니다. 환경 필드는 다음 AEM 릴리스에서 제거되고 라이선스가 있는 자산 경로가 예정된 기능의 일부이며 이 필드에 대한 지원은 다음 AEM 릴리스에서 도입됩니다.

>[!NOTE]
>
>통합하려면 [엔터프라이즈 Adobe Stock 계획](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) 및 [서비스 팩 2](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq640/servicepack/AEM-6.4.2.0) 이상이 배포된 AEM 6.4가 필요합니다. AEM 6.4 서비스 팩 세부 정보에 대해서는 이 [릴리스 노트](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html)를 참조하십시오. 또한 통합을 설정하려면 [Adobe I/O 콘솔](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) 및 Adobe Experience Manager에 대한 관리 권한이 필요합니다.

### 설치 {#installations}

* AEM 6.4의 경우 [AEM 서비스 팩 2](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq640/servicepack/AEM-6.4.2.0)를 설치한 다음 cq-dam-stock-integration-content-1.0.4.zip 파일을 다시 설치해야 합니다.
* 통합을 설정하려면 [Adobe I/O 콘솔](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) 및 Adobe Experience Manager에 대한 관리자 권한이 있어야 합니다.

#### Adobe I/O 콘솔 {#set-up-adobe-ims-configuration-using-adobe-i-o-console}을 사용하여 Adobe IMS 구성 설정

1. **도구 > 보안**&#x200B;에서 Adobe IMS 기술 계정 구성을 만듭니다.
2. *클라우드 솔루션* 을 *Adobe Stock*&#x200B;로 선택하고 새 인증서를 만들거나 구성에 대한 기존 인증서를 다시 사용합니다.
3. Adobe I/O 콘솔으로 이동하고 *Adobe Stock*&#x200B;에 대한 새 서비스 계정 통합을 만듭니다.
4. 2단계에서 Adobe Stock 서비스 계정 통합으로 인증서를 업로드합니다.
5. 필요한 Adobe Stock 프로필 구성을 선택하고 서비스 통합을 완료합니다.
6. 통합 세부 사항을 사용하여 IMS 기술 계정 Adobe 구성을 완료합니다
7. Adobe IMS 기술 계정을 사용하여 액세스 토큰을 받을 수 있는지 확인합니다.

![Adobe IMS 기술 계정](assets/screen_shot_2018-10-22at12219pm.png)

#### Adobe Stock Cloud Services {#set-up-adobe-stock-cloud-services} 설정

1. **도구 > Cloud Services에서 Adobe Stock에 대한 새 클라우드 서비스 구성을 만듭니다.**
2. *Adobe Stock Cloud* 구성에 대한 위의 섹션에서 만든 *Adobe IMS 구성*&#x200B;을 선택합니다

3. **ENVIRONMENT**&#x200B;를 PROD로 선택해야 합니다. 스테이징 환경은 지원되지 않으며 AEM의 다음 릴리스에서 제거됩니다.
4. **라이선스가** 있는 자산 경로는 /content/dam 아래의 모든 디렉토리를 가리킬 수 있습니다. 이 필드에 대한 기능 지원이 AEM의 다음 릴리스에서 추가됩니다
5. 로케일을 선택하고 설정을 완료합니다.
6. Adobe Stock 클라우드 서비스에 사용자/그룹을 추가하여 특정 사용자 또는 그룹에 대한 액세스를 활성화할 수도 있습니다.

![Adobe Assets Stock 구성](assets/screen_shot_2018-10-22at12425pm.png)

### 추가 리소스

* [Enterprise Stock 계획](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* [AEM 6.4 서비스 팩 2 릴리스 노트](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html)
* [AEM 및 Adobe Stock 통합](https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html#IntegrateAEMandAdobeStock)
* [Adobe I/O 콘솔 통합 API](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Adobe Stock API 문서](https://www.adobe.io/apis/creativecloud/stock/docs.html)