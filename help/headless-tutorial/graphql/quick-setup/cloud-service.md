---
title: AEMas a Cloud Service 용 AEM Headless 빠른 설정
description: AEM Headless 빠른 설정은 WKND Site 샘플 프로젝트의 콘텐츠 및 AEM Headless GraphQL API에 대한 콘텐츠를 사용하는 React 앱을 사용하여 AEM Headless를 실습해 볼 수 있도록 합니다.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-9442
thumbnail: 339073.jpg
exl-id: 62e807b7-b1a4-4344-9b1e-2c626b869e10
duration: 781
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1078'
ht-degree: 0%

---

# AEMas a Cloud Service 용 AEM Headless 빠른 설정

AEM Headless 빠른 설정은 WKND Site 샘플 프로젝트 및 AEM Headless GraphQL API에 대한 콘텐츠를 사용하는 샘플 React 앱(SPA)의 콘텐츠를 사용하여 AEM Headless를 실습해 볼 수 있도록 합니다.

## 사전 요구 사항

이 빠른 설정을 수행하려면 다음이 필요합니다.

+ AEM as a Cloud Service 샌드박스 환경(개발 권장)
+ AEM as a Cloud Service 및 Cloud Manager 액세스
   + __AEM 관리자__ AEM as a Cloud Service 액세스
   + __Cloud Manager - 배포 관리자__ cloud Manager 액세스
+ 다음 도구를 로컬에 설치해야 합니다.
   + [Node.js v18](https://nodejs.org/en/)
   + [Git](https://git-scm.com/)
   + IDE(예: [Microsoft® Visual Studio 코드](https://code.visualstudio.com/))

## 1. Cloud Manager Git 저장소 만들기

먼저 WKND 사이트를 배포하는 데 사용되는 Cloud Manager Git 저장소를 생성합니다. WKND Site 는 빠른 설정의 React 앱에서 사용하는 컨텐츠(컨텐츠 조각) 및 GraphQL AEM 엔드포인트가 포함된 샘플 AEM 웹 사이트 프로젝트입니다.

_단계 스크린캐스트_
>[!VIDEO](https://video.tv.adobe.com/v/339073?quality=12&learn=on)

1. 다음으로 이동 [https://my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com)
1. Cloud Manager 선택 __프로그램__ 이 빠른 설정에 사용할 AEM as a Cloud Service 환경이 포함된 경우
1. WKND 사이트 프로젝트에 대한 Git 저장소 만들기
   1. 선택 __저장소__ 위쪽 탐색
   1. 선택 __저장소 추가__ 맨 위의 작업 표시줄에서
   1. 새 Git 저장소에 이름을 지정합니다. `aem-headless-quick-setup-wknd`
      + Git 저장소 이름은 Adobe 조직별로 고유해야 하며
   1. 선택 __저장__&#x200B;및 Git 저장소가 초기화될 때까지 대기

## 2. 샘플 WKND 사이트 프로젝트를 Cloud Manager Git 저장소에 푸시

Cloud Manager Git 저장소가 생성되면 GitHub에서 WKND 사이트 프로젝트의 소스 코드를 복제하여 Cloud Manager Git 저장소로 푸시합니다. 이제 Cloud Manager를 사용하여 WKND 사이트 프로젝트에 액세스하고 AEM as a Cloud Service 환경에 배포합니다.

_단계 스크린캐스트_
>[!VIDEO](https://video.tv.adobe.com/v/339074?quality=12&learn=on)

1. 명령줄에서 GitHub에서 샘플 WKND 사이트 프로젝트의 소스 코드를 복제합니다

   ```shell
   $ mkdir -p ~/Code
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Cloud Manager Git 저장소를 원격으로 추가
   1. 선택 __저장소__ 위쪽 탐색
   1. 선택 __저장소 정보 액세스__ 맨 위의 작업 표시줄에서
   1. 다음에서 명령 실행 찾음: __Git 저장소에 원격 장치 추가__ 명령줄에서

      ```shell
      $ cd aem-guides-wknd
      $ git remote add adobe https://git.cloudmanager.adobe.com/<YOUR ADOBE ORGANIZATION>/aem-headless-quick-setup-wknd/
      ```

1. 로컬 Git 저장소에서 Cloud Manager Git 저장소로 샘플 프로젝트의 소스 코드 푸시

   ```shell
   $ git push adobe main:main
   ```

   자격 증명을 입력하라는 메시지가 뜨면 __사용자 이름__ 및 __암호__ Cloud Manager의 __저장소 정보__ 모달

## AEM 3. WKND 사이트를 as a Cloud Service으로 배포

WKND 사이트 프로젝트가 Cloud Manager Git 저장소로 푸시되면 Cloud Manager 파이프라인을 사용하여 AEM에 as a Cloud Service으로 배포할 수 없습니다.

WKND 사이트 프로젝트는 AEM Headless GraphQL API에 대해 React 앱이 사용하는 샘플 콘텐츠를 제공합니다.

_단계 스크린캐스트_
>[!VIDEO](https://video.tv.adobe.com/v/339075?quality=12&learn=on)

1. 첨부 __비프로덕션 배포 파이프라인__ 새 Git 저장소로
   1. 선택 __파이프라인__ 위쪽 탐색
   1. 선택 __파이프라인 추가__ 맨 위의 작업 표시줄에서
   1. 다음에서 __구성__ 탭
      1. 선택 __배포 파이프라인__ 옵션
      1. 설정 __비프로덕션 파이프라인 이름__ 끝 `Dev Deployment pipeline`
      1. 선택 __배포 트리거 > Git 변경 시__
      1. 선택 __중요한 지표 장애 비헤이비어 > 즉시 계속__
      1. 선택 __계속__
   1. 다음에서 __소스 코드__ 탭
      1. 선택 __전체 스택 코드__ 옵션
      1. 다음 항목 선택 __AEM as a Cloud Service 개발 환경__ 다음에서 __적합한 배포 환경__ 선택 상자
      1. 선택 `aem-headless-quick-setup-wknd` 다음에서 __저장소__ 선택 상자
      1. 선택 `main` 다음에서 __Git 분기__ 선택 상자
      1. 선택 __저장__
1. 실행 __개발 배포 파이프라인__
   1. 선택 __개요__ 위쪽 탐색
   1. 새로 생성된 를 찾습니다. __개발 배포 파이프라인__ 다음에서 __파이프라인__ 섹션
   1. 다음 항목 선택 __...__ 파이프라인 항목 오른쪽
   1. 선택 __실행__&#x200B;및 를 클릭하고 모달에서 확인합니다
   1. 다음 항목 선택 __...__ 현재 실행 중인 파이프라인 오른쪽에
   1. 선택 __세부 정보 보기__
1. 파이프라인 실행의 세부 정보에서 완료될 때까지 진행률을 모니터링합니다. 파이프라인 실행은 30~40분 정도 소요됩니다.

## 4. WKND React 앱 다운로드 및 실행

WKND 사이트 프로젝트의 콘텐츠로 as a Cloud Service 부트스트랩된 AEM을 사용하여 AEM Headless GraphQL API에 대해 WKND 사이트의 콘텐츠를 사용하는 샘플 WKND React 앱을 다운로드하여 시작합니다.

_단계 스크린캐스트_
>[!VIDEO](https://video.tv.adobe.com/v/339076?quality=12&learn=on)

1. 명령줄에서 GitHub에서 React 앱의 소스 코드를 복제합니다.

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 폴더 열기 `~/Code/aem-guides-wknd-graphql/react-app` IDE에서.
1. IDE에서 파일을 엽니다. `.env.development`.
1. AEM as a Cloud Service을 가리킵니다. __게시__ 에서 서비스의 호스트 URI  `REACT_APP_HOST_URI` 속성.

   ```plain
   REACT_APP_HOST_URI=https://publish-pXXXX-eYYYY.adobeaemcloud.com
   ...
   ```

   AEM as a Cloud Service Publish 서비스의 호스트 URI를 찾으려면 다음을 수행합니다.

   1. Cloud Manager에서 __환경__ 위쪽 탐색
   1. 선택 __개발__ 환경
   1. 를 찾습니다. __게시 서비스(AEM 및 Dispatcher)__ 링크 __환경 세그먼트__ 표
   1. 링크의 주소를 복사하여 AEM as a Cloud Service Publish 서비스의 URI로 사용합니다

1. IDE에서 변경 내용을 `.env.development`
1. 명령줄에서 React 앱을 실행합니다

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. 로컬에서 실행되는 React 앱은에서 시작합니다. [http://localhost:3000](http://localhost:3000) 및 에는 AEM Headless의 GraphQL API를 사용하여 AEM as a Cloud Service에서 가져온 모험 목록이 표시됩니다.

## 5. AEM에서 컨텐츠 편집

AEM Headless GraphQL API의 콘텐츠에 연결하고 콘텐츠를 사용하는 샘플 WKND React 앱을 사용하여 AEM Author 서비스에서 콘텐츠를 작성하고 React 앱의 경험이 함께 어떻게 업데이트되는지 확인합니다.

_단계 스크린캐스트_
>[!VIDEO](https://video.tv.adobe.com/v/339077?quality=12&learn=on)

1. AEM as a Cloud Service Author 서비스에 로그인
1. 다음으로 이동 __Assets > 파일 > WKND 공유 > 영어 > 모험__
1. 를 엽니다. __유타 남부 자전거 경기__ 폴더
1. 다음 항목 선택 __유타 남부 자전거 경기__ 컨텐츠 조각 및 선택 __편집__ 맨 위의 작업 표시줄에서
1. 콘텐츠 조각의 일부 필드를 업데이트합니다. 예:
   + 제목: `Cycling Utah's National Parks`
   + 이동 길이: `6 Days`
   + 어려움: `Intermediate`
   + 가격: `3500`
   + 1차 이미지: `/content/dam/wknd-shared/en/activities/cycling/mountain-biking.jpg`
1. 선택 __저장__ 맨 위의 작업 표시줄에서
1. 선택 __빠른 게시__ 맨 위의 작업 표시줄에서 __...__
1. 실행 중인 React 앱 새로 고침 [http://localhost:3000](http://localhost:3000).
1. React 앱에서 지금 업데이트된 사이클링 어드벤처를 선택하고 콘텐츠 조각에 대한 콘텐츠 변경 사항을 확인합니다.

1. AEM Author 서비스에서 동일한 접근 방식을 사용합니다.
   1. 기존 어드벤처 콘텐츠 조각의 게시를 취소하고 React 앱 경험에서 제거되었는지 확인합니다.
   1. 새 Adventure 콘텐츠 조각을 만들어 게시하고 React 앱 경험에 표시되는지 확인합니다.

   >[!TIP]
   >
   > 새로운 콘텐츠 조각을 만들고 게시하거나 기존 콘텐츠 조각의 게시를 취소하는 데 익숙하지 않은 경우 위의 스크린캐스트를 시청하십시오.

## 축하합니다!

축하합니다! AEM Headless를 사용하여 React 앱을 구동했습니다!

React 앱이 AEM as a Cloud Service의 콘텐츠를 어떻게 사용하는지 자세히 알아보려면 을 확인하십시오 [AEM Headless 자습서](../multi-step/overview.md). 자습서에서는 AEM의 콘텐츠 조각을 만드는 방법과 이 React 앱이 해당 콘텐츠를 JSON으로 사용하는 방법을 살펴봅니다.

### 다음 단계

+ [AEM Headless 튜토리얼 시작](../multi-step/overview.md)
