---
title: AEM Adaptive Forms에서 CAPTCHA 사용
description: AEM Adaptive Forms과 함께 CAPTCHA 추가 및 사용.
feature: Adaptive Forms,Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 7e5dcc6e-fe56-49af-97e3-7dfaa9c8738f
last-substantial-update: 2019-06-09T00:00:00Z
duration: 260
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# AEM Adaptive Forms에서 CAPTCHA 사용{#using-captchas-with-aem-adaptive-forms}

AEM Adaptive Forms과 함께 CAPTCHA 추가 및 사용.

>[!VIDEO](https://video.tv.adobe.com/v/18336?quality=12&learn=on)

*이 비디오는 내장된 AEM CAPTCHA 서비스와 Google의 reCAPTCHA 서비스를 모두 사용하여 AEM 적응형 양식에 CAPTCHA를 추가하는 프로세스를 안내합니다.*

>[!NOTE]
>
>이 기능은 AEM 6.3 이상에서만 사용할 수 있습니다.

>[!NOTE]
>
>**게시 인스턴스에서 reCaptcha를 구성하려면 단계를 따르십시오.**
>
>작성자 인스턴스에서 reCaptach 구성
>
>Felix 열기 [웹 콘솔](http://localhost:4502/system/console/bundles) 작성자 인스턴스에서
>
>com.adobe.granite.crypto.file 번들 검색
>
>번들 ID를 확인합니다. 내 생각에는 20이다
>
>작성자 인스턴스의 파일 시스템에 있는 번들 ID로 이동합니다
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* HMAC 및 마스터 파일 복사
>
를 엽니다. [felix 웹 콘솔](http://localhost:4502/system/console/bundles) 게시 인스턴스에서 확인할 수 있습니다. com.adobe.granite.crypto.file 번들을 검색합니다. 번들 ID를 확인합니다
>
게시 인스턴스의 파일 시스템에 있는 번들 ID로 이동합니다
>
* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* 기존 HMAC 및 마스터 파일을 삭제합니다.
* 작성자 인스턴스에서 복사한 HMAC 및 마스터 파일 붙여넣기
>
AEM 게시 서버 다시 시작

## 지원 자료 {#supporting-materials}

* [Google recaptcha](https://www.google.com/recaptcha)
