---
title: 1장 - 튜토리얼 설정 및 다운로드 - Content Services
description: AEM Headless 자습서의 1장 자습서에 대한 AEM 인스턴스에 대한 기준선 설정입니다.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: f24a75f6-9062-498c-b782-7d9011aa0bcf
duration: 85
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 1%

---

# 튜토리얼 설정

최신 버전의 AEM 및 AEM WCM 핵심 구성 요소를 항상 사용하는 것이 좋습니다.

* AEM 6.5 이상
* AEM WCM 코어 구성 요소 2.4.0 이상
   * 아래 [WKND 모바일 AEM 응용 프로그램 콘텐츠 패키지에 포함됨](#wknd-mobile-application-packages)

이 자습서를 시작하기 전에 다음 AEM 인스턴스가 [설치되어 있고 로컬 컴퓨터에서 실행 중인지 확인](https://helpx.adobe.com/kr/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install)합니다.

* **포트 4502**&#x200B;의 **AEM 작성자**
* **포트 4503**&#x200B;의 **AEM Publish**

## WKND 모바일 애플리케이션 패키지{#wknd-mobile-application-packages}

[!DNL AEM Package Manager]을(를) 사용하여 **AEM 작성자 및 AEM Publish 모두에**&#x200B;다음 AEM 콘텐츠 패키지를 설치합니다.

* [ui.apps: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * AEM WCM 코어 구성 요소용 [!DNL WKND Mobile] 프록시 구성 요소
   * [!DNL WKND Mobile] AEM Content Services 페이지의 CSS(부 스타일링용)
* [ui.content: GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] 사이트 구조
   * [!DNL WKND Mobile] DAM 폴더 구조
   * [!DNL WKND Mobile]개의 이미지 자산

[챕터 7](./chapter-7.md)에서 [Android Studio](https://developer.android.com/studio) 및 제공된 APK(Android 응용 프로그램 패키지)를 사용하여 [!DNL WKND Mobile] Android 모바일 앱을 실행합니다.

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 챕터 AEM 콘텐츠 패키지

이 콘텐츠 패키지 세트는 관련 챕터 및 이전 모든 챕터에 설명된 콘텐츠 및 구성을 만듭니다. 이러한 패키지는 선택 사항이지만 컨텐츠를 신속하게 만들 수 있습니다.

* [2장 내용: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [3장 콘텐츠: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [4장 콘텐츠: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [5장 콘텐츠: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 소스 코드

AEM 프로젝트와 [!DNL Android Mobile App]의 소스 코드는 [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile)에서 사용할 수 있습니다. 이 자습서에서는 소스 코드를 빌드하거나 수정할 필요가 없으며, 이 코드는 자습서의 모든 측면을 빌드하는 방법에 대해 완전히 투명하게 제공됩니다.

튜토리얼이나 코드에서 문제가 발견되면 [GitHub 문제](https://github.com/adobe/aem-guides-wknd-mobile/issues)를 남깁니다.

## 끝으로 건너뛰기

자습서의 끝으로 건너뛰려면 [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 컨텐츠 패키지를 **AEM 작성자 및 AEM Publish 모두에 설치할 수 있습니다.** 콘텐츠 및 구성은 AEM 작성자에 게시된 대로 표시되지 않지만 수동 배포로 인해 [!DNL WKND Mobile App]이(가) 콘텐츠에 액세스할 수 있도록 AEM Publish에서 모든 필수 콘텐츠 및 구성을 사용할 수 있습니다.


## 다음 단계

* [2장 - 이벤트 콘텐츠 조각 모델 정의](./chapter-2.md)
