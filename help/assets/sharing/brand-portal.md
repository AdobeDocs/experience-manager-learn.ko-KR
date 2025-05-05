---
title: Brand Portal 사용
description: AEM Author 및 AEM Assets Brand Portal 통합에 대한 비디오 둘러보기.
feature: Brand Portal
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-06-15T00:00:00Z
doc-type: Feature Video
exl-id: 42f13a19-52bf-413d-a141-63f1f0910dce
duration: 2460
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1702'
ht-degree: 0%

---

# AEM Assets에서 Brand Portal 사용{#using-brand-portal-with-aem-assets}

Adobe Experience Manager(AEM) Assets Brand Portal 통합의 비디오 안내서입니다.

## Brand Portal 2019년 9월 기능 및 개선 사항

Brand Portal의 2019년 9월은 컨텐츠 속도를 높이고 Experience Manager 작성자와 서드파티 크리에이티브 및 기여자 간에 자산을 쉽고 빠르게 교환할 수 있는 Asset Sourcing을 가장 눈에 띄게 도입했습니다.

### Brand Portal 에셋 소싱{#asset-sourcing}

Brand Portal의 에셋 소싱을 사용하여 서드파티 에이전시 및 팀의 에셋을 수집하고 검토 및 사용하기 위해 다시 Experience Manager Author로 원활하게 동기화합니다.

>[!VIDEO](https://video.tv.adobe.com/v/32893?quality=12&learn=on&captions=kor)

*에셋 소싱을 사용하려면 Experience Manager 작성자 6.5 SP2(6.5.2) 이상이 필요합니다*

Experience Manager 작성자에 대한 자산 소싱을 구성하고 설정하는 방법에 대한 지침은 [자산 소싱에 Experience Manager 작성자 사용](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/brand-portal-asset-sourcing.html?lang=ko-KR)을 검토하십시오.

## Brand Portal 2019년 2월 기능 및 개선 사항{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/41132?quality=12&learn=on&captions=kor)

Brand Portal의 2019년 2월 릴리스는 텍스트 검색 및 주요 고객 요청에 대한 개선 사항에 중점을 둡니다.

### 검색 개선 사항

Brand Portal은 필터링 창의 속성 술어에 대한 부분 텍스트 검색을 통해 검색을 개선합니다. 부분 텍스트 검색을 허용하려면 검색 양식의 속성 조건자에서 부분 검색을 활성화해야 합니다.

부분 텍스트 검색 및 와일드카드 검색에 대한 자세한 내용은 계속 읽으십시오.

#### 부분 구문 검색

이제 필터링 창에서 검색한 구문의 한 부분(한 단어 또는 두 단어)만 지정하여 자산을 검색할 수 있습니다.

**사용 사례** : 부분 구문 검색은 검색된 구문에서 발생하는 정확한 단어 조합을 모를 때 유용합니다.

예를 들어, Brand Portal의 검색 양식에서 자산 제목의 부분 검색을 위해 속성 조건자 를 사용하는 경우 camp라는 용어를 지정하면 제목 구문에 camp라는 단어가 포함된 모든 자산이 반환됩니다.

#### 와일드카드 검색

Brand Portal을 사용하면 검색 쿼리에서 검색 구문에 있는 단어의 일부와 함께 별표(*)를 사용할 수 있습니다.

**사용 사례**: 검색된 구문에서 발생하는 정확한 단어를 모를 경우 와일드카드 검색을 사용하여 검색 쿼리의 간격을 채울 수 있습니다.

예를 들어, Brand Portal의 검색 양식에서 에셋 제목의 부분 검색에 대해 속성 조건자를 사용하는 경우 climb*을 지정하면 제목 구문에 climb 문자로 시작하는 단어가 포함된 모든 에셋이 반환됩니다.

마찬가지로, 다음을 지정합니다.

* \*climb 은 문자가 오르는 단어가 포함된 모든 에셋을 제목 구문에 반환합니다.
* \*climb\*은 제목 구문에 climb 문자를 구성하는 단어가 포함된 모든 에셋을 반환합니다.

#### 폴더 계층 구조 사용

이제 관리자는 로그인 시 관리자가 아닌 사용자(편집자, 뷰어 및 게스트 사용자)에게 폴더를 표시하는 방법을 구성할 수 있습니다.
[폴더 계층 구조 사용](https://helpx.adobe.com/kr/experience-manager/brand-portal/using/brand-portal-general-configuration.html) 구성이 관리 도구 패널의 일반 설정에 추가되었습니다. 구성이 다음과 같은 경우

* 활성화하면 루트 폴더에서 시작하는 폴더 트리가 관리자가 아닌 사용자에게 표시됩니다. 따라서 관리자에게 관리자와 유사한 탐색 경험을 제공할 수 있습니다.
* 비활성화되어 랜딩 페이지에 공유 폴더만 표시됩니다.

[폴더 계층 구조 사용](https://helpx.adobe.com/kr/experience-manager/brand-portal/using/brand-portal-general-configuration.html) 기능(활성화된 경우)을 사용하면 서로 다른 계층에서 공유되는 이름이 같은 폴더를 구분할 수 있습니다. 이제 로그인하면 관리자가 아닌 사용자에게 공유 폴더의 가상 상위(및 상위) 폴더가 표시됩니다.

공유 폴더는 가상 폴더의 각 디렉토리 내에 구성됩니다. 잠금 아이콘으로 이러한 가상 폴더를 인식할 수 있습니다.

가상 폴더의 기본 썸네일은 첫 번째 공유 폴더의 썸네일 이미지입니다.

### Dynamic Media 비디오 렌디션 지원

AEM 작성자 인스턴스가 Dynamic Media 하이브리드 모드에 있는 사용자는 원본 비디오 파일 외에 Dynamic Media 렌디션을 미리 보고 다운로드할 수 있습니다.

특정 테넌트 계정에서 Dynamic Media 렌디션을 미리 보고 다운로드할 수 있도록 하려면 관리자는 관리 도구 패널의 비디오 구성에서 Dynamic Media 구성(비디오 서비스 URL(DM-게이트웨이 URL) 및 Dynamic Video를 가져오기 위한 등록 ID)을 지정해야 합니다.

Dynamic Media 비디오는 다음 위치에서 미리 볼 수 있습니다.

* 자산 세부 사항 페이지
* 에셋의 카드 보기
* 링크 공유 미리 보기 페이지

Dynamic Media 비디오 인코딩은 다음 위치에서 다운로드할 수 있습니다.

* Brand Portal
* 공유 링크

### Brand Portal에 예약된 게시

[AEM(6.4.2.0)](https://helpx.adobe.com/kr/experience-manager/6-5/release-notes/sp-release-notes.html#main-pars_header_9658011) 작성자 인스턴스에서 Brand Portal으로 Assets(및 폴더) 게시 워크플로우를 나중에 예약할 수 있습니다.

마찬가지로 Brand Portal에서 게시 취소 워크플로우를 예약하여 게시된 자산을 나중에 포털에서 제거할 수 있습니다.

### URL에서 구성 가능한 테넌트 별칭

조직은 URL에 대체 접두사를 사용하여 포털 URL을 사용자 정의할 수 있습니다. 기존 포털 URL에서 테넌트 이름에 대한 별칭을 얻으려면 조직에서 Adobe 지원에 문의해야 합니다.

Brand Portal URL의 접두사만 사용자 정의할 수 있으며 전체 URL은 사용자 정의할 수 없습니다.
예를 들어 기존 도메인이 `wknd.brand-portal.adobe.com`인 조직은 요청 시 `wkndinc.brand-portal.adobe.com`을(를) 만들 수 있습니다.

그러나 AEM 작성자 인스턴스는 테넌트 별칭(대체) URL이 아닌 테넌트 ID URL로만 [구성](https://helpx.adobe.com/kr/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)될 수 있습니다.

**사용 사례** : 조직은 Adobe에서 제공한 URL을 고수하는 대신 포털 URL을 사용자 지정하여 브랜딩 요구 사항을 충족할 수 있습니다.

## Brand Portal 2018년 12월 기능 및 개선 사항{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707?quality=12&learn=on)

### 게스트 액세스

AEM Brand Portal에서 게스트가 포털에 액세스할 수 있습니다. 게스트 사용자는 포털에 로그인하기 위해 자격 증명이 필요하지 않으며 모든 공개 폴더 및 컬렉션에 액세스하고 다운로드할 수 있습니다. 게스트 사용자는 자신의 light-box(개인 컬렉션)에 에셋을 추가하고 다운로드할 수 있습니다. 스마트 태그 검색과 관리자가 설정한 검색 술어를 볼 수도 있습니다. 게스트 세션에서는 사용자가 컬렉션과 저장된 검색을 만들거나 더 이상 공유하고, 폴더 및 컬렉션 설정에 액세스하며, 자산을 링크로 공유할 수 없습니다.

### 가속화된 다운로드

Brand Portal 사용자는 Aspera 기반의 빠른 다운로드를 활용하여 최대 25배 더 빠르게 속도를 높이고 전 세계 위치에 상관없이 원활한 다운로드 경험을 누릴 수 있습니다. 조직에서 다운로드 가속화가 활성화된 경우 Brand Portal 또는 공유 링크에서 에셋을 더 빨리 다운로드하려면 다운로드 대화 상자에서 다운로드 가속화 활성화 옵션을 선택해야 합니다.

* [Brand Portal에서 다운로드를 가속화하는 지침](https://helpx.adobe.com/kr/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Aspera 연결 테스트 서버](https://test-connect.asperasoft.com/)

### 사용자 로그인 보고서

사용자 로그인을 추적하는 새 보고서가 도입되었습니다. 사용자 로그인 보고서는 조직에서 위임된 관리자 및 Brand Portal의 다른 사용자를 감사하고 확인할 수 있도록 하는 데 유용합니다.

보고서 로그에는 각 사용자의 이름, 이메일 ID, 가상 사용자(관리자, 뷰어, 편집기, 게스트), 그룹, 마지막 로그인, 활동 상태 및 로그인 수가 표시됩니다.

### 원본 렌디션에 액세스

관리자는 원본 이미지 파일(jpeg, tiff, png, bmp, gif, pjpeg, x-portable-anymap, x-portable-bitmap, x-portable-graymap, x-portable-pixmap, x-rgb, x-xbitmap, x-xpixmap, x-icon, image/photoshop, image/x-photoshop, psd, image/vnd.adobe.photoshop)에 대한 사용자 액세스를 제한하고 Brand Portal 또는 공유 링크에서 다운로드하는 저해상도 표현물에 대한 액세스를 제공할 수 있습니다. 이 액세스는 관리자 도구 패널의 사용자 역할 페이지에 있는 그룹 탭에서 사용자 그룹 수준에서 제어할 수 있습니다.

### 새 구성

관리자가 특정 테넌트에서 다음 기능을 활성화/비활성화할 수 있도록 6개의 새 구성이 추가되었습니다.

* 게스트 액세스 허용
* 사용자가 Brand Portal에 대한 액세스 권한을 요청하도록 허용
* 관리자가 Brand Portal에서 에셋을 삭제하도록 허용
* 공용 컬렉션 생성 허용
* 공용 스마트 컬렉션 만들기 허용
* 다운로드 가속화 허용

### 기타 개선 사항

* *카드 및 목록 보기의 폴더 계층 구조 경로* — 사용자가 Brand Portal 인스턴스 내에 저장된 폴더의 위치를 알 수 있습니다. 사용자가 서로 다른 폴더 계층 구조 내에서 이름이 같은 폴더를 구분할 수 있도록 지원합니다.
* *개요 옵션* - 관리자가 아닌 사용자가 에셋/폴더를 선택한 다음 도구 모음에서 개요 옵션을 선택하여 에셋/폴더에 대한 메타데이터를 제공합니다. 현재 에는 제목, 생성 날짜 및 경로가 표시됩니다

### Adobe I/O이 oAuth 통합 구성을 위한 UI를 호스팅함

Brand Portal은 Adobe I/O [https://legacy-oauth.cloud.adobe.io/](https://legacy-oauth.cloud.adobe.io/) 인터페이스를 사용하여 JWT 애플리케이션을 만듭니다. 이렇게 하면 Brand Portal과의 AEM Assets 통합을 허용하도록 oAuth 통합을 구성할 수 있습니다. 이전에는 OAuth 통합 구성을 위한 UI가 `https://marketing.adobe.com/developer/`에서 호스팅되었습니다. AEM Assets과 Brand Portal을 통합하여 Brand Portal에 자산 및 컬렉션을 게시하는 방법에 대한 자세한 내용은 [Brand Portal과 AEM Assets 통합 구성](https://helpx.adobe.com/kr/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html)을 참조하세요.

## Brand Portal 2018년 2월 기능 및 개선 사항{#brand-portal-features-and-enhancements-632}

Brand Portal과 AEM의 정렬을 지향하는 향상된 기능을 제공합니다.

>[!VIDEO](https://video.tv.adobe.com/v/41132?quality=12&learn=on&captions=kor)

### 탐색 개선 사항

* AEM에 맞게 조정되고 Coral3 UI를 사용하는 업그레이드된 사용자 인터페이스.
* 새로운 Adobe 로고를 통해 관리 도구에 빠르고 쉽게 액세스할 수 있습니다.
* 오버레이를 통한 제품 탐색
* 하위 폴더에서 상위 폴더로 빠른 탐색.
* 관리 도구 및 콘텐츠로 이동하는 Omnisearch 옵션입니다.
* 카드 보기, 목록 보기 및 열 보기를 사용하면 중첩된 폴더를 쉽게 탐색할 수 있습니다.
* 더 이상 목록 보기의 자산 정렬이 화면에 표시되는 자산 수로 제한되지 않습니다. 폴더의 모든 에셋이 정렬됩니다.

### 검색 개선 사항

* Omnisearch 기능을 사용하면 Brand Portal 내에서 에셋 및 파일을 빠르게 검색할 수 있습니다.
* Omnisearch는 특정 폴더 또는 위치 내에서 에셋을 검색하는 옵션도 제공합니다
* 검색을 쉽게 하기 위한 자동 키워드 제안
* 추가 필터를 사용하여 Omnisearch를 개선합니다. 검색 결과를 Smart Collection에 저장하여 나중에 다시 검색할 수 있도록 하는 옵션입니다.
* 스마트 태그가 지정된 에셋 검색 지원
* AEM 스마트 태그가 지정된 에셋은 AEM에서 Brand Portal으로 공유할 수 있으며 Brand Portal 내의 에셋 검색에 스마트 태그를 사용할 수 있습니다.

### 파일 공유 개선 사항

* 사용자는 링크 공유 옵션을 사용하여 에셋을 공유할 수 있습니다.
* 에셋을 공유하는 동안 사용자는 각 에셋에 대한 만료 날짜를 설정하게 됩니다. 사용자가 공유된 에셋을 보다 세밀하게 제어할 수 있도록 합니다.
* 자산 공유 링크가 있는 외부 사용자는 이미지를 다운로드하고 속성을 볼 수 있습니다.
* 다운로드한 에셋 폴더에 대해 원래 중첩 폴더 계층 구조가 유지됩니다.

### 보고 및 관리 기능

* 이제 AEM Assets의 메타데이터 스키마를 AEM에서 Brand Portal으로 게시할 수 있습니다.
* 관리자는 다운로드, 만료 및 게시된 자산의 세 가지 유형의 보고서를 만들고 관리할 수 있습니다
* 보고서에 포함해야 하는 열을 구성하는 기능.
* Brand Portal 내에서 에셋에 대한 이미지 사전 설정을 만듭니다.
* 추가 필터링 옵션을 포함하도록 관리자 검색 레일 양식 또는 Forms 검색 을 수정하는 기능입니다.
* 브랜드의 사용자 정의 배경 무늬 업데이트 및 미리 보기
* 사용자 수, 사용한 저장소 공간 및 총 에셋에 대해 알 수 있는 사용 보고서.

## 추가 리소스{#additional-resources}

* [Brand Portal의 새로운 기능](https://helpx.adobe.com/kr/experience-manager/brand-portal/using/whats-new.html)
* [AEM 작성자 복제 에이전트](https://helpx.adobe.com/kr/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [가속화된 다운로드 안내서](https://helpx.adobe.com/kr/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [AEM Assets Brand Portal Adobe 문서](https://helpx.adobe.com/kr/experience-manager/brand-portal/using/brand-portal.html)
* [AEM Assets Dynamic Media Adobe 문서](https://experienceleague.adobe.com/docs/?lang=ko)
* [Aspera 연결 다운로드](https://downloads.asperasoft.com/connect2/)
* [Aspera 연결 테스트 서버](https://test-connect.asperasoft.com/)
