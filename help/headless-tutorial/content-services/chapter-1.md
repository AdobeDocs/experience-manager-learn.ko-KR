---
title: 1장 - 자습서 설정 및 다운로드 - 컨텐트 서비스
seo-title: AEM Content Services 시작하기 - 1장 - 자습서 설정
description: AEM 헤드리스 자습서의 1장에서는 자습서에 대한 AEM 인스턴스의 기준선 설정을 설명합니다.
seo-description: AEM 헤드리스 자습서의 1장에서는 자습서에 대한 AEM 인스턴스의 기준선 설정을 설명합니다.
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 0%

---


# 자습서 설정

AEM 및 AEM WCM 핵심 구성 요소의 최신 버전이 항상 권장됩니다.

* AEM 6.5 이상
* AEM WCM 핵심 구성 요소 2.4.0 이상
   * [WKND Mobile AEM 응용 프로그램 콘텐츠 패키지에 포함됨](#wknd-mobile-application-packages)

이 자습서를 시작하기 전에 다음 AEM 인스턴스가 로컬 컴퓨터에 설치되어 실행되고 있는지 확인합니다](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install).[

* **AEM** Authoron  **포트 4502**
* **AEM** Publishon  **포트 4503**

## WKND 모바일 응용 프로그램 패키지{#wknd-mobile-application-packages}

[!DNL AEM Package Manager]을(를) 사용하여 **AEM 작성자 및 AEM 게시 모두에 다음 AEM 컨텐츠 패키지를 설치합니다.**

* [ui.apps:GitHub > Assets > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] AEM WCM 핵심 구성 요소용 프록시 구성 요소
   * [!DNL WKND Mobile] AEM 컨텐츠 서비스 페이지의 CSS(간단한 스타일링을 위한)
* [ui.content:GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] 사이트 구조
   * [!DNL WKND Mobile] DAM 폴더 구조
   * [!DNL WKND Mobile] 이미지 자산

[7장](./chapter-7.md)에서는 [Android Studio](https://developer.android.com/studio)와 제공된 APK(Android 응용 프로그램 패키지)를 사용하여 [!DNL WKND Mobile] Android 모바일 앱을 실행합니다.

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 장 AEM 컨텐츠 패키지

이 콘텐츠 패키지 세트는 관련 장 및 모든 이전 장에 설명된 내용과 구성을 생성합니다. 이러한 패키지는 선택 사항이지만 신속하게 콘텐츠를 만들 수 있습니다.

* [제2장 내용:GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [제3장 내용:GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [제4장 내용:GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [제5장 내용:GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 소스 코드

AEM 프로젝트와 [!DNL Android Mobile App] 모두에 대한 소스 코드는 [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile)에서 사용할 수 있습니다. 이 튜토리얼을 위해 소스 코드를 작성하거나 수정할 필요가 없습니다. 이 코드는 튜토리얼의 모든 측면을 투명하게 하기 위해 제공됩니다.

자습서 또는 코드에 문제가 있는 경우 [GitHub 문제](https://github.com/adobe/aem-guides-wknd-mobile/issues)를 남겨 두십시오.

## 끝으로 건너뛰기

자습서의 끝으로 건너뛰려면 [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 컨텐츠 패키지를 **AEM 작성자 및 AEM 게시 모두에 설치할 수 있습니다.** 컨텐츠 및 구성은 AEM 작성자에 게시된 것으로 표시되지 않지만 수동 배포로 인해 [!DNL WKND Mobile App]이(가) 컨텐츠에 액세스할 수 있도록 AEM 게시에서 모든 필수 컨텐츠 및 구성을 사용할 수 있습니다.


## 다음 단계

* [2장 - 이벤트 컨텐츠 조각 모델 정의](./chapter-2.md)
