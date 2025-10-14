---
title: 빠른 설정 SPA 편집기 및 원격 SPA
description: 15분 내에 원격 SPA 및 AEM SPA 편집기를 시작하고 실행하는 방법에 대해 알아보십시오.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7629
thumbnail: 333181.jpg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: ef7a1dad-993a-4c47-a9fb-91fa73de9b5d
duration: 647
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '726'
ht-degree: 10%

---

# 빠른 설정

{{spa-editor-deprecation}}

빠른 설정은 WKND 앱을 원격 SPA로 설치 및 실행하고 AEM SPA 편집기를 사용하여 작성하는 방법에 대한 자세한 설명입니다.

빠른 설정을 통해 이 자습서의 종료 상태로 바로 이동할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/333181?quality=12&learn=on)

_빠른 설정의 비디오 둘러보기_

## 사전 요구 사항

이 튜토리얼을 실행하려면 다음 소프트웨어 및 리소스가 필요합니다.

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=ko)
+ [Node.js v18](https://nodejs.org/en/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ macOS 전용 사전 요구 사항
   + [Xcode](https://developer.apple.com/xcode/) 또는 [Xcode 명령줄 도구](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all-2.1.0.zip 이상](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql 소스 코드(분기: 기능/spa-editor)](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


이 튜토리얼에서는 다음을 가정합니다.

+ IDE로 [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/)를 사용함
+ 작업 디렉터리는 `~/Code/wknd-app`임
+ AEM SDK를 작성자 서비스로 실행 중이며 주소는 `http://localhost:4502`임
+ AEM SDK를 로컬 `admin` 계정과 암호 `admin`로 실행 중임
+ SPA는 `http://localhost:3000`에서 실행 중임

## AEM SDK 빠른 시작

기본 `admin/admin` 자격 증명으로 포트 4502에 AEM SDK 빠른 시작을 다운로드하여 설치하십시오.

1. [최신 AEM SDK 다운로드](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&orderby.sort=desc&layout=list&p.offset=0&p.limit=1)
1. `~/aem-sdk`에 AEM SDK 압축 풀기
1. AEM SDK Quickstart Jar 실행

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK은 [http://localhost:4502](http://localhost:4502)에 시작되고 자동으로 실행됩니다. 다음 자격 증명을 사용하여 로그인합니다.

+ 사용자 이름: `admin`
+ 암호: `admin`

## WKND Site 패키지 다운로드 및 설치

이 자습서는 __WKND 2.1.0+의__ 프로젝트(콘텐츠의 경우)에 종속되어 있습니다.

1. [최신 버전의 `aem-guides-wknd.all.x.x.x.zip`을(를) 다운로드](https://github.com/adobe/aem-guides-wknd/releases)
1. `admin` 자격 증명을 사용하여 [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr)에서 AEM SDK의 패키지 관리자에 로그인합니다.
1. 1단계에서 다운로드한 `aem-guides-wknd.all.x.x.x.zip`을(를) __업로드__
1. `aem-guides-wknd.all-x.x.x.zip` 항목의 __설치__ 단추를 탭합니다.

## WKND 앱 SPA 패키지 다운로드 및 설치

빠른 설정을 수행하기 위해 자습서의 최종 AEM 구성 및 콘텐츠가 포함된 AEM 패키지가 여기에 제공됩니다.

1. [다운로드 &#x200B;](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [다운로드 &#x200B;](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
1. `admin` 자격 증명을 사용하여 [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr)에서 AEM SDK의 패키지 관리자에 로그인합니다.
1. 1단계에서 다운로드한 `wknd-app.all.x.x.x.zip`을(를) __업로드__
1. `wknd-app.all.x.x.x.zip` 항목의 __설치__ 단추를 탭합니다.
1. 2단계에서 다운로드한 `wknd-app.ui.content.sample.x.x.x.zip`을(를) __업로드__
1. `wknd-app.ui.content.sample.x.x.x.zip` 항목의 __설치__ 단추를 탭합니다.

## WKND 앱 소스 다운로드

Github.com에서에서 WKND 앱의 소스 코드를으로 다운로드하고 이 자습서에서 수행한 SPA에 대한 변경 사항이 포함된 분기를 전환합니다.

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## SPA 애플리케이션 시작

프로젝트의 루트에서 SPA 프로젝트 npm 종속성을 설치하고 애플리케이션을 실행합니다.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

`npm install`을(를) 실행할 때 오류가 발생하면 다음 단계를 수행하십시오.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

SPA가 [http://localhost:3000](http://localhost:3000)에서 실행 중인지 확인하십시오.

## AEM SPA 편집기에서 컨텐츠 작성

콘텐츠를 작성하기 전에 브라우저 창을 정렬하여 AEM 작성자(`http://localhost:4502`)가 왼쪽에 있고 원격 SPA(`http://localhost:3000`)가 오른쪽에 실행됩니다. 이렇게 하면 AEM 소스 콘텐츠의 변경 사항이 SPA에 어떻게 즉시 반영되는지 볼 수 있습니다.

1. [AEM SDK 작성자 서비스](http://localhost:4502)에 `admin`(으)로 로그인
1. __사이트 > WKND 앱 > us > en__(으)로 이동합니다.
1. __WKND 앱 홈 페이지 편집__
1. __편집__ 모드로 전환

### 홈 보기의 고정 구성 요소 작성

1. __WKND Adventures__ 텍스트를 탭하여 SPA의 홈 보기로 하드코딩된 고정 제목 구성 요소를 활성화합니다
1. 제목 구성 요소의 작업 표시줄에서 __렌치__ 아이콘을 탭합니다.
1. 제목 구성 요소의 콘텐츠를 변경하고 저장합니다.
1. `http://localhost:3000`에서 실행 중인 SPA를 새로 고치고 변경 내용이 반영되었는지 확인합니다.

### 홈 보기의 컨테이너 구성 요소 작성

1. __WKND 앱 홈 페이지__&#x200B;를 편집하는 동안...
1. __SPA 편집기의 사이드바__(왼쪽)를 확장합니다.
1. __구성 요소__ 아이콘을 탭합니다.
1. WKND 로고 아래 및 고정 제목 구성 요소 위에 있는 컨테이너 구성 요소에서 구성 요소를 추가, 변경 또는 제거합니다.
1. `http://localhost:3000`에서 실행 중인 SPA를 새로 고치고 변경 내용이 반영되었는지 확인합니다.

### 동적 경로에서 컨테이너 구성 요소 작성

1. SPA 편집기에서 __미리 보기__ 모드로 전환
1. __Bali Surf Camp__ 카드를 탭하고 동적 경로로 이동합니다.
1. __일정__ 머리글 위에 있는 컨테이너 구성 요소에서 구성 요소를 추가, 변경 또는 제거합니다.
1. `http://localhost:3000`에서 실행 중인 SPA를 새로 고치고 변경 내용이 반영되었는지 확인합니다.

__WKND 앱 홈 페이지 > 어드벤처__ _필수_&#x200B;의 새 AEM 페이지에는 해당 모험의 콘텐츠 조각 이름과 일치하는 AEM 페이지 이름이 있습니다. 이는 AEM 페이지에 대한 SPA 경로 매핑이 콘텐츠 조각의 이름인 경로의 마지막 세그먼트에 따라 수행되기 때문입니다.

## 축하합니다!

이제 AEM SPA 편집기에서 제어되고 편집 가능한 영역을 사용하여 SPA를 향상시키는 방법을 빠르게 살펴볼 수 있습니다. 관심있는 경우 - 자습서의 나머지 부분을 확인하십시오. 하지만 이 빠른 설정에서 로컬 개발 환경이 이제 자습서의 끝 상태에 있으므로 새로 시작해야 합니다.
