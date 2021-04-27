---
title: 신속하게 SPA 편집기 및 원격 SPA 설정
description: 15분 만에 원격 SPA 및 AEM SPA 편집기를 사용하여 시작하는 방법 살펴보기!
topic: 헤드리스, SPA, 개발
feature: SPA 편집기, 핵심 구성 요소, API, 개발
role: Developer, Architect
level: Beginner
kt: 7629
thumbnail: kt-7629.jpeg
translation-type: tm+mt
source-git-commit: b6f63110f14ede51fa2dd740aea7cbb623cbec60
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 4%

---


# 빠른 설정

빠른 설정은 WKND 앱과 Remote SPA의 설치 및 실행 방법을 소개하고 AEM SPA Editor를 사용하여 이를 작성하는 방법을 설명하는 빠른 방법입니다.

빠른 설정을 통해 이 튜토리얼의 최종 상태가 바로 표시됩니다.

## 전제 조건

이 자습서에는 다음이 필요합니다.

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v14+](https://nodejs.org/en/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all.0.3.0.zip 이상](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql 소스 코드](https://github.com/adobe/aem-guides-wknd-graphql)

이 자습서에서는 다음 내용을 가정합니다.

+ [Microsoft® Visual Studio ](https://visualstudio.microsoft.com/) 코드 IDE
+ `~/Code/wknd-app`의 작업 디렉토리
+ `http://localhost:4502`에서 작성자 서비스로 AEM SDK 실행
+ 암호가 `admin`인 로컬 `admin` 계정으로 AEM SDK 실행
+ `http://localhost:3000`에서 SPA 실행

## AEM SDK 빠른 시작

기본 `admin/admin` 자격 증명과 함께 포트 4502에서 AEM SDK Quickstart를 다운로드하여 설치합니다.

1. [최신 AEM SDK 다운로드](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. AEM SDK를 `~/aem-sdk`에 압축 해제
1. AEM SDK 빠른 시작 Jar 실행

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK가 시작되고 [http://localhost:4502](http://localhost:4502)에 자동으로 시작됩니다. 다음 자격 증명을 사용하여 로그인합니다.

+ 사용자 이름: `admin`
+ 암호: `admin`

## WKND 사이트 패키지 다운로드 및 설치

이 자습서는 __WKND 0.3.0+의__ 프로젝트(컨텐츠의 경우)에 대한 종속성을 갖습니다.

1. [최신 버전의  `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. `admin` 자격 증명으로 [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr)에 AEM SDK의 패키지 관리자에 로그인합니다.
1. ____ 1단계에서  `aem-guides-wknd.all.x.x.x.zip` 다운로드한 파일 업로드
1. `aem-guides-wknd.all-x.x.x.zip` 항목의 __설치__ 단추를 누릅니다.

## WKND 앱 SPA 패키지 다운로드 및 설치

빠른 설정을 수행하려면 튜토리얼의 최종 AEM 구성과 내용이 포함된 AEM 패키지가 제공됩니다.

1. [다운로드 `wknd-app.all.x.x.x.zip`](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [다운로드 `wknd-app.ui.content.sample.x.x.x.zip`](./assets/quick-setup/wknd-app.ui.content.sample-1.0.0.zip)
1. `admin` 자격 증명으로 [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr)에 AEM SDK의 패키지 관리자에 로그인합니다.
1. ____ 1단계에서  `wknd-app.all.x.x.x.zip` 다운로드한 파일 업로드
1. `wknd-app.all.x.x.x.zip` 항목의 __설치__ 단추를 누릅니다.
1. ____ 2단계에서  `wknd-app.ui.content.sample.x.x.x.zip` 다운로드한 파일 업로드
1. `wknd-app.ui.content.sample.x.x.x.zip` 항목의 __설치__ 단추를 누릅니다.

## WKND 앱 소스 다운로드

Github.com에서 WKND 앱의 소스 코드를 다운로드하고 이 튜토리얼에서 수행한 SPA의 변경 사항이 포함된 분기를 전환합니다.

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
$ git checkout -b feature/spa-editor
```

## SPA 응용 프로그램 시작

프로젝트의 루트에서 SPA 프로젝트 npm 종속성을 설치하고 응용 프로그램을 실행합니다.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

`npm install`을(를) 실행할 때 오류가 발생하면 다음 단계를 시도하십시오.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

SPA이 [http://localhost:3000](http://localhost:3000)에서 실행되고 있는지 확인합니다.

## AEM SPA Editor에서 컨텐츠 작성

컨텐츠를 작성하기 전에 AEM 작성자(`http://localhost:4502`)가 왼쪽에 있고 원격 SPA(`http://localhost:3000`)이 오른쪽에서 실행되도록 브라우저 창을 정렬할 수 있습니다. 이러한 방식으로 AEM 소스 컨텐츠의 변경 사항이 SPA에 즉시 어떻게 반영되는지 확인할 수 있습니다.

1. [AEM SDK 작성자 서비스](http://localhost:4502)에 `admin` 로그인
1. __사이트 > WKND 앱 > us > en__&#x200B;으로 이동합니다.
1. __WKND 앱 홈 페이지 편집__
1. __편집__ 모드로 전환

### 홈 보기의 고정 구성 요소 작성

1. 텍스트 __WKND Adventures__&#x200B;을 눌러 고정된 제목 구성 요소(SPA 홈 보기로 하드코딩됨)를 활성화합니다.
1. 제목 구성 요소의 작업 표시줄에서 __렌치__ 아이콘을 누릅니다.
1. 제목 구성 요소의 컨텐츠를 변경하고 저장합니다.
1. `http://localhost:3000`에서 실행 중인 SPA을 새로 고치고 변경 내용이 반영되었는지 확인합니다.

### 홈 보기의 컨테이너 구성 요소 작성

1. 여전히 __WKND 앱 홈 페이지__&#x200B;를 편집하는 동안...
1. __SPA Editor의 사이드바__(왼쪽)를 확장합니다.
1. __구성 요소__ 아이콘을 누릅니다.
1. WKND 로고 아래에 있고 고정된 제목 구성 요소 위에 있는 컨테이너 구성 요소에서 구성 요소를 추가, 변경 또는 제거합니다.
1. `http://localhost:3000`에서 실행 중인 SPA을 새로 고치고 변경 내용이 반영되었는지 확인합니다.

### 동적 경로에서 컨테이너 구성 요소 작성

1. SPA 편집기에서 __미리 보기__ 모드로 전환
1. __Bali Surf Camp__ 카드를 탭하고 동적 경로로 이동합니다.
1. __일정표__ 머리글 위에 있는 사이트를 포함하는 컨테이너 구성 요소에서 구성 요소를 추가, 변경 또는 제거합니다.
1. `http://localhost:3000`에서 실행 중인 SPA을 새로 고치고 변경 내용이 반영되었는지 확인합니다.

## 축하합니다!

AEM SPA Editor를 사용하여 제어되고 편집 가능한 영역으로 SPA을 개선할 수 있는 방법을 빠르게 익힐 수 있습니다. 관심이 있는 경우 - 튜토리얼의 나머지 부분을 살펴보십시오. 그러나 이 빠른 설정에서 로컬 개발 환경이 이제 자습서의 끝 상태에 있으므로 반드시 새로 시작해야 합니다.
