---
title: AEM Assets에서 Adobe Stock assets 사용
description: AEM은 사용자가 AEM에서 직접 Adobe Stock 에셋을 검색, 미리보기, 저장 및 라이선스를 제공할 수 있습니다. 조직은 이제 Adobe Stock 엔터프라이즈 플랜을 AEM Assets과 통합하여 AEM의 강력한 자산 관리 기능과 함께 라이선스가 있는 자산을 이제 귀사의 광고 및 마케팅 프로젝트에 폭넓게 사용할 수 있도록 할 수 있습니다.
feature: Adobe Stock
version: 6.5
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-06-26T00:00:00Z
doc-type: Feature Video
exl-id: a3c3a01e-97a6-494f-b7a9-22057e91f4eb
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '975'
ht-degree: 3%

---

# AEM Assets에서 Adobe Stock 사용{#using-adobe-stock-assets-with-aem-assets}

AEM 6.4.2는 사용자가 AEM에서 직접 Adobe Stock 에셋을 검색, 미리보기, 저장 및 라이선스할 수 있는 기능을 제공합니다. 조직은 이제 Adobe Stock 엔터프라이즈 플랜을 AEM Assets과 통합하여 AEM의 강력한 자산 관리 기능과 함께 라이선스가 있는 자산을 이제 귀사의 광고 및 마케팅 프로젝트에 폭넓게 사용할 수 있도록 할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/24678?quality=12&learn=on)

>[!NOTE]
>
>통합하려면 [엔터프라이즈 Adobe Stock 플랜](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) 서비스 팩 2 이상이 배포된 AEM 6.4 AEM 6.4 서비스 팩 세부 사항은 다음을 참조하십시오 [릴리스 정보](https://helpx.adobe.com/kr/experience-manager/6-4/release-notes/sp-release-notes.html).

Adobe Stock 및 AEM Assets 통합을 통해 콘텐츠 작성자와 마케터는 크리에이티브 또는 마케팅 목적으로 스톡 자산에 대한 라이센스를 쉽게 부여하고 사용할 수 있습니다. 위치 필터를 Adobe Stock으로 추가하거나 AEM Assets 기본 탐색을 탐색하고 Adobe Stock Coral UI 아이콘 검색을 클릭하여 옴니 검색을 사용하여 Stock 자산 검색을 수행할 수 있습니다.

## 기능

### 검색 및 저장

* AEM 작업 영역을 종료하지 않고 Adobe Stock 에셋 검색을 수행합니다.
* 자산에 라이센스를 주지 않고 미리보기를 위해 Adobe Stock 자산을 저장합니다.
* Adobe Stock 에셋에 라이센스를 부여하고 AEM Assets에 저장하는 기능
* AEM Assets UI 내에서 Adobe Stock에서 유사한 에셋을 검색하는 기능
* Adobe Stock 웹 사이트의 AEM Assets 내에서 Stock Search에서 선택한 에셋 보기
* 라이센스가 부여된 자산 파일은 쉽게 식별할 수 있도록 파란색 라이센스 배지로 표시됩니다

### 자산 메타데이터

* 라이센스가 부여된 자산은 AEM Assets 내에 저장됩니다. 자산 속성에는 별도의 자산 메타데이터 탭 아래에 주식 메타데이터가 포함됩니다
* 자산 메타데이터에 라이선스 참조를 추가하는 기능

### 자산 스톡 프로필

* 사용자는 아래에서 Adobe Stock 프로필을 선택할 수 있습니다. *사용자 > 내 환경 설정 > Stock 구성*
* 자산 라이센스 창에는 필수 및 선택적 참조를 추가할 수 있습니다.
* 지역에 따라 Asset Licensing 창의 언어 기본 설정을 선택할 수 있습니다.

### 필터

* 사용자는 자산 유형, 방향 및 유사 보기에 따라 스톡 자산을 필터링할 수 있습니다
* 에셋 유형에는 사진, 일러스트레이션, 벡터, 비디오, 템플릿, 3D, 프리미엄, 에디토리얼이 포함됩니다.
* 방향에는 가로, 세로 및 사각형이 포함됩니다.
* 유사 필터 보기를 사용하려면 Adobe Stock 파일 번호가 필요합니다.

### 액세스 제어

* 관리자는 Adobe Stock Cloud Service 구성을 설정할 때 Stock 자산에 라이센스를 부여할 수 있는 권한을 특정 사용자/그룹에 제공할 수 있습니다.
* 특정 사용자/그룹에 Stock 자산에 라이센스를 부여할 권한이 없는 경우 *Stock Asset Search / Asset 라이센스* 기능이 비활성화됩니다.

## AEM Assets을 사용하여 Adobe Stock 설정{#set-up-adobe-stock-with-aem-assets}

AEM 6.4.2는 사용자가 AEM에서 직접 Adobe Stock 에셋을 검색, 미리보기, 저장 및 라이선스할 수 있는 기능을 제공합니다. 이 비디오는 Adobe I/O 콘솔을 사용하여 AEM Assets으로 Adobe 재고를 설정하는 방법에 대한 빠른 연습을 다룹니다.

>[!VIDEO](https://video.tv.adobe.com/v/25043?quality=12&learn=on)

>[!NOTE]
>
>Adobe Stock Cloud Service 구성의 경우 프로덕션 환경 및 라이선스 에셋 경로 지점을 선택해야 합니다. `/content/dam`. 이제 환경 필드가 AEM에서 제거되었습니다.

>[!NOTE]
>
>통합하려면 [엔터프라이즈 Adobe Stock 플랜](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) 및 AEM 6.4 이상 [서비스 팩 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=입니다.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aaem%2F6-4&amp;3_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulative-fix&amp;orderby=%40jcr%3Acontent%2Fmetadata%2Fdc%3Atitle&amp;orderby.sort=asc&amp;layout=list&amp;p.offset=0&amp;p.limit=24) 배포되었습니다. AEM 6.4 서비스 팩 세부 사항은 다음을 참조하십시오 [릴리스 정보](https://helpx.adobe.com/kr/experience-manager/6-4/release-notes/sp-release-notes.html). 다음에 대한 관리자 권한도 필요합니다. [Adobe I/O 콘솔](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) 및 Adobe Experience Manager을 사용하여 통합을 설정할 수 있습니다.

### 설치 {#installations}

* AEM 6.4의 경우 [AEM 서비스 팩 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=입니다.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aaem%2F6-4&amp;3_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulative-fix&amp;orderby=%40jcr%3Acontent%2Fmetadata%2Fdc%3Atitle&amp;orderby.sort=asc&amp;layout=list&amp;p.offset=0&amp;p.limit=24) 그런 다음 cq-dam-stock-integration-content-1.0.4.zip 파일을 다시 설치합니다.
* 다음에 대한 관리자 권한이 있는지 확인하십시오. [Adobe I/O 콘솔](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) 및 Adobe Experience Manager을 사용하여 통합을 설정할 수 있습니다.

#### Adobe I/O 콘솔을 사용하여 Adobe IMS 구성 설정 {#set-up-adobe-ims-configuration-using-adobe-i-o-console}

1. 아래에서 Adobe IMS 기술 계정 구성 만들기 **도구 > 보안**
2. 다음 항목 선택 *클라우드 솔루션* 다음으로: *Adobe Stock* 새 인증서를 만들거나 구성에 기존 인증서를 다시 사용합니다.
3. Adobe I/O 콘솔로 이동하여 새 서비스 계정 통합을 만듭니다. *Adobe Stock*.
4. 2단계에서 인증서를 Adobe Stock 서비스 계정 통합에 업로드합니다.
5. 필요한 Adobe Stock 프로필 구성을 선택하고 서비스 통합을 완료합니다.
6. 통합 세부 정보를 사용하여 Adobe IMS 기술 계정 구성을 완료합니다
7. Adobe IMS 기술 계정을 사용하여 액세스 토큰을 받을 수 있는지 확인합니다.

![Adobe IMS 기술 계정](assets/screen_shot_2018-10-22at12219pm.png)

#### Adobe Stock Cloud Service 설정 {#set-up-adobe-stock-cloud-services}

1. 아래에서 Adobe Stock에 대한 새 클라우드 서비스 구성 만들기 **도구 > Cloud Service.**
2. 다음 항목 선택 *Adobe IMS 구성* 의 위 섹션에서 을(를) 만들었습니다. *Adobe Stock Cloud* 구성

3. 다음을 선택하십시오. **환경** 프로덕션으로.
4. **라이센스가 부여된 자산 경로** 아래의 모든 디렉터리를 가리킬 수 있습니다. `/content/dam`.
5. 로케일을 선택하고 설정을 완료합니다.
6. Adobe Stock Cloud Service에 사용자/그룹을 추가하여 특정 사용자 또는 그룹에 대한 액세스를 활성화할 수도 있습니다.

![Adobe 에셋 재고 구성](assets/screen_shot_2018-10-22at12425pm.png)

### 추가 리소스

* [기업 주식 계획](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* [AEM 6.4 서비스 팩 2 릴리스 노트](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/release-notes.html?lang=ko-KR)
* [AEM 및 Adobe Stock 통합](https://experienceleague.adobe.com/docs/experience-manager-65/assets/using/aem-assets-adobe-stock.html)
* [Adobe I/O 콘솔 통합 API](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Adobe Stock API 문서](https://www.adobe.io/apis/creativecloud/stock/docs.html)
