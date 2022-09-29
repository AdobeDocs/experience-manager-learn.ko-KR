---
title: 1장 - 자습서 설정 및 다운로드 - 컨텐츠 서비스
seo-title: Getting Started with AEM Content Services - Chapter 1 -  Tutorial Set up
description: AEM Headless 자습서의 1장에서는 자습서에 대한 AEM 인스턴스에 대한 기준선 설정을 설명합니다.
seo-description: Chapter 1 of the AEM Headless tutorial the baseline setup for the AEM instance for the tutorial.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: f24a75f6-9062-498c-b782-7d9011aa0bcf
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 1%

---

# 자습서 설정

AEM 및 AEM WCM 코어 구성 요소의 최신 버전은 항상 권장됩니다.

* AEM 6.5 이상
* AEM WCM Core Components 2.4.0 이상
   * 에 포함 [아래의 WKND Mobile AEM 애플리케이션 컨텐츠 패키지](#wknd-mobile-application-packages)

이 자습서를 시작하기 전에 다음 AEM 인스턴스가 [로컬 시스템에서 설치 및 실행](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install):

* **AEM 작성자** on **포트 4502**
* **AEM 게시** on **포트 4503**

## WKND 모바일 애플리케이션 패키지{#wknd-mobile-application-packages}

다음 AEM 컨텐츠 패키지 설치 **둘 다** AEM 작성자 및 AEM 게시, 사용 [!DNL AEM Package Manager].

* [ui.apps: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] AEM WCM 코어 구성 요소용 프록시 구성 요소
   * [!DNL WKND Mobile] AEM Content Services 페이지의 CSS(부 스타일 지정)
* [ui.content: GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] 사이트 구조
   * [!DNL WKND Mobile] DAM 폴더 구조
   * [!DNL WKND Mobile] 이미지 자산

in [7장](./chapter-7.md) 우리는 [!DNL WKND Mobile] 다음을 사용하는 Android 모바일 앱 [Android Studio](https://developer.android.com/studio) 및 제공된 APK(Android 애플리케이션 패키지):

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## AEM 컨텐츠 패키지 장

이 컨텐츠 패키지 세트는 관련 장 및 모든 이전 장에 설명된 컨텐츠 및 구성을 만듭니다. 이러한 패키지는 선택 사항이지만 신속한 컨텐츠 생성을 수행할 수 있습니다.

* [제2장 내용 GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [제3장 내용 GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [제4장 내용: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [제5장 내용: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 소스 코드

AEM 프로젝트와 프로젝트 모두에 대한 소스 코드입니다 [!DNL Android Mobile App] 다음 위치에서 사용 가능 [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile). 이 자습서에서는 모든 자습서를 만드는 방법을 완전히 투명할 수 있도록 소스 코드를 빌드하거나 수정할 필요가 없습니다.

자습서나 코드에 문제가 있는 경우 [GitHub 문제](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## 끝으로 이동

자습서의 끝으로 건너뛰려면 [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 컨텐츠 패키지는 **둘 다** AEM 작성자 및 AEM 게시. 컨텐츠 및 구성이 AEM 작성자에 게시됨으로 표시되지 않지만, 수동 배포로 인해 모든 필수 컨텐츠 및 구성을 AEM Publish에서 사용할 수 있으므로 [!DNL WKND Mobile App] 을 눌러 콘텐츠에 액세스합니다.


## 다음 단계

* [2장 - 이벤트 컨텐츠 조각 모델 정의](./chapter-2.md)
