---
title: SPA 편집기 및 원격 SPA 빠른 설정
description: 15분 안에 원격 SPA 및 AEM SPA 편집기를 사용하여 를 시작하고 실행하는 방법을 배웁니다.
topic: 헤드리스, SPA, 개발
feature: SPA 편집기, 핵심 구성 요소, API, 개발
role: Developer, Architect
level: Beginner
kt: 7629
thumbnail: 333181.jpg
source-git-commit: 6981a1ad5019fd383b1ca1f6fbfbec87e81a6e31
workflow-type: tm+mt
source-wordcount: '801'
ht-degree: 4%

---


# 빠른 설정

빠른 설정은 WKND 앱과 Remote SPA을 설치 및 실행하고 AEM SPA Editor를 사용하여 작성하는 방법을 소개하는 빠른 연습입니다.

빠른 설정을 사용하면 이 자습서의 종료 상태로 바로 이동할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/333181/?quality=12&learn=on)

_빠른 설정의 비디오 둘러보기_

## 전제 조건

이 자습서에는 다음 내용이 필요합니다.

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v14+](https://nodejs.org/en/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ macOS만 해당 사전 요구 사항
   + [](https://developer.apple.com/xcode/) Xcodeor  [Xcode 명령줄 도구](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all.0.3.0.zip 이상](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql 소스 코드](https://github.com/adobe/aem-guides-wknd-graphql)


이 자습서에서는 다음을 가정합니다.

+ [Microsoft® Visual Studio ](https://visualstudio.microsoft.com/) Codeas the IDE
+ `~/Code/wknd-app`의 작업 디렉터리
+ `http://localhost:4502`에서 작성자 서비스로 AEM SDK 실행
+ 암호 `admin`를 사용하여 로컬 `admin` 계정으로 AEM SDK 실행
+ `http://localhost:3000`에서 SPA 실행

## AEM SDK 빠른 시작

기본 `admin/admin` 자격 증명으로 포트 4502에 AEM SDK Quickstart를 다운로드하여 설치합니다.

1. [최신 AEM SDK 다운로드](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. AEM SDK의 압축을 `~/aem-sdk` (으)로 해제합니다.
1. AEM SDK Quickstart Jar 실행

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK는 [http://localhost:4502](http://localhost:4502)에서 자동으로 시작됩니다. 다음 자격 증명을 사용하여 로그인합니다.

+ 사용자 이름: `admin`
+ 암호: `admin`

## WKND 사이트 패키지 다운로드 및 설치

이 자습서에서는 __WKND 0.3.0+의__ 프로젝트(컨텐츠용)에 대한 종속성을 갖습니다.

1. [최신 버전의 를 다운로드합니다.  `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. `admin` 자격 증명으로 AEM SDK의 패키지 관리자([http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr)에 로그인합니다.
1. ____ 1단계에서  `aem-guides-wknd.all.x.x.x.zip` 다운로드한 업로드
1. `aem-guides-wknd.all-x.x.x.zip` 항목의 __Install__ 단추를 누릅니다

## WKND App SPA 패키지 다운로드 및 설치

빠른 설정을 수행하려면 자습서의 최종 AEM 구성 및 컨텐츠가 포함된 AEM 패키지가 제공됩니다.

1. [다운로드 ](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [다운로드 ](./assets/quick-setup/wknd-app.ui.content.sample-1.0.0.zip)
1. `admin` 자격 증명으로 AEM SDK의 패키지 관리자([http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr)에 로그인합니다.
1. ____ 1단계에서  `wknd-app.all.x.x.x.zip` 다운로드한 업로드
1. `wknd-app.all.x.x.x.zip` 항목의 __Install__ 단추를 누릅니다
1. ____ 2단계에서  `wknd-app.ui.content.sample.x.x.x.zip` 다운로드한 업로드
1. `wknd-app.ui.content.sample.x.x.x.zip` 항목의 __Install__ 단추를 누릅니다

## WKND 앱 소스 다운로드

Github.com에서 WKND 앱의 소스 코드를 다운로드하고 이 자습서에서 수행한 SPA의 변경 사항이 포함된 분기를 전환합니다.

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## SPA 애플리케이션 시작

프로젝트의 루트에서 SPA 프로젝트 npm 종속성을 설치하고 응용 프로그램을 실행합니다.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

`npm install`을 실행할 때 오류가 발생하면 다음 단계를 수행하십시오.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

SPA이 [http://localhost:3000](http://localhost:3000)에서 실행 중인지 확인합니다.

## AEM SPA 편집기에서 컨텐츠 작성

컨텐츠를 작성하기 전에 AEM 작성자(`http://localhost:4502`)가 왼쪽에 있고 원격 SPA(`http://localhost:3000`)가 오른쪽에서 실행되도록 브라우저 창을 정렬합니다. 이러한 배치를 통해 AEM 소스 컨텐츠의 변경 사항이 SPA에 즉시 반영되는 방식을 확인할 수 있습니다.

1. [AEM SDK 작성자 서비스](http://localhost:4502)에 `admin` 로그인
1. __사이트 > WKND 앱 > us > en__ 로 이동합니다.
1. __WKND 앱 홈 페이지 편집__
1. __편집__ 모드로 전환

### 홈 보기의 고정 구성 요소 작성

1. 텍스트 __WKND Adventures__&#x200B;를 탭하여 고정된 제목 구성 요소(SPA 홈 보기로 하드코딩됨)를 활성화합니다
1. 제목 구성 요소의 작업 표시줄에서 __렌치__ 아이콘을 누릅니다
1. 제목 구성 요소의 컨텐츠를 변경하고 저장합니다
1. `http://localhost:3000`에서 실행 중인 SPA을 새로 고침하고 변경 사항이 반영되었는지 확인합니다

### 홈 보기의 컨테이너 구성 요소 작성

1. 여전히 __WKND 앱 홈 페이지를 편집하는 동안__...
1. __SPA Editor의 사이드바__(왼쪽에서)를 확장합니다.
1. __구성 요소__ 아이콘을 누릅니다
1. WKND 로고 아래에 있고 고정된 제목 구성 요소 위에 있는 컨테이너 구성 요소에서 구성 요소를 추가, 변경 또는 제거합니다
1. `http://localhost:3000`에서 실행 중인 SPA을 새로 고침하고 변경 사항이 반영되었는지 확인합니다

### 동적 경로에서 컨테이너 구성 요소 작성

1. SPA 편집기에서 __미리 보기__ 모드로 전환합니다.
1. __Bali Surf Camp__ 카드를 탭하고 해당 동적 경로로 이동합니다
1. __Journey__ 제목 위에 있는 컨테이너 구성 요소에서 구성 요소를 추가, 변경 또는 제거합니다
1. `http://localhost:3000`에서 실행 중인 SPA을 새로 고침하고 변경 사항이 반영되었는지 확인합니다

__WKND 앱 홈 페이지 > Adventure__ _에는 해당 모험의 컨텐츠 조각 이름과 일치하는 AEM 페이지 이름이 있어야 합니다._ AEM 페이지로 가는 SPA 경로는 컨텐츠 조각의 이름인 경로의 마지막 세그먼트를 기반으로 하기 때문입니다.

## 축하합니다!

AEM SPA Editor에서 제어되고 편집 가능한 영역으로 SPA을 향상시키는 방법을 간단히 확인할 수 있습니다. 관심이 있는 경우 자습서의 나머지 부분을 확인해보십시오. 이 빠른 설정에서 로컬 개발 환경이 이제 자습서의 종료 상태에 있으므로 새로 시작해야 합니다.
