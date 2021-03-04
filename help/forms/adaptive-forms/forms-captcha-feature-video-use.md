---
title: AEM Adaptive Forms에서 CAPTCHA 사용
seo-title: AEM Adaptive Forms에서 CAPTCHA 사용
description: AEM Adaptive Forms에서 CAPTCHA 추가 및 사용
seo-description: AEM Adaptive Forms에서 CAPTCHA 추가 및 사용
feature: 적응형 양식
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
uuid: bd63e207-4f4d-4f34-9ac4-7572ed26f646
discoiquuid: 5e184e44-e385-4df7-b7ed-085239f2a642
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---


# AEM Adaptive Forms에서 CAPTCHA 사용{#using-captchas-with-aem-adaptive-forms}

AEM Adaptive Forms에서 CAPTCHA 추가 및 사용

이 기능의 라이브 데모를 연결하는 링크는 [AEM Forms samples](https://forms.enablementadobe.com/content/samples/samples.html?query=0) 페이지를 방문하십시오.

>[!VIDEO](https://video.tv.adobe.com/v/18336/?quality=9&learn=on)

*이 비디오는 내장된 AEM CAPTCHA 서비스와 Google의 reCAPTCHA 서비스를 모두 사용하여 AEM 응용 양식에 CAPTCHA를 추가하는 과정을 안내합니다.*

>[!NOTE]
>
>이 기능은 AEM 6.3 버전에서만 사용할 수 있습니다.

>[!NOTE]
>
>**게시 인스턴스에서 reCaptcha를 구성하려면 다음 단계를 수행하십시오**
>
>작성 인스턴스에서 다시 캡처 구성
>
>작성 인스턴스에서 felix [웹 콘솔](http://localhost:4502/system/console/bundles)을 엽니다.
>
>com.adobe.granite.crypto.file bundle 검색
>
>번들 ID를 확인합니다. 내 경우에는 20이다
>
>작성자 인스턴스의 파일 시스템에서 번들 ID로 이동합니다.
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* HMAC 및 마스터 파일 복사

게시 인스턴스에서 [felix 웹 콘솔](http://localhost:4502/system/console/bundles)을 엽니다. com.adobe.granite.crypto.file bundle을 검색합니다. 번들 ID 확인
게시 인스턴스의 파일 시스템에서 번들 ID로 이동합니다.
* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* 기존 HMAC 및 마스터 파일을 삭제합니다.
* 작성 인스턴스에서 복사한 HMAC 및 마스터 파일 붙여넣기

AEM 게시 서버를 다시 시작합니다.

## 지원 자료 {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)

