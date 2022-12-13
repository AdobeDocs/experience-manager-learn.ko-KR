---
title: AEM 컨텐츠 조각 콘솔 확장 배포
description: AEM 컨텐츠 조각 콘솔 확장을 배포하는 방법을 알아봅니다.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: a7b32982b547eb292384d2ebde80ba745091702a
workflow-type: tm+mt
source-wordcount: '802'
ht-degree: 0%

---


# 확장 배포

AEM as a Cloud Service 환경에서 사용하려면 확장 앱 빌더 앱을 배포하고 승인해야 합니다.

확장 App Builder 앱을 배포할 때 고려해야 할 몇 가지 고려 사항이 있습니다.

+ 확장이 Adobe Developer 콘솔 프로젝트 작업 공간에 배포됩니다. 기본 작업 공간은 다음과 같습니다.
   + __프로덕션__ 작업 공간에는 모든 AEM as a Cloud Service에서 사용할 수 있는 확장 배포가 포함되어 있습니다.
   + __단계__ 작업 공간은 개발자 작업 공간으로 사용됩니다. 스테이지 작업 영역에 배포된 확장은 AEM as a Cloud Service에서 사용할 수 없습니다.
Adobe Developer 콘솔 작업 공간에는 AEM as a Cloud Service 환경 유형과 직접적인 상관관계가 없습니다.
+ 프로덕션 작업 공간에 배포된 확장은 확장이 있는 Adobe 조직의 모든 AEM as a Cloud Service 환경에 표시됩니다.
확장은 추가 [AEM as a Cloud Service 호스트 이름을 확인하는 조건부 논리](https://developer.adobe.com/uix/docs/guides/publication/#enabling-extension-only-on-specific-aem-environments).
+ AEM as a Cloud Service에서 여러 확장을 사용할 수 있습니다. Adobe은 각 확장 앱 빌더 앱을 사용하여 단일 비즈니스 목표를 해결할 것을 권장합니다. 즉, 단일 확장 앱 빌더 앱은 일반적인 비즈니스 목표를 지원하는 여러 확장 지점을 구현할 수 있습니다.

## 초기 배포

확장 프로그램을 AEM as a Cloud Service 환경에서 사용하려면 Adobe Developer 콘솔에 배포해야 합니다.

배포 프로세스는 다음 두 가지 논리 단계로 분할됩니다.

1. 개발자가 Adobe Developer Console에 확장 App Builder 앱 배포.
1. 배포 관리자 또는 비즈니스 소유자가 확장을 승인합니다.

### 확장 앱 빌더 앱 배포

확장을 프로덕션 작업 공간에 배포합니다. Production Workspace에 배포된 확장은 확장이 배포되는 Adobe 조직의 모든 AEM as a Cloud Service 작성자 서비스에 자동으로 추가됩니다.

1. 업데이트된 확장 앱 빌더 앱의 루트에 대한 명령줄을 엽니다.
1. 프로덕션 작업 공간이 활성화되어 있는지 확인합니다

   ```shell
   $ aio app use -w Production
   ```

   변경 내용 병합 `.env` 및 `.aio`.

1. 업데이트된 확장 앱 빌더 앱을 배포합니다.

   ```shell
   $ aio app deploy
   ```

#### 배포 승인 요청

![승인을 위해 확장 제출](./assets/deploy/submit-for-approval.png){align="center"}

1. 에 로그인합니다. [Adobe Developer 콘솔](https://developer.adobe.com)
1. 선택 __콘솔__
1. 다음으로 이동 __프로젝트__
1. 확장과 연결된 프로젝트를 선택합니다
1. 을(를) 선택합니다 __프로덕션__ 작업 영역
1. 선택 __승인을 위한 제출__
1. 양식을 작성하여 제출하면 필요에 따라 필드를 업데이트합니다.

+ 아이콘이 필요합니다. 아이콘이 없는 경우 [이 아이콘](./assets/deploy/icon.png).

### 배포 요청 승인

![확장 승인](./assets/deploy/adobe-exchange.png){align="center"}

1. 에 로그인합니다. [Adobe 교환](https://exchange.adobe.com/)
1. 다음으로 이동 __관리__ > __앱 보류 중인 검토__
1. __검토__ 확장 앱 빌더 앱
1. 확장 변경이 허용되는 경우 __수락__ 리뷰 이렇게 하면 Adobe 조직 내의 모든 AEM as a Cloud Service 작성자 서비스에 확장이 즉시 삽입됩니다.

확장 요청이 승인되면 확장은 AEM as a Cloud Service 작성자 서비스에서 즉시 활성화됩니다.

## 확장 업데이트

App Builder 앱 업데이트 및 확장 프로그램은 [초기 배포](#initial-deployment)인 경우, 기존 확장 배포를 먼저 취소해야 합니다.

### 확장 취소

새 버전의 확장을 배포하려면 먼저 취소(또는 제거)해야 합니다. 확장은 취소됨이지만 AEM 콘솔에서 사용할 수 없습니다.

1. 에 로그인합니다. [Adobe 교환](https://exchange.adobe.com/)
1. 다음으로 이동 __관리__ > __App Builder 앱__
1. __취소__ 업데이트할 확장

### 확장 배포

확장을 프로덕션 작업 공간에 배포합니다. Production Workspace에 배포된 확장은 확장이 배포되는 Adobe 조직의 모든 AEM as a Cloud Service 작성자 서비스에 자동으로 추가됩니다.

1. 업데이트된 확장 앱 빌더 앱의 루트에 대한 명령줄을 엽니다.
1. 프로덕션 작업 공간이 활성화되어 있는지 확인합니다

   ```shell
   $ aio app use -w Production
   ```

   변경 내용 병합 `.env` 및 `.aio`.

1. 업데이트된 확장 앱 빌더 앱을 배포합니다.

   ```shell
   $ aio app deploy
   ```

#### 배포 승인 요청

![승인을 위해 확장 제출](./assets/deploy/submit-for-approval.png){align="center"}

1. 에 로그인합니다. [Adobe Developer 콘솔](https://developer.adobe.com)
1. 선택 __콘솔__
1. 다음으로 이동 __프로젝트__
1. 확장과 연결된 프로젝트를 선택합니다
1. 을(를) 선택합니다 __프로덕션__ 작업 영역
1. 선택 __승인을 위한 제출__
1. 양식을 작성하여 제출하면 필요에 따라 필드를 업데이트합니다.

#### 배포 요청 승인

![확장 승인](./assets/deploy/adobe-exchange.png){align="center"}

1. 에 로그인합니다. [Adobe 교환](https://exchange.adobe.com/)
1. 다음으로 이동 __관리__ > __앱 보류 중인 검토__
1. __검토__ 확장 앱 빌더 앱
1. 확장 변경이 허용되는 경우 __수락__ 리뷰 이렇게 하면 Adobe 조직 내의 모든 AEM as a Cloud Service 작성자 서비스에 확장이 즉시 삽입됩니다.

확장 요청이 승인되면 확장은 AEM as a Cloud Service 작성자 서비스에서 즉시 활성화됩니다.

## 확장 제거

![확장 제거](./assets/deploy/revoke.png)

확장을 제거하려면 Adobe Exchange에서 확장을 취소(또는 제거)합니다. 확장이 취소되면 모든 AEM as a Cloud Service 작성자 서비스에서 제거됩니다.

1. 에 로그인합니다. [Adobe 교환](https://exchange.adobe.com/)
1. 다음으로 이동 __관리__ > __App Builder 앱__
1. __취소__ 제거할 확장
