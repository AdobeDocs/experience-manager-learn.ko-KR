---
title: 빠른 설정 SPA 편집기 및 원격 SPA
description: 15분 내에 원격 SPA 및 AEM SPA 편집기를 시작하고 실행하는 방법에 대해 알아보십시오.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7629
thumbnail: 333181.jpg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: ef7a1dad-993a-4c47-a9fb-91fa73de9b5d
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '793'
ht-degree: 4%

---

# 빠른 설정

빠른 설정은 WKND 앱을 원격 SPA으로 설치 및 실행하고 AEM SPA Editor를 사용하여 작성하는 방법에 대한 자세한 설명입니다.

빠른 설정을 통해 이 자습서의 종료 상태로 바로 이동할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/333181?quality=12&learn=on)

_빠른 설정에 대한 비디오 설명_

## 사전 요구 사항

이 자습서에서는 다음 작업을 수행해야 합니다.

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v18](https://nodejs.org/en/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ macOS 전용 사전 요구 사항
   + [Xcode](https://developer.apple.com/xcode/) 또는 [Xcode 명령줄 도구](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all-2.1.0.zip 이상](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql 소스 코드(분기: 기능/spa-editor)](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


이 자습서에서는 다음과 같이 가정합니다.

+ [Microsoft® Visual Studio 코드](https://visualstudio.microsoft.com/) IDE로
+ 의 작업 디렉터리 `~/Code/wknd-app`
+ 에서 작성자 서비스로 AEM SDK 실행 `http://localhost:4502`
+ 로컬로 AEM SDK 실행 `admin` 암호가 있는 계정 `admin`
+ SPA 실행 `http://localhost:3000`

## AEM SDK 빠른 시작

포트 4502에서 기본적으로 AEM SDK 빠른 시작을 다운로드하여 설치합니다. `admin/admin` 자격 증명.

1. [최신 AEM SDK 다운로드](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. AEM SDK 압축 풀기 `~/aem-sdk`
1. AEM SDK Quickstart Jar 실행

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK가에 시작 및 자동 시작 [http://localhost:4502](http://localhost:4502). 다음 자격 증명을 사용하여 로그인합니다.

+ 사용자 이름: `admin`
+ 암호: `admin`

## WKND Site 패키지 다운로드 및 설치

이 튜토리얼은 다음에 종속되어 있습니다. __WKND 2.1.0+__ 프로젝트(콘텐츠).

1. [최신 버전 다운로드 `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. 에서 AEM SDK의 패키지 관리자에 로그인합니다. [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) (으)로 `admin` 자격 증명.
1. __업로드__ 다음 `aem-guides-wknd.all.x.x.x.zip` 1단계에서 다운로드됨
1. 탭 __설치__ 항목 단추 `aem-guides-wknd.all-x.x.x.zip`

## WKND 앱 SPA 패키지 다운로드 및 설치

빠른 설정을 수행하기 위해 자습서의 최종 AEM 구성 및 콘텐츠가 포함된 AEM 패키지가 여기에 제공됩니다.

1. [다운로드 ](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [다운로드 ](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
1. 에서 AEM SDK의 패키지 관리자에 로그인합니다. [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) (으)로 `admin` 자격 증명.
1. __업로드__ 다음 `wknd-app.all.x.x.x.zip` 1단계에서 다운로드됨
1. 탭 __설치__ 항목 단추 `wknd-app.all.x.x.x.zip`
1. __업로드__ 다음 `wknd-app.ui.content.sample.x.x.x.zip` 2단계에서 다운로드됨
1. 탭 __설치__ 항목 단추 `wknd-app.ui.content.sample.x.x.x.zip`

## WKND 앱 소스 다운로드

Github.com에서 WKND 앱의 소스 코드를으로 다운로드하고 이 자습서에서 수행한 SPA에 대한 변경 사항이 포함된 분기를 전환합니다.

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

실행 시 오류가 있는 경우 `npm install` 다음 단계를 수행하십시오.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

SPA이에서 실행 중인지 확인 [http://localhost:3000](http://localhost:3000).

## AEM SPA Editor에서 컨텐츠 작성

콘텐츠를 작성하기 전에 AEM 작성자(`http://localhost:4502`)가 왼쪽에 있고 원격 SPA(`http://localhost:3000`)는 오른쪽에서 실행됩니다. 이렇게 하면 AEM 소스 콘텐츠에 대한 변경 사항이 SPA에 즉시 반영되는 방식을 확인할 수 있습니다.

1. 에 로그인 [AEM SDK Author 서비스](http://localhost:4502) 다음으로: `admin`
1. 다음으로 이동 __사이트 > WKND 앱 > us > en__
1. 편집 __WKND 앱 홈 페이지__
1. 다음으로 전환 __편집__ 모드

### 홈 보기의 고정 구성 요소 작성

1. 텍스트 탭 __어드벤처__ 고정 제목 구성 요소를 활성화하려면 (SPA 홈 보기로 하드코딩됨)
1. 탭 __렌치__ 제목 구성 요소의 작업 표시줄에 있는 아이콘
1. 제목 구성 요소의 콘텐츠를 변경하고 저장합니다.
1. 실행 중인 SPA 새로 고침 `http://localhost:3000` 그리고 변경 사항이 반영되었는지 확인합니다.

### 홈 보기의 컨테이너 구성 요소 작성

1. 을 편집하는 동안 __WKND 앱 홈 페이지__...
1. 확장 __SPA 편집기 사이드바__ (왼쪽)
1. 탭 __구성 요소__ 아이콘
1. WKND 로고 아래 및 고정 제목 구성 요소 위에 있는 컨테이너 구성 요소에서 구성 요소를 추가, 변경 또는 제거합니다.
1. 실행 중인 SPA 새로 고침 `http://localhost:3000` 그리고 변경 사항이 반영되었는지 확인합니다.

### 동적 경로에서 컨테이너 구성 요소 작성

1. 다음으로 전환 __미리 보기__ SPA 편집기의 모드
1. 을 누릅니다. __발리 서프 캠프__ 카드 및 동적 경로로 이동
1. 위의 컨테이너 구성 요소에서 구성 요소를 추가, 변경 또는 제거합니다. __여행 일정__ 제목
1. 실행 중인 SPA 새로 고침 `http://localhost:3000` 그리고 변경 사항이 반영되었는지 확인합니다.

아래의 새 AEM 페이지 __WKND 앱 홈 페이지 > 어드벤처__ _필수_ 해당 모험의 콘텐츠 조각 이름과 일치하는 AEM 페이지 이름이 있습니다. 이는 AEM 페이지에 대한 SPA 경로 매핑이 콘텐츠 조각의 이름인 경로의 마지막 세그먼트를 기반으로 하기 때문입니다.

## 축하합니다!

AEM SPA Editor가 제어되고 편집 가능한 영역을 통해 SPA을 향상시키는 방법을 빠르게 이해할 수 있습니다! 관심있는 경우 - 자습서의 나머지 부분을 확인하십시오. 하지만 이 빠른 설정에서 로컬 개발 환경이 이제 자습서의 끝 상태에 있으므로 새로 시작해야 합니다.
