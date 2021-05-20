---
title: 1장 - 자습서 설정 및 다운로드 - 컨텐츠 서비스
seo-title: AEM 컨텐츠 서비스 시작하기 - 1장 - 자습서 설정
description: AEM Headless 자습서의 1장에서는 자습서에 대한 AEM 인스턴스에 대한 기준선 설정을 설명합니다.
seo-description: AEM Headless 자습서의 1장에서는 자습서에 대한 AEM 인스턴스에 대한 기준선 설정을 설명합니다.
feature: 컨텐츠 조각, API
topic: 헤드리스, 컨텐츠 관리
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '444'
ht-degree: 0%

---


# 자습서 설정

AEM 및 AEM WCM 코어 구성 요소의 최신 버전은 항상 권장됩니다.

* AEM 6.5 이상
* AEM WCM Core Components 2.4.0 이상
   * 아래의 [WKND Mobile AEM 애플리케이션 컨텐츠 패키지에 포함됨](#wknd-mobile-application-packages)

이 자습서를 시작하기 전에 다음 AEM 인스턴스가 [로컬 컴퓨터에 설치되어 실행 중인지 확인하십시오](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install).

* **AEM** Authoron  **포트 4502**
* **AEM** Publishon  **포트 4503**

## WKND 모바일 애플리케이션 패키지{#wknd-mobile-application-packages}

**AEM Author와 AEM Publish 모두에 [!DNL AEM Package Manager]를 사용하여 다음 AEM 컨텐츠 패키지를 설치합니다.**

* [ui.apps:GitHub > Assets > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] AEM WCM 코어 구성 요소용 프록시 구성 요소
   * [!DNL WKND Mobile] AEM Content Services 페이지의 CSS(부 스타일 지정)
* [ui.content:GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] 사이트 구조
   * [!DNL WKND Mobile] DAM 폴더 구조
   * [!DNL WKND Mobile] 이미지 자산

[Chapter 7](./chapter-7.md)에서는 [Android Studio](https://developer.android.com/studio) 및 제공된 APK(Android 애플리케이션 패키지)를 사용하여 [!DNL WKND Mobile] Android 모바일 앱을 실행합니다.

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## AEM 컨텐츠 패키지 장

이 컨텐츠 패키지 세트는 관련 장 및 모든 이전 장에 설명된 컨텐츠 및 구성을 만듭니다. 이러한 패키지는 선택 사항이지만 신속한 컨텐츠 생성을 수행할 수 있습니다.

* [제2장 내용GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [제3장 내용GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [제4장 내용:GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [제5장 내용:GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 소스 코드

AEM 프로젝트와 [!DNL Android Mobile App] 모두에 대한 소스 코드는 [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile)에서 사용할 수 있습니다. 이 자습서에서는 모든 자습서를 만드는 방법을 완전히 투명할 수 있도록 소스 코드를 빌드하거나 수정할 필요가 없습니다.

자습서나 코드에 문제가 있으면 [GitHub 문제](https://github.com/adobe/aem-guides-wknd-mobile/issues)를 그대로 두십시오.

## 끝으로 이동

자습서의 끝 부분으로 건너뛰려면 [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 컨텐츠 패키지를 **AEM 작성자 및 AEM 게시 모두에 설치할 수 있습니다.** 컨텐츠 및 구성은 AEM 작성자에 게시됨으로 표시되지 않지만, 수동 배포로 인해 [!DNL WKND Mobile App] 이 컨텐츠에 액세스할 수 있는 AEM 게시에 모든 필수 컨텐츠 및 구성을 사용할 수 있습니다.


## 다음 단계

* [2장 - 이벤트 컨텐츠 조각 모델 정의](./chapter-2.md)
