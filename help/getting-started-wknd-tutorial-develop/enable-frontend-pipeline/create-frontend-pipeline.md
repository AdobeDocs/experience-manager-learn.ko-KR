---
title: 프론트엔드 파이프라인을 사용하여 배포
description: AEM as a Cloud Service의 기본 제공 CDN에 프론트엔드 리소스를 빌드하고 배포하는 프론트엔드 파이프라인을 생성 및 실행하는 방법을 알아봅니다.
version: Experience Manager as a Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: d6da05e4-bd65-4625-b9a4-cad8eae3c9d7
duration: 225
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '685'
ht-degree: 100%

---

# 프론트엔드 파이프라인을 사용하여 배포

이 챕터에서는 Adobe Cloud Manager에서 프론트엔드 파이프라인을 만들고 실행합니다. 이때 `ui.frontend` 모듈의 파일만 빌드하며 이 파일들을 AEM as a Cloud Service에서 기본 제공되는 CDN에 배포합니다. 따라서 `/etc.clientlibs` 기반 프론트엔드 리소스 게재를 사용하지 않게 됩니다.


## 목표 {#objectives}

* 프론트엔드 파이프라인을 생성
* 프론트엔드 리소스가 `/etc.clientlibs`가 아닌 `https://static-`으로 시작하는 새로운 호스트 이름에서 제공되는지 확인

## 프론트엔드 파이프라인 사용

>[!VIDEO](https://video.tv.adobe.com/v/3409420?quality=12&learn=on)

## 사전 요구 사항 {#prerequisites}

여러 부분으로 구성된 튜토리얼로, [표준 AEM Project 업데이트](./update-project.md)에 개괄된 단계가 완료되었다고 가정합니다.

[Cloud Manager에서 파이프라인을 생성 및 배포할 수 있는 권한](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=ko#role-definitions) 및 [AEM as a Cloud Service 환경에 액세스할 수 있는 권한](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html)이 있는지 확인합니다.

## 기존 파이프라인 이름 재설정

__구성__&#x200B;탭의 __비프로덕션 파이프라인 이름__ 필드로 이동하여 기존 파이프라인의 이름을 __개발 배포__&#x200B;에서 __풀스택 WKND 개발 배포__&#x200B;로 재설정합니다. 이는 파이프라인의 이름만 보고도 파이프라인이 풀스택인지 프론트엔드인지 명확하게 알 수 있도록 하기 위함입니다.

![파이프라인 이름 재설정](assets/fullstack-wknd-deploy-dev-pipeline.png)


또한 __소스 코드__ 탭에서 저장소 및 Git 분기 필드 값이 올바른지, 분기에 프론트엔드 파이프라인 계약 변경 사항이 있는지 확인합니다.

![소스 코드 구성 파이프라인](assets/fullstack-wknd-source-code-config.png)


## 프론트엔드 파이프라인 생성

`ui.frontend` 모듈에서 프론트엔드 리소스&#x200B;__만__ 빌드 및 배포하려면 아래의 단계를 수행합니다.

1. __파이프라인__ 섹션의 Cloud Manager UI에서 __추가__ 버튼을 클릭한 다음 배포를 원하는 AEM as a Cloud Service 환경에 따라 __비프로덕션 파이프라인 추가__(또는 __프로덕션 파이프라인 추가__)를 선택합니다.

1. __비프로덕션 파이프라인__ 대화 상자에서 __구성__ 단계의 일부로서 __배포 파이프라인__ 옵션을 선택한 다음 __프론트엔드 WKND 배포 개발__&#x200B;이라 명명하고 __계속__&#x200B;을 클릭합니다.

![프론트엔드 파이프라인 구성 생성](assets/create-frontend-pipeline-configs.png)

1. __소스 코드__ 단계의 일부로서 __프론트엔드 코드__ 옵션을 선택하고 __적합한 배포 환경__&#x200B;에서 환경을 선택합니다. __소스 코드__ 섹션에서 저장소 및 Git 분기 필드 값이 올바른지, 분기에 프론트엔드 파이프라인 계약 변경 사항이 있는지 확인합니다.
__코드 위치__ 필드와 관련하여 __가장 중요한 점__&#x200B;은 값이 `/ui.frontend`라는 점입니다. 마지막으로 __저장__&#x200B;을 클릭합니다.

![프론트엔드 파이프라인 소스 코드 생성](assets/create-frontend-pipeline-source-code.png)


## 배포 순서

* 먼저 새롭게 이름이 바뀐 __풀스택 WKND 배포 개발__ 파이프라인을 실행하여 AEM 저장소에서 WKND clientlib 파일을 제거합니다. 가장 중요한 점은 Sling __Sling 구성__ 파일(`SiteConfig`, `HtmlPageItemsConfig`)을 추가하여 프론트엔드 파이프라인 계약에 대한 AEM을 준비하는 것입니다.

![스타일이 지정되지 않은 WKND 사이트](assets/unstyled-wknd-site.png)

>[!WARNING]
>
>__풀스택 WKND 개발 배포__ 파이프라인이 완료되면 __스타일이 지정되지 않은__ WKND 사이트가 생성되며 이 사이트는 손상된 것처럼 보일 수 있습니다. 중단이나 일반적이지 않은 시각에 배포하는 것을 계획하시기 바랍니다. 이는 단일 풀스택 파이프라인에서 프론트엔드 파이프라인으로 처음 전환하는 동안 계획해야 하는 일회성 중단입니다.


* 마지막으로, __프론트엔드 WKND 개발 배포__ 파이프라인을 실행하여 `ui.frontend` 모듈만 빌드하고 프론트엔드 리소스를 CDN에 직접 배포합니다.

>[!IMPORTANT]
>
>__스타일이 지정되지 않은__ WKND 사이트가 정상으로 돌아왔고 이번에는 __프론트엔드__ 파이프라인 실행이 풀스택 파이프라인보다 훨씬 빠른 것을 알 수 있습니다.

## 스타일 변경 및 새로운 게재 패러다임 확인

* WKND 사이트의 아무 페이지에서나 열면 텍스트 색상이 __Adobe Red__&#x200B;로 표시되고 프론트엔드 리소스(CSS, JS) 파일은 CDN에서 제공됩니다. 리소스 요청 호스트 이름은 `https://static-pXX-eYY.p123-e456.adobeaemcloud.com/$HASH_VALUE$/theme/site.css`로 시작하며, `HtmlPageItemsConfig` 파일에서 참조한 site.js 또는 다른 정적 리소스도 마찬가지입니다.


![새롭게 스타일이 지정된 WKND 사이트](assets/newly-styled-wknd-site.png)



>[!TIP]
>
>`$HASH_VALUE$` 또한 __프론트엔드 WKND 개발 배포__ 파이프라인의 __콘텐츠 해시__ 필드와 동일합니다. AEM은 프론트엔드 리소스의 CDN URL에 대한 알림을 받고, 해당 값은 __prefixPath__ 속성 아래의 `/conf/wknd/sling:configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/jcr:content`에 저장됩니다.


![해시 값 상관 관계](assets/hash-value-correlartion.png)



## 축하합니다! {#congratulations}

축하합니다. WKND Sites 프로젝트의 &#39;ui.frontend&#39; 모듈만 빌드하고 배포하는 프론트엔드 파이프라인을 생성, 실행 및 검증했습니다. 이제 프론트엔드 팀은 AEM 프로젝트의 전체 수명 주기를 벗어나 사이트의 디자인과 프론트엔드 동작을 빠르게 반복할 수 있습니다.

## 다음 단계 {#next-steps}

다음 챕터인 [고려 사항](considerations.md)에서는 프론트엔드와 백엔드 개발 프로세스에 미치는 영향을 검토합니다.
