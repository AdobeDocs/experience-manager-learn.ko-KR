---
title: 브랜드 포털 사용
description: AEM 작성자 및 AEM Assets 브랜드 포털 통합의 비디오 개요.
feature: Brand Portal
version: 6.3, 6.4, 6.5
topic: 컨텐츠 관리
role: 비즈니스 전문가
level: 초급
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1777'
ht-degree: 2%

---


# AEM Assets{#using-brand-portal-with-aem-assets}에서 브랜드 포털 사용

Adobe Experience Manager(AEM) Assets 브랜드 포털 통합의 비디오 가이드

## 브랜드 포털 2019년 9월 기능 및 개선 사항

브랜드 포털의 2019년 9월 가장 눈에 띄는 것은 컨텐츠 속도를 높이고 Experience Manager 작성자와 제3자 크리에이티브 및 기여자 간에 자산을 쉽고 빠르게 교환할 수 있도록 해주는 자산 소싱을 소개합니다.

### 브랜드 포털 자산 소싱{#asset-sourcing}

브랜드 포털의 자산 소싱은 제3자 기관 및 팀으로부터 자산을 수집하여 검토 및 사용을 위해 Experience Manager 작성자에게 다시 동기화하기 위해 사용됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/29365/?quality=12&learn=on)

*자산 소싱을 사용하려면 Experience Manager 작성자 6.5 SP2(6.5.2) 이상이 필요합니다.*

Experience Manager 작성자에 대한 자산 소싱을 구성 및 설정하는 방법에 대한 지침은 [자산 소싱에 대한 Experience Manager 작성자 활성화](https://docs.adobe.com/content/help/en/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/configure-asset-sourcing-in-aem/brand-portal-enable-asset-sourcing.html)를 검토하십시오.

## 브랜드 포털 2019년 2월 기능 및 개선 사항{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

브랜드 포털의 2019년 2월 릴리스는 텍스트 검색 및 상위 고객 요청에 대한 개선 사항에 중점을 둡니다.

### 향상된 검색 기능

브랜드 포털은 필터링 창에서 속성 조건자에 대한 부분 텍스트 검색을 통해 검색을 향상시켜 줍니다. 부분 텍스트 검색을 허용하려면 검색 양식에서 속성 조건자에서 부분 검색을 활성화해야 합니다.

부분 텍스트 검색 및 와일드카드 검색에 대해 자세히 알아보려면 읽어 보십시오.

#### 부분 구문 검색

이제 필터링 창에서 검색된 구문 중 단어 또는 2인 부분만 지정하여 자산을 검색할 수 있습니다.

**사용 사례** :부분 구문 검색은 검색어에 나타나는 정확한 단어 조합을 잘 모르는 경우 유용합니다.

예를 들어 브랜드 포털의 검색 양식에서 자산 제목에 대한 부분 검색을 위해 속성 조건자를 사용하는 경우, camp라는 용어를 지정하면 제목 구문에 camp라는 단어가 있는 모든 자산이 반환됩니다.

#### 와일드카드 검색

브랜드 포털에서는 검색 구문의 단어 일부와 함께 검색 쿼리에서 별표(*)를 사용할 수 있습니다.

**사용 사례** :검색된 구문에서 발생하는 정확한 단어를 정확히 알지 못할 경우 와일드카드 검색을 사용하여 검색 쿼리의 간격을 채울 수 있습니다.

예를 들어 scaling*을 지정하면 브랜드 포털의 검색 양식에서 자산 제목에서 부분 검색을 위해 속성 조건자를 사용하는 경우 제목 구문에서 문자를 시작하는 단어가 있는 모든 에셋이 반환됩니다.

마찬가지로 지정:

* \*hide는 제목 구문에 문자가 올라가는 단어가 있는 모든 자산을 반환합니다.
* \*climb\* 제목 구문에서 문자를 구성하는 단어가 있는 모든 자산을 반환합니다.

#### 폴더 계층 구조 사용

이제 관리자는 로그인할 때 관리자가 아닌 사용자(편집기, 뷰어 및 손님 사용자)에게 폴더가 표시되는 방식을 구성할 수 있습니다.
[폴더 계층 ](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) 구성 활성화가 관리 도구 패널의 일반 설정에 추가됩니다. 구성이 다음과 같은 경우:

* 활성화하면 루트 폴더에서 시작하는 폴더 트리가 관리자가 아닌 사용자에게 표시됩니다. 따라서 관리자와 유사한 탐색 경험을 제공할 수 있습니다.
* 비활성화된 경우, 공유 폴더만 랜딩 페이지에 표시됩니다.

[폴더 계층 ](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) 기능 활성화(활성화되면) 다른 계층 구조에서 공유되는 동일한 이름의 폴더를 구별할 수 있습니다. 로그인하면 이제 관리자가 아닌 사용자가 공유 폴더의 가상 상위(및 상위) 폴더를 볼 수 있습니다.

공유 폴더는 가상 폴더의 각 디렉토리 내에 구성됩니다. 이러한 가상 폴더는 잠금 아이콘으로 인식할 수 있습니다.

가상 폴더의 기본 축소판은 첫 번째 공유 폴더의 축소판 이미지입니다.

### Dynamic Media 비디오 변환 지원

AEM 작성자 인스턴스가 Dynamic Media 하이브리드 모드에 있는 사용자는 원본 비디오 파일 외에 다이내믹 미디어 변환을 미리 보고 다운로드할 수 있습니다.

특정 테넌트 계정에서 다이내믹 미디어 표현물을 미리 보고 다운로드할 수 있도록 하려면 관리자는 관리 도구 패널에서 비디오 구성에서 Dynamic Media 구성(비디오 서비스 URL(DM-게이트웨이 URL) 및 등록 ID)을 지정해야 합니다.

Dynamic Media 비디오를 다음과 같이 미리 볼 수 있습니다.

* 자산 세부 사항 페이지
* 자산의 카드 보기
* 링크 공유 미리 보기 페이지

Dynamic Media 비디오 인코딩은 다음 위치에서 다운로드할 수 있습니다.

* 브랜드 포털
* 공유 링크

### 브랜드 포털에 게시 예약

[AEM (6.4.2.0)](https://helpx.adobe.com/experience-manager/6-5/release-notes/sp-release-notes.html#main-pars_header_9658011) Author 인스턴스를 브랜드 포털로 게시 워크플로우는 나중 날짜, 시간으로 예약할 수 있습니다.

마찬가지로 게시된 자산은 브랜드 포털에서 게시 취소 워크플로우를 예약하여 나중 날짜(시간)에 포털에서 제거할 수 있습니다.

### URL에서 구성 가능한 테넌트 별칭

조직은 URL에 대체 접두어가 추가되어 포털 URL을 사용자 정의할 수 있습니다. 기존 포털 URL에서 임차인 이름에 대한 별칭을 얻으려면 조직이 Adobe 지원에 문의해야 합니다.

브랜드 포털 URL의 접두어만 사용자 정의할 수 있으며 전체 URL은 사용자 지정할 수 없습니다.
예를 들어 기존 도메인이 `wknd.brand-portal.adobe.com`인 조직은 요청 시 `wkndinc.brand-portal.adobe.com`을(를) 만들 수 있습니다.

그러나 AEM 작성자 인스턴스는 테넌트 ID를 가진 [구성된](https://helpx.adobe.com/kr/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)만 될 수 있고 임차인 별칭(대체) URL을 포함하지 않습니다.

**사용 사례** :조직은 Adobe에서 제공하는 URL을 고수하는 대신 포털 URL을 사용자 정의하여 브랜딩 요구 사항을 충족할 수 있습니다.

## 브랜드 포털 2018년 12월 기능 및 개선 사항{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707/?quality=9&learn=on)

### 게스트 액세스

AEM 브랜드 포털에서는 게스트가 포털에 액세스할 수 있도록 합니다. 게스트 사용자는 포털에 로그인하는 데 자격 증명이 필요하지 않으며 모든 공개 폴더 및 컬렉션에 액세스하여 다운로드할 수 있습니다. 게스트 사용자는 자신의 라이트박스(비공개 컬렉션)에 에셋을 추가하고 동일한 에셋을 다운로드할 수 있습니다. 관리자가 설정한 스마트 태그 검색 및 검색 조건도 볼 수 있습니다. 게스트 세션은 사용자가 컬렉션과 저장된 검색을 만들거나 추가로 공유하거나 폴더 및 컬렉션 설정에 액세스하고 자산을 링크로 공유할 수 있도록 허용하지 않습니다.

### 가속화된 다운로드

브랜드 포털 사용자는 Aspera 기반의 빠른 다운로드 기능을 활용하여 최대 25배 빠르게 다운로드할 수 있고 전 세계 위치에 관계없이 완벽한 다운로드 경험을 제공할 수 있습니다. 조직에서 다운로드 가속이 활성화되면 브랜드 포털 또는 공유 링크에서 에셋을 보다 빠르게 다운로드하려면 다운로드 대화 상자에서 다운로드 가속 활성화 옵션을 선택해야 합니다.

* [브랜드 포털에서 다운로드 시간을 단축하는 가이드](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Aspera Connect 테스트 서버](https://test-connect.asperasoft.com/)

### 사용자 로그인 보고서

사용자 로그인을 추적하는 새 보고서가 도입되었습니다. 사용자 로그인 보고서는 조직이 위임 관리자 및 브랜드 포털의 다른 사용자를 감사하고 유지할 수 있도록 하는 데 도움이 될 수 있습니다.

보고서 로그에는 각 사용자의 이름, 이메일 ID, 페르소나(관리자, 뷰어, 편집기, 손님), 그룹, 마지막 로그인, 활동 상태 및 로그인 수가 표시됩니다.

### 원본 표현물에 액세스

관리자는 원본 이미지 파일(jpeg, tiff, png, bmp, gif, pjpeg, x-portable-anymap, x-portable-bitmap, x-portable-graymap, x-portable-graymap, x-rgb, x-xbitmap, x-xpixmap, x-icon, image/photoshop, image/x-photoshop, psd, image/vnd.adobe.photoshop)에 대한 사용자 액세스를 제한하고, Photoshop에서 다운로드할 저해상도 브랜드 변환에 대한 액세스를 제공할 수 있습니다. 포털 또는 공유 링크. 이 액세스는 관리 도구 패널의 사용자 역할 페이지의 그룹 탭에서 사용자 그룹 수준에서 제어할 수 있습니다.

### 새 구성

관리자는 특정 테넌트에 대해 다음 기능을 활성화/비활성화하기 위해 6개의 새로운 구성이 추가되었습니다.

* 게스트 액세스 허용
* 사용자가 브랜드 포털에 대한 액세스 권한을 요청할 수 있도록 허용
* 관리자가 브랜드 포털에서 자산을 삭제할 수 있도록 허용
* 공개 컬렉션 만들기 허용
* 공개 스마트 컬렉션 만들기 허용
* 다운로드 가속 허용

### 기타 개선 사항

* *카드 및 목록 보기의 폴더 계층*  경로 — 사용자는 브랜드 포털 인스턴스 내에 저장된 폴더의 위치를 알 수 있습니다. 다른 폴더 계층 구조 내에서 이름이 같은 폴더를 차별화할 수 있습니다.
* *개요 옵션*  — 자산/폴더를 선택한 다음 도구 모음에서 개요 옵션을 선택하여 비관리 사용자에게 자산/폴더에 대한 메타데이터를 제공합니다. 현재 제목, 만든 날짜 및 경로가 표시됩니다.

### Adobe I/O 호스트 UI를 사용하여 oAuth 통합 구성

브랜드 포털은 Adobe I/O [https://legacy-oauth.cloud.adobe.io/](https://legacy-oauth.cloud.adobe.io/) 인터페이스를 사용하여 JWT 애플리케이션을 만듭니다. 이 인터페이스를 사용하면 브랜드 포털과 AEM Assets 통합을 허용하도록 oAuth 통합을 구성할 수 있습니다. 이전에는 OAuth 통합을 구성하기 위한 UI가 `https://marketing.adobe.com/developer/`에서 호스팅되었습니다. 브랜드 포털에 자산 및 컬렉션을 게시하기 위한 AEM Assets과 브랜드 포털의 통합에 대한 자세한 내용은 [브랜드 포털과 AEM Assets 통합 구성](https://helpx.adobe.com/kr/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html)을 참조하십시오.

## 브랜드 포털 2018년 2월 기능 및 개선 사항{#brand-portal-features-and-enhancements-632}

AEM과 Brand Portal을 연계하는 방향으로 향상된 새로운 기능

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

### 탐색 개선 사항

* AEM에 정렬되고 Coral3 UI를 사용하는 업그레이드된 사용자 인터페이스입니다.
* 새로운 Adobe 로고를 통해 관리 툴에 빠르고 손쉽게 액세스할 수 있습니다.
* 오버레이를 통한 제품 탐색
* 하위 폴더에서 상위 폴더로 빠른 탐색.
* 관리 도구 및 컨텐츠로 이동하는 Omniture 옵션
* 카드 보기, 목록 보기 및 열 보기를 사용하면 중첩된 폴더를 손쉽게 검색할 수 있습니다.
* 목록 보기의 자산 정렬이 더 이상 화면에 표시되는 자산 수로 제한되지 않습니다. 폴더의 모든 에셋이 정렬됩니다.

### 검색 개선 사항

* Omniture 기능을 사용하면 브랜드 포털 내에서 에셋과 파일을 신속하게 검색할 수 있습니다.
* Omniture는 특정 폴더 또는 위치 내에서 자산을 검색하는 옵션도 제공합니다
* 검색을 쉽게 하기 위한 자동 키워드 제안
* 추가 필터를 사용하여 Omniture 작업을 향상시킬 수 있습니다. 나중에 검색을 다시 방문할 수 있도록 검색 결과를 Smart Collection에 저장하는 옵션입니다.
* 스마트 태그 자산 검색 지원
* AEM 스마트 태그 자산은 AEM에서 브랜드 포털로 공유되고 브랜드 포털 내에서 자산 검색을 위해 스마트 태그를 사용할 수 있습니다.

### 파일 공유 개선 사항

* 사용자는 링크 공유 옵션을 사용하여 자산을 공유할 수 있습니다.
* 자산을 공유하는 동안 사용자는 각 자산에 대한 만료 날짜를 설정합니다. 사용자에게 공유 에셋에 대한 더 많은 제어 기능을 제공합니다.
* 자산 공유 링크가 있는 외부 사용자는 이미지를 다운로드하고 속성을 볼 수 있습니다.
* 다운로드한 자산 폴더에 대해 원래 중첩 폴더 계층 구조가 유지됩니다.

### 보고 및 관리 기능

* 이제 AEM Assets의 메타데이터 스키마를 AEM에서 브랜드 포털로 게시할 수 있습니다.
* 관리자는 3가지 유형의 보고서(다운로드, 만료 및 게시된 자산)를 만들고 관리할 수 있습니다
* 보고서에 포함되어야 하는 열을 구성하는 기능입니다.
* 브랜드 포털 내에서 자산에 대한 이미지 사전 설정을 만듭니다.
* 추가 필터링 옵션을 포함하도록 관리 검색 레일 양식 또는 Forms 검색 기능을 수정할 수 있습니다.
* 브랜드의 맞춤형 배경 무늬 업데이트 및 미리 보기
* 사용자 수, 사용된 저장소 공간 및 총 자산에 대해 아는 사용 보고서.

## 추가 리소스{#additional-resources}

* [브랜드 포털의 새로운 기능](https://helpx.adobe.com/experience-manager/brand-portal/using/whats-new.html)
* [AEM 작성자 복제 에이전트](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [가속화된 다운로드 안내서](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [AEM Assets 브랜드 포털 Adobe 문서](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html)
* [AEM Assets Dynamic Media Adobe 문서](https://docs.adobe.com/docs/ko/aem/6-3/author/assets/dynamic-media.html)
* [Aspera Connect 다운로드](https://downloads.asperasoft.com/connect2/)
* [Aspera Connect 테스트 서버](https://test-connect.asperasoft.com/)