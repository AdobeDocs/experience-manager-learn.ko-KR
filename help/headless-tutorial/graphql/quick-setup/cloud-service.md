---
title: AEM as a Cloud Service AEM 헤드리스 빠른 설정
description: AEM 헤드리스 빠른 설정을 사용하면 WKND Site 샘플 프로젝트의 콘텐츠를 사용하여 AEM Headless를 직접 사용하고 AEM Headless GraphQL API를 통해 컨텐츠를 소비하는 React 앱을 사용할 수 있습니다.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 9442
thumbnail: 339073.jpg
exl-id: 62e807b7-b1a4-4344-9b1e-2c626b869e10
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '1081'
ht-degree: 2%

---

# AEM as a Cloud Service AEM 헤드리스 빠른 설정

AEM Headless 빠른 설정을 사용하면 WKND Site 샘플 프로젝트의 컨텐츠와 AEM Headless GraphQL API를 통해 컨텐츠를 소비하는 샘플 React App (a SPA)을 직접 사용할 수 있습니다.

## 사전 요구 사항

이 빠른 설정을 수행하려면 다음 사항이 필요합니다.

+ AEM as a Cloud Service 샌드박스 환경(바람직하게는 개발)
+ AEM as a Cloud Service 및 Cloud Manager 액세스
   + __AEM 관리자__ AEM as a Cloud Service 액세스
   + __Cloud Manager - 배포 관리자__ cloud Manager 액세스
+ 다음 도구는 로컬로 설치해야 합니다.
   + [Node.js v18](https://nodejs.org/en/)
   + [Git](https://git-scm.com/)
   + IDE(예: [Microsoft® Visual Studio 코드](https://code.visualstudio.com/))

## 1. Cloud Manager Git 리포지토리 만들기

먼저 WKND 사이트를 배포하는 데 사용되는 Cloud Manager Git 리포지토리를 만듭니다. WKND 사이트는 컨텐츠(컨텐츠 조각)와 빠른 설정의 React App에서 사용하는 GraphQL AEM 엔드포인트를 포함하는 샘플 AEM 웹 사이트 프로젝트입니다.

_단계 화면_
>[!VIDEO](https://video.tv.adobe.com/v/339073?quality=12&learn=on)

1. 다음으로 이동 [https://my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com)
1. Cloud Manager 선택 __프로그램__ 여기에는 이 빠른 설정에 사용할 AEM as a Cloud Service 환경이 포함되어 있습니다
1. WKND Site 프로젝트를 위한 Git 리포지토리 만들기
   1. 선택 __저장소__ 위쪽 탐색에서
   1. 선택 __저장소 추가__ 상단 작업 모음에서
   1. 새 Git 리포지토리의 이름을 지정합니다. `aem-headless-quick-setup-wknd`
      + Git 리포지토리 이름은 Adobe 조직별로 고유해야 합니다.
   1. 선택 __저장__&#x200B;및에서 Git 리포지토리가 초기화될 때까지 기다립니다.

## 2. 샘플 WKND Site 프로젝트를 Cloud Manager Git 리포지토리에 푸시

만든 Cloud Manager Git 리포지토리를 사용하여 GitHub에서 WKND Site 프로젝트의 소스 코드를 복제하고 Cloud Manager Git 리포지토리에 푸시합니다. 이제 Cloud Manager가 WKND 사이트 프로젝트에 액세스하고 AEM as a Cloud Service 환경에 배포할 수 있습니다.

_단계 화면_
>[!VIDEO](https://video.tv.adobe.com/v/339074?quality=12&learn=on)

1. 명령줄에서 GitHub에서 샘플 WKND Site 프로젝트의 소스 코드를 복제합니다

   ```shell
   $ mkdir -p ~/Code
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Cloud Manager Git 리포지토리를 원격으로 추가
   1. 선택 __저장소__ 위쪽 탐색에서
   1. 선택 __보고서 정보에 액세스__ 맨 위 작업 표시줄에서
   1. 에 있는 Execute 명령 __Git 리포지토리에 원격 추가__ 명령줄에서

      ```shell
      $ cd aem-guides-wknd
      $ git remote add adobe https://git.cloudmanager.adobe.com/<YOUR ADOBE ORGANIZATION>/aem-headless-quick-setup-wknd/
      ```

1. 샘플 프로젝트의 소스 코드를 로컬 Git 리포지토리의 Cloud Manager Git 리포지토리에 푸시합니다

   ```shell
   $ git push adobe main:main
   ```

   자격 증명을 입력하라는 메시지가 표시되면 __사용자 이름__ 및 __암호__ Cloud Manager의 __저장소 정보__ 모달.

## 3. AEM as a Cloud Service에 WKND 사이트 배포

WKND 사이트 프로젝트가 Cloud Manager Git 리포지토리에 푸시되어 Cloud Manager 파이프라인을 사용하여 AEM as a Cloud Service에 배포할 수 없습니다.

WKND Site 프로젝트는 AEM Headless GraphQL API보다 React 앱이 사용하는 샘플 컨텐츠를 제공합니다.

_단계 화면_
>[!VIDEO](https://video.tv.adobe.com/v/339075?quality=12&learn=on)

1. 첨부 __비프로덕션 배포 파이프라인__ 새 Git 리포지토리에
   1. 선택 __파이프라인__ 위쪽 탐색에서
   1. 선택 __파이프라인 추가__ 맨 위 작업 표시줄에서
   1. 설정 __구성__ 탭
      1. 선택 __배포 파이프라인__ 옵션
      1. 설정 __비프로덕션 파이프라인 이름__ to `Dev Deployment pipeline`
      1. 선택 __배포 트리거 > Git 변경 시__
      1. 선택 __중요한 지표 실패 동작 > 즉시 계속__
      1. 선택 __계속__
   1. __소스 코드__ 탭에서
      1. 선택 __전체 스택 코드__ 옵션
      1. 을(를) 선택합니다 __AEM as a Cloud Service 개발 환경__ 에서 __적합한 배포 환경__ 선택 상자
      1. 선택 `aem-headless-quick-setup-wknd` 에서 __저장소__ 선택 상자
      1. 선택 `main` 에서 __Git 분기__ 선택 상자
      1. __저장__&#x200B;을 선택합니다
1. 를 실행합니다. __개발 배포 파이프라인__
   1. 선택 __개요__ 위쪽 탐색에서
   1. 새로 만든 항목을 찾습니다 __개발 배포 파이프라인__ 에서 __파이프라인__ 섹션
   1. 을(를) 선택합니다 __...__ 파이프라인 엔트리 오른쪽에 있습니다.
   1. 선택 __실행__&#x200B;를 반환하고 모달에서 확인합니다.
   1. 을(를) 선택합니다 __...__ 현재 실행 중인 파이프라인의 오른쪽으로 이동
   1. 선택 __세부 사항 보기__
1. 파이프라인 실행의 세부 정보에서 성공적으로 완료될 때까지 진행 상황을 모니터링합니다. 파이프라인 실행은 30~40분 정도 소요됩니다.

## 4. WKND React 앱을 다운로드하여 실행합니다.

AEM이 WKND Site 프로젝트의 컨텐츠와 as a Cloud Service으로 부트된 상태에서 AEM Headless GraphQL API를 통해 WKND Site의 컨텐츠를 소비하는 샘플 WKND React 앱을 다운로드하여 시작합니다.

_단계 화면_
>[!VIDEO](https://video.tv.adobe.com/v/339076?quality=12&learn=on)

1. 명령줄에서 GitHub에서 React 앱의 소스 코드를 복제합니다.

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 폴더를 엽니다. `~/Code/aem-guides-wknd-graphql/react-app` IDE에서
1. IDE에서 파일을 엽니다 `.env.development`.
1. AEM as a Cloud Service을 가리킵니다. __게시__ 서비스의 호스트 URI를  `REACT_APP_HOST_URI` 속성을 사용합니다.

   ```plain
   REACT_APP_HOST_URI=https://publish-pXXXX-eYYYY.adobeaemcloud.com
   ...
   ```

   AEM as a Cloud Service 게시 서비스의 호스트 URI를 찾으려면 다음을 수행하십시오.

   1. Cloud Manager에서 __환경__ 위쪽 탐색에서
   1. 선택 __개발__ 환경
   1. 을(를) 찾습니다 __게시 서비스(AEM 및 Dispatcher)__ 링크 __환경 세그먼트__ 표
   1. 링크의 주소를 복사하여 AEM as a Cloud Service 게시 서비스의 URI로 사용합니다

1. IDE에서 변경 내용을 `.env.development`
1. 명령줄에서 React 앱을 실행합니다

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. 로컬에서 실행되는 React App이 다음으로 시작됩니다 [http://localhost:3000](http://localhost:3000) 및 는 AEM 헤드리스 GraphQL API를 사용하여 AEM as a Cloud Service에서 가져온 모험 목록을 표시합니다.

## 5. AEM에서 컨텐츠 편집

AEM Headless GraphQL API의 컨텐츠에 연결하고 소비하는 샘플 WKND React 앱과 함께 AEM Author Service에서 컨텐츠를 작성하고 React App의 경험이 함께 어떻게 업데이트되는지 확인합니다.

_단계 화면_
>[!VIDEO](https://video.tv.adobe.com/v/339077?quality=12&learn=on)

1. AEM as a Cloud Service 작성자 서비스에 로그인합니다.
1. 다음으로 이동 __자산 > 파일 > WKND 공유 > 영어 > 모험__
1. 를 엽니다. __서던 유타 사이클__ 폴더
1. 을(를) 선택합니다 __서던 유타 사이클__ 컨텐츠 조각 을 선택하고 을 선택합니다 __편집__ 맨 위 작업 표시줄에서
1. 컨텐츠 조각의 일부 필드를 업데이트합니다(예: ).
   + 제목: `Cycling Utah's National Parks`
   + 이동 길이: `6 Days`
   + 문제: `Intermediate`
   + 가격: `3500`
   + 기본 이미지: `/content/dam/wknd-shared/en/activities/cycling/mountain-biking.jpg`
1. 선택 __저장__ 상단 작업 모음에서
1. 선택 __빠른 게시__ 상단 작업 표시줄의 __...__
1. 실행 중인 React 앱 새로 고침 [http://localhost:3000](http://localhost:3000).
1. React 앱에서, 이제 업데이트된 사이클링 모험을 선택하고 컨텐츠 조각에 수행된 컨텐츠 변경 사항을 확인합니다.

1. AEM 작성자 서비스에서 동일한 방법 사용:
   1. 기존 Adventure 컨텐츠 조각을 게시 취소하고 React 앱 경험에서 제거되었는지 확인합니다
   1. 새 Adventure 컨텐츠 조각을 만들어 게시하고 React 앱 경험에 표시되는지 확인합니다

   >[!TIP]
   >
   > 새 컨텐츠 조각 만들기 및 게시 또는 기존 컨텐츠 조각 게시 취소에 익숙하지 않다면 위의 스크린에서 확인하십시오.

## 축하합니다!

축하합니다! AEM Headless를 사용하여 React 앱을 작동했습니다!

React 앱에서 AEM as a Cloud Service의 컨텐츠를 사용하는 방법을 자세히 이해하려면 다음을 확인하십시오 [AEM Headless 자습서](../multi-step/overview.md). 이 자습서에서는 생성된 AEM의 컨텐츠 조각과 이 React App이 컨텐츠를 JSON으로 사용하는 방법을 설명합니다.

### 다음 단계

+ [AEM Headless 자습서 시작](../multi-step/overview.md)
