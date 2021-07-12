---
title: Brand Portal 사용
description: AEM 작성자 및 AEM Assets Brand Portal 통합의 비디오 둘러보기.
feature: Brand Portal
version: 6.3, 6.4, 6.5
topic: 컨텐츠 관리
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '1774'
ht-degree: 2%

---


# AEM Assets에서 Brand Portal 사용{#using-brand-portal-with-aem-assets}

Adobe Experience Manager(AEM) Assets Brand Portal 통합의 비디오 가이드.

## Brand Portal 2019년 9월 기능 및 개선 사항

Brand Portal의 2019년 9월 가장 주목할 만한 것은 자산 소싱이 컨텐츠 속도를 높이고 Experience Manager 작성자와 타사 크리에이티브 및 기여자 간의 자산을 쉽고 빠르게 교환할 수 있도록 하는 것입니다.

### Brand Portal 자산 소싱{#asset-sourcing}

Brand Portal의 자산 소싱은 타사 에이전시 및 팀에서 자산을 수집하여 검토 및 사용하기 위해 Experience Manager 작성자에게 다시 원활하게 동기화하는 데 사용됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/29365/?quality=12&learn=on)

*자산 소싱을 사용하려면 Experience Manager 작성자 6.5 SP2(6.5.2) 이상이 필요합니다*

Experience Manager 작성자에 대한 자산 소싱을 구성 및 설정하는 방법에 대한 지침은 [자산 소싱용 Experience Manager 작성자 활성화](https://docs.adobe.com/content/help/en/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/configure-asset-sourcing-in-aem/brand-portal-enable-asset-sourcing.html)를 검토하십시오.

## Brand Portal 2019년 2월 기능 및 개선 사항{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

Brand Portal의 2019년 2월 릴리스는 텍스트 검색 개선 사항과 상위 고객 요청에 중점을 둡니다.

### 검색 개선 사항

Brand Portal은 필터링 창에서 속성 조건부에서 부분 텍스트 검색을 사용하여 검색을 개선합니다. 부분 텍스트 검색을 허용하려면 검색 양식에서 속성 조건부에서 부분 검색을 활성화해야 합니다.

부분 텍스트 검색 및 와일드카드 검색에 대해 자세히 알아보려면 계속 읽어보십시오.

#### 부분 구문 검색

이제 필터링 창에서 검색한 구문의 부분인 단어 또는 두 개만 지정하여 자산을 검색할 수 있습니다.

**사용 사례** : 부분 구문 검색은 검색한 구문에서 발생하는 단어의 정확한 조합을 잘 모르는 경우 유용합니다.

예를 들어 Brand Portal의 검색 양식에서 자산 제목에서 부분 검색에 속성 설명을 사용하는 경우 camp 라는 용어를 지정하면 제목 구문에 camp 라는 단어가 포함된 모든 자산이 반환됩니다.

#### 와일드카드 검색

Brand Portal에서는 검색 쿼리의 별표(*)를 검색한 구문에 있는 단어의 일부와 함께 사용할 수 있습니다.

**사용 사례** : 검색한 구문에서 발생하는 정확한 단어를 모를 경우 와일드카드 검색을 사용하여 검색 쿼리의 간격을 채울 수 있습니다.

예를 들어 climb* 을 지정하면 Brand Portal의 검색 양식에서 자산 제목에서 부분 검색에 속성 설명을 사용하는 경우 제목 구문에 문자가 오르는 단어가 포함된 모든 자산을 반환합니다.

마찬가지로, 다음을 지정합니다.

* \*climb 은 제목 구문에 문자가 climb으로 끝나는 단어가 포함된 모든 자산을 반환합니다.
* \*climb\* 제목 구문에 climb 문자가 포함된 단어가 있는 모든 자산을 반환합니다.

#### 폴더 계층 구조 사용

이제 관리자는 로그인 시 관리자가 아닌 사용자(편집기, 뷰어 및 게스트 사용자)에게 폴더가 표시되는 방식을 구성할 수 있습니다.
[폴더 계층 구성 ](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) 활성화 는 관리 도구 패널의 일반 설정에 추가됩니다. 구성이 다음과 같은 경우:

* 활성화되어 있으면 루트 폴더에서 시작하는 폴더 트리가 관리자가 아닌 사용자에게 표시됩니다. 따라서 관리자와 유사한 탐색 경험을 제공할 수 있습니다.
* 비활성화하면 공유 폴더만 랜딩 페이지에 표시됩니다.

[폴더 계층 ](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) 기능 활성화(활성화된 경우)를 사용하면 서로 다른 계층 간에 공유되는 동일한 이름의 폴더를 구분할 수 있습니다. 로그인 시 이제 관리자가 아닌 사용자에게 공유 폴더의 가상 상위(및 상위) 폴더가 표시됩니다.

공유 폴더는 가상 폴더의 각 디렉토리 내에 구성됩니다. 잠금 아이콘을 사용하여 이러한 가상 폴더를 인식할 수 있습니다.

가상 폴더의 기본 축소판은 첫 번째 공유 폴더의 축소판 이미지입니다.

### Dynamic Media 비디오 표현물 지원

AEM 작성자 인스턴스가 Dynamic Media 하이브리드 모드에 있는 사용자는 원본 비디오 파일 외에 Dynamic Media 렌디션을 미리 보고 다운로드할 수 있습니다.

특정 테넌트 계정에서 Dynamic Media 렌디션의 미리 보기 및 다운로드를 허용하려면 관리자는 관리 도구 패널에서 비디오 구성에서 Dynamic Media 구성(DM-Gateway URL) 및 등록 ID를 지정하여 다이내믹 비디오를 가져오도록 해야 합니다.

Dynamic Media 비디오를 미리 볼 수 있는 시점:

* 자산 세부 사항 페이지
* 자산의 카드 보기
* 링크 공유 미리 보기 페이지

Dynamic Media 비디오 인코딩은 다음 위치에서 다운로드할 수 있습니다.

* Brand Portal
* 공유 링크

### Brand Portal에 게시 예약됨

[AEM(6.4.2.0)](https://helpx.adobe.com/experience-manager/6-5/release-notes/sp-release-notes.html#main-pars_header_9658011) 작성자 인스턴스를 Brand Portal에 게시하는 자산(및 폴더) 게시 워크플로우는 나중 날짜, 시간 동안 예약할 수 있습니다.

마찬가지로 Brand Portal에서 게시 취소 워크플로우를 예약하면 나중에 게시된 자산을 포털에서 제거할 수 있습니다.

### URL에서 구성 가능한 테넌트 별칭

조직은 URL에 대체 접두사를 포함하여 포털 URL을 사용자 지정할 수 있습니다. 기존 포털 URL에서 테넌트 이름에 대한 별칭을 가져오려면 조직이 Adobe 지원에 문의해야 합니다.

Brand Portal URL의 접두사만 사용자 지정할 수 있으며 전체 URL은 사용자 지정할 수 없습니다.
예를 들어 기존 도메인 `wknd.brand-portal.adobe.com`이 있는 조직은 요청 시 `wkndinc.brand-portal.adobe.com`을 만들 수 있습니다.

그러나 AEM 작성자 인스턴스는 테넌트 ID가 없고 임차인 별칭(대체) URL이 없는 [구성된](https://helpx.adobe.com/kr/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)일 수 있습니다.

**사용 사례** : 조직은 Adobe에서 제공한 URL에 의존하는 대신 포털 URL을 사용자 지정하여 브랜딩 요구 사항을 충족할 수 있습니다.

## Brand Portal 2018년 12월 기능 및 개선 사항{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707/?quality=9&learn=on)

### 게스트 액세스

AEM Brand Portal에서는 게스트가 포털에 액세스할 수 있습니다. 게스트 사용자는 포털에 들어가는 자격 증명이 필요하지 않으며 모든 공용 폴더 및 컬렉션에 액세스하고 다운로드할 수 있습니다. 게스트 사용자는 보유한 라이트 박스(개인 컬렉션)에 자산을 추가하고 이를 다운로드할 수 있습니다. 또한 관리자가 설정한 스마트 태그 검색 및 검색 조건자를 볼 수 있습니다. 게스트 세션에서는 사용자가 컬렉션을 만들고 저장된 검색을 만들거나 추가 검색을 공유하며 폴더 및 컬렉션 설정에 액세스하고 자산을 링크로 공유할 수 없습니다.

### 가속 다운로드

Brand Portal 사용자는 Aspera 기반 빠른 다운로드를 활용하여 최대 25배 더 빠른 속도를 얻을 수 있고 전 세계 위치에 상관없이 원활한 다운로드 경험을 제공할 수 있습니다. Brand Portal 또는 공유 링크에서 자산을 더 빨리 다운로드하려면 조직에서 다운로드 가속화가 활성화되어 있을 경우 다운로드 대화 상자에서 다운로드 가속화 활성화 옵션을 선택해야 합니다.

* [Brand Portal에서 다운로드를 가속화하기 위한 안내서](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Aspera Connect Test Server](https://test-connect.asperasoft.com/)

### 사용자 로그인 보고서

사용자 로그인을 추적하는 새 보고서가 도입되었습니다. 사용자 로그인 보고서는 조직이 Brand Portal의 위임된 관리자 및 기타 사용자를 감사하고 유지할 수 있도록 하는 데 도움이 될 수 있습니다.

보고서 로그에는 각 사용자의 이름, 이메일 ID, 가상(관리자, 뷰어, 편집기, 게스트), 그룹, 마지막 로그인, 활동 상태 및 로그인 수가 표시됩니다.

### 원래 표현물에 대한 액세스

관리자는 원본 이미지 파일(jpeg, tiff, png, bmp, gif, pjpeg, x-portable-anymap, x-portable-graymap, x-portable-graymap, x-portable-graymap, x-rgb, x-xbitmap, x-xpixmap, x-icon, image/photoshop, image/x-photoshop, psd, image/vnd.adobe.photoshop)에 대한 사용자 액세스를 제한하고 Brand Portal 또는 공유 링크에서 다운로드하는 저해상도 표현물에 대한 액세스를 제공할 수 있습니다. 이 액세스는 관리 도구 패널의 사용자 역할 페이지의 그룹 탭에서 사용자 그룹 수준에서 제어할 수 있습니다.

### 새 구성

관리자가 특정 테넌트에서 다음 기능을 활성화/비활성화하도록 6개의 새 구성이 추가되었습니다.

* 게스트 액세스 허용
* 사용자가 Brand Portal에 대한 액세스를 요청할 수 있도록 허용
* 관리자가 Brand Portal에서 자산을 삭제할 수 있도록 허용
* 공개 컬렉션 만들기 허용
* 공개 스마트 컬렉션 만들기 허용
* 다운로드 가속 허용

### 기타 개선 사항

* *카드 및 목록 보기의 폴더 계층*  경로 — 사용자가 Brand Portal 인스턴스 내에 저장된 폴더의 위치를 알 수 있습니다. 다른 폴더 계층 구조 내에서 동일한 이름의 폴더를 구분할 수 있도록 지원합니다.
* *개요 옵션*  — 자산/폴더를 선택한 다음 도구 모음에서 개요 옵션을 선택하여 자산/폴더에 대한 관리자가 아닌 사용자 메타데이터를 제공합니다. 현재 에는 제목, 만든 날짜 및 경로가 표시됩니다

### Adobe I/O 호스트 UI를 사용하여 oAuth 통합 구성

Brand Portal은 Adobe I/O [https://legacy-oauth.cloud.adobe.io/](https://legacy-oauth.cloud.adobe.io/) 인터페이스를 사용하여 Brand Portal과 AEM Assets을 통합할 수 있도록 oAuth 통합을 구성할 수 있는 JWT 애플리케이션을 만듭니다. 이전에는 OAuth 통합 구성을 위한 UI가 `https://marketing.adobe.com/developer/`에 호스팅되었습니다. Brand Portal에 자산 및 컬렉션을 게시하기 위해 AEM Assets과 Brand Portal을 통합하는 방법에 대한 자세한 내용은 [Brand Portal과 AEM Assets 통합 구성](https://helpx.adobe.com/kr/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html)을 참조하십시오.

## Brand Portal 2018년 2월 기능 및 개선 사항{#brand-portal-features-and-enhancements-632}

Brand Portal과 AEM을 정렬하는 데 초점을 맞춘 새로운 기능이 향상되었습니다.

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

### 탐색 개선 사항

* AEM에 정렬되고 Coral3 UI를 사용하는 업그레이드된 사용자 인터페이스입니다.
* 새로운 Adobe 로고를 통해 관리 도구에 빠르고 쉽게 액세스할 수 있습니다.
* 오버레이를 통한 제품 탐색
* 하위 폴더에서 상위 폴더로 빠른 탐색.
* 관리 도구 및 컨텐츠로 이동하는 Omnisearch 옵션입니다.
* 카드 보기, 목록 보기 및 열 보기를 사용하면 중첩된 폴더를 쉽게 탐색할 수 있습니다.
* 목록 보기의 자산 정렬은 더 이상 화면에 표시되는 자산 수로 제한되지 않습니다. 폴더의 모든 자산은 정렬됩니다.

### 검색 개선 사항

* Omnisearch 기능을 사용하면 Brand Portal 내에서 자산 및 파일을 빠르게 검색할 수 있습니다.
* Omnisearch에서는 특정 폴더 또는 위치 내에서 자산을 검색할 수 있는 옵션도 제공합니다
* 검색을 쉽게 하기 위한 자동 키워드 제안
* 추가 필터를 사용하여 Omnisearch를 개선합니다. 나중에 검색을 다시 방문할 수 있도록 검색 결과를 Smart Collection에 저장하는 옵션입니다.
* 스마트 태그가 지정된 자산 검색 지원
* AEM 스마트 태그가 지정된 자산은 AEM에서 Brand Portal으로 공유할 수 있고 Brand Portal 내에서 자산 검색에 스마트 태그를 사용할 수 있습니다.

### 파일 공유 개선 사항

* 사용자는 링크 공유 옵션을 사용하여 자산을 공유할 수 있습니다.
* 자산을 공유하는 동안 사용자는 각 자산에 대한 만료 날짜를 설정하게 됩니다. 사용자에게 공유된 자산에 대한 더 많은 제어 권한을 제공합니다.
* 자산 공유 링크가 있는 외부 사용자는 이미지를 다운로드하고 속성을 볼 수 있습니다.
* 다운로드한 자산 폴더에 대해 원래 중첩된 폴더 계층 구조가 유지됩니다.

### 보고 및 관리 기능

* 이제 AEM Assets의 메타데이터 스키마를 AEM에서 Brand Portal으로 게시할 수 있습니다.
* 관리자는 다운로드, 만료 및 게시된 자산의 세 가지 유형의 보고서를 만들고 관리할 수 있습니다
* 보고서에 포함해야 하는 열을 구성하는 기능.
* Brand Portal 내에서 자산에 대한 이미지 사전 설정 을 만듭니다.
* 추가 필터링 옵션을 포함하도록 관리 검색 레일 양식 또는 Forms 검색 을 수정하는 기능.
* 브랜드에 대한 사용자 정의 배경 무늬 업데이트 및 미리 보기
* 사용자 수, 사용된 저장소 공간 및 총 자산에 대해 알기 위한 사용 보고서

## 추가 리소스{#additional-resources}

* [Brand Portal의 새로운 기능](https://helpx.adobe.com/experience-manager/brand-portal/using/whats-new.html)
* [AEM 작성자 복제 에이전트](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [가속 다운로드 안내서](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [AEM Assets Brand Portal Adobe 문서](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html)
* [AEM Assets Dynamic Media Adobe 문서](https://docs.adobe.com/docs/ko/aem/6-3/author/assets/dynamic-media.html)
* [Aspera Connect 다운로드](https://downloads.asperasoft.com/connect2/)
* [Aspera Connect Test Server](https://test-connect.asperasoft.com/)