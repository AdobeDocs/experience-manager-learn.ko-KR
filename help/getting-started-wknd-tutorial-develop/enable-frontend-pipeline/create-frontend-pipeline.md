---
title: 프론트엔드 파이프라인을 사용하여 배포
description: 프론트엔드 리소스를 빌드하고 AEMas a Cloud Service 로 기본 제공 CDN에 배포하는 프론트엔드 파이프라인을 만들고 실행하는 방법에 대해 알아봅니다.
version: Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: d6da05e4-bd65-4625-b9a4-cad8eae3c9d7
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '701'
ht-degree: 0%

---

# 프론트엔드 파이프라인을 사용하여 배포

이 장에서는 Adobe Cloud Manager에서 프론트엔드 파이프라인을 만들고 실행합니다. 다음에서 파일만 빌드합니다. `ui.frontend` 모듈을 만들고 AEMas a Cloud Service 의 기본 제공 CDN에 배포합니다. 따라서 다음에서 멀리 이동합니다.  `/etc.clientlibs` 기반 프론트엔드 리소스 제공.


## 목표 {#objectives}

* 프론트엔드 파이프라인을 만들고 실행합니다.
* 프론트엔드 리소스를에서 제공하지 않는지 확인 `/etc.clientlibs` 다음으로 시작하는 새 호스트 이름에서 `https://static-`

## 프론트엔드 파이프라인 사용

>[!VIDEO](https://video.tv.adobe.com/v/3409420?quality=12&learn=on)

## 사전 요구 사항 {#prerequisites}

이 자습서는 여러 부분으로 구성되어 있으며 다음에 설명된 단계를 가정합니다. [표준 AEM 프로젝트 업데이트](./update-project.md) 완료되었습니다.

다음을 수행했는지 확인 [cloud Manager에서 파이프라인을 만들고 배포할 수 있는 권한](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions) 및 [AEM as a Cloud Service 환경에 액세스](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html).

## 기존 파이프라인 이름 바꾸기

에서 기존 파이프라인 이름 바꾸기 __개발로 배포__ 끝  __개발로 FullStack WKND 배포__ 로 이동 __구성__ 탭 __비프로덕션 파이프라인 이름__ 필드. 이는 파이프라인의 이름만 보고 파이프라인이 전체 스택인지 프론트엔드인지를 명시적으로 하기 위한 것입니다.

![파이프라인 이름 바꾸기](assets/fullstack-wknd-deploy-dev-pipeline.png)


또한에 __소스 코드__ 탭에서 저장소 및 Git 분기 필드 값이 올바르고 분기에 프론트엔드 파이프라인 계약 변경 사항이 있는지 확인합니다.

![소스 코드 구성 파이프라인](assets/fullstack-wknd-source-code-config.png)


## 프론트엔드 파이프라인 만들기

종료 __전용__ 에서 프론트엔드 리소스 구축 및 배포 `ui.frontend` 모듈에서 다음 단계를 수행합니다.

1. Cloud Manager UI의 __파이프라인__ 섹션, 클릭 __추가__ 버튼을 누른 다음 선택 __비프로덕션 파이프라인 추가__ (또는 __프로덕션 파이프라인 추가__)에 배포할 AEM as a Cloud Service 환경을 기반으로 합니다.

1. 다음에서 __비프로덕션 파이프라인 추가__ 대화 상자, 의 일부 __구성__ 단계, 다음을 선택 __배포 파이프라인__ 옵션, 이름을 로 지정합니다. __개발로 프론트엔드 WKND 배포__, 및 클릭 __계속__

![프론트엔드 파이프라인 구성 만들기](assets/create-frontend-pipeline-configs.png)

1. 의 일부로 __소스 코드__ 단계, 다음을 선택 __프론트엔드 코드__ 옵션 및 다음 위치에서 환경 선택 __적합한 배포 환경__. 다음에서 __소스 코드__ 섹션은 저장소 및 Git 분기 필드 값이 올바른지, 분기에 프론트엔드 파이프라인 계약 변경 사항이 있는지 확인합니다.
및 __가장 중요한 것__ 대상: __코드 위치__ 값이 인 필드 `/ui.frontend` 그리고 마지막으로 __저장__.

![프론트엔드 파이프라인 소스 코드 만들기](assets/create-frontend-pipeline-source-code.png)


## 배포 시퀀스

* 먼저 새로 이름이 변경된 을 실행합니다. __개발로 FullStack WKND 배포__ 파이프라인을 통해 AEM 저장소에서 WKND clientlib 파일을 제거할 수 있습니다. 또한 가장 중요한 것은 를 추가하여 AEM에서 프론트엔드 파이프라인 계약을 준비시키는 것입니다. __Sling 구성__ 파일(`SiteConfig`, `HtmlPageItemsConfig`).

![스타일이 지정되지 않은 WKND 사이트](assets/unstyled-wknd-site.png)

>[!WARNING]
>
>이후, __개발로 FullStack WKND 배포__ 파이프라인 완료 시 __스타일이 없음__ WKND Site, 손상된 것처럼 보일 수 있습니다. 가동 중단을 계획하거나 홀수 시간 동안 배포하십시오. 이는 단일 전체 스택 파이프라인을 사용하는 것에서부터 프론트엔드 파이프라인으로 초기 전환 중에 계획해야 하는 1회적인 중단입니다.


* 마지막으로 __개발로 프론트엔드 WKND 배포__ 빌드할 파이프라인 `ui.frontend` 는 프론트엔드 리소스를 CDN에 바로 모듈화하고 배포합니다.

>[!IMPORTANT]
>
>다음과 같은 사실을 알 수 있습니다. __스타일이 없음__ WKND 사이트가 정상으로 돌아왔으며 이번에 __프론트엔드__ 파이프라인 실행은 전체 스택 파이프라인보다 훨씬 빠릅니다.

## 스타일 변경 사항 및 새로운 게재 패러다임 확인

* WKND Site의 아무 페이지나 열면 텍스트 색상이 표시됩니다. __Adobe 레드__ 및 프론트엔드 리소스(CSS, JS) 파일은 CDN에서 전달됩니다. 리소스 요청 호스트 이름이 다음으로 시작 `https://static-pXX-eYY.p123-e456.adobeaemcloud.com/$HASH_VALUE$/theme/site.css` 및 마찬가지로 사이트에서 참조한 site.js 또는 기타 정적 리소스 `HtmlPageItemsConfig` 파일.


![새로 스타일이 지정된 WKND 사이트](assets/newly-styled-wknd-site.png)



>[!TIP]
>
>다음 `$HASH_VALUE$` 다음은 에 표시되는 것과 같습니다. __개발로 프론트엔드 WKND 배포__  파이프라인 __콘텐츠 해시__ 필드. AEM은 프론트엔드 리소스의 CDN URL에 대한 알림을 받고, 값은 다음 위치에 저장됩니다. `/conf/wknd/sling:configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/jcr:content` 아래에 __prefixPath__ 속성.


![해시 값 상관 관계](assets/hash-value-correlartion.png)



## 축하합니다! {#congratulations}

축하합니다. WKND Sites 프로젝트의 &#39;ui.frontend&#39; 모듈만 빌드하고 배포하는 프론트엔드 파이프라인을 만들고 실행하고 확인했습니다. 이제 프론트엔드 팀은 전체 AEM 프로젝트의 수명 주기를 벗어나 사이트의 디자인 및 프론트엔드 동작을 빠르게 반복할 수 있습니다.

## 다음 단계 {#next-steps}

다음 장에서는 [고려 사항](considerations.md)에서는 프론트엔드 및 백엔드 개발 프로세스에 미치는 영향을 검토하게 됩니다.
