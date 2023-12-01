---
title: 1장 - 튜토리얼 설정 및 다운로드 - Content Services
seo-title: Getting Started with AEM Content Services - Chapter 1 -  Tutorial Set up
description: AEM Headless 자습서의 1장 자습서에 대한 AEM 인스턴스에 대한 기준선 설정입니다.
seo-description: Chapter 1 of the AEM Headless tutorial the baseline setup for the AEM instance for the tutorial.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: f24a75f6-9062-498c-b782-7d9011aa0bcf
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 1%

---

# 튜토리얼 설정

최신 버전의 AEM 및 AEM WCM 핵심 구성 요소를 항상 사용하는 것이 좋습니다.

* AEM 6.5 이상
* AEM WCM 코어 구성 요소 2.4.0 이상
   * 에 포함됨 [아래 WKND Mobile AEM 애플리케이션 컨텐츠 패키지](#wknd-mobile-application-packages)

이 자습서를 시작하기 전에 다음 AEM 인스턴스가 [로컬 컴퓨터에 설치 및 실행 중](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install):

* **AEM 작성자** 날짜 **포트 4502**
* **AEM 게시** 날짜 **포트 4503**

## WKND 모바일 애플리케이션 패키지{#wknd-mobile-application-packages}

다음 AEM 컨텐츠 패키지 설치 **모두** AEM Author 및 AEM Publish, 사용 [!DNL AEM Package Manager].

* [ui.apps: GitHub > 자산 > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] AEM WCM 코어 구성 요소에 대한 프록시 구성 요소
   * [!DNL WKND Mobile] AEM Content Services 페이지의 CSS(작은 스타일링용)
* [ui.content: GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] 사이트 구조
   * [!DNL WKND Mobile] DAM 폴더 구조
   * [!DNL WKND Mobile] 이미지 에셋

위치 [7장](./chapter-7.md) 다음을 실행합니다. [!DNL WKND Mobile] Android 모바일 앱 사용 [Android 스튜디오](https://developer.android.com/studio) 및 제공된 APK (Android 애플리케이션 패키지):

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 챕터 AEM 콘텐츠 패키지

이 콘텐츠 패키지 세트는 관련 챕터 및 이전 모든 챕터에 설명된 콘텐츠 및 구성을 만듭니다. 이러한 패키지는 선택 사항이지만 컨텐츠를 신속하게 만들 수 있습니다.

* [2장 컨텐츠: GitHub > 자산 > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [3장 컨텐츠: GitHub > 자산 > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [4장 컨텐츠: GitHub > 자산 > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [5장 컨텐츠: GitHub > 자산 > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 소스 코드

AEM 프로젝트 및 의 소스 코드 [!DNL Android Mobile App] 다음에서 사용할 수 있습니다. [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile). 이 자습서에서는 소스 코드를 빌드하거나 수정할 필요가 없으며, 이 코드는 자습서의 모든 측면을 빌드하는 방법에 대해 완전히 투명하게 제공됩니다.

자습서나 코드에서 문제를 발견하면 [GitHub 문제](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## 끝으로 건너뛰기

자습서의 끝으로 이동하려면 [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 콘텐츠 패키지를 설치할 수 있는 위치 **모두** AEM 작성자 및 AEM 게시. 컨텐츠 및 구성은 AEM Author에 게시된 것으로 표시되지 않지만 수동 배포로 인해 AEM Publish에서 필요한 모든 컨텐츠 및 구성을 사용할 수 있으므로 다음을 수행할 수 있습니다. [!DNL WKND Mobile App] 콘텐츠에 액세스합니다.


## 다음 단계

* [2장 - 이벤트 콘텐츠 조각 모델 정의](./chapter-2.md)
