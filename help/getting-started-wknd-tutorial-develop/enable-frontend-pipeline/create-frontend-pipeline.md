---
title: 프런트엔드 파이프라인을 사용하여 배포
description: 프런트 엔드 리소스를 빌드하고 AEM의 기본 제공 CDN에 배포하는 프런트 엔드 파이프라인을 만들고 실행하는 방법을 as a Cloud Service으로 알아봅니다.
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '701'
ht-degree: 0%

---


# 프런트엔드 파이프라인을 사용하여 배포

이 장에서는 Adobe Cloud Manager에서 프런트엔드 파이프라인을 만들고 실행합니다. 이 파일은 `ui.frontend` 모듈 및 에서는 두 플러그인을 AEM as a Cloud Service의 기본 CDN에 배포합니다. 따라서  `/etc.clientlibs` 프런트엔드 리소스 제공 기반.


## 목표 {#objectives}

* 프런트엔드 파이프라인을 생성하고 실행합니다.
* 프런트 엔드 리소스에서 제공되지 않았는지 확인합니다. `/etc.clientlibs` 하지만 새 호스트 이름에서 `https://static-`

## 프런트엔드 파이프라인 사용

>[!VIDEO](https://video.tv.adobe.com/v/3409420?quality=12&learn=on)

## 사전 요구 사항 {#prerequisites}

이 내용은 여러 부분으로 구성된 자습서이며 [표준 AEM 프로젝트 업데이트](./update-project.md) 을(를) 완료했습니다.

다음을 확인하십시오 [Cloud Manager에서 파이프라인을 생성하고 배포할 권한](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions) 및 [AEM as a Cloud Service 환경 액세스](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html).

## 기존 파이프라인 이름 바꾸기

기존 파이프라인의 이름을 __개발에 배포__ to  __개발에 전체 스택 WKND 배포__ 다음과 같이 __구성__ 탭 __비프로덕션 파이프라인 이름__ 필드. 파이프라인의 이름만 보고 전체 스택인지 아니면 프런트 엔드인지를 명확하게 하기 위한 것입니다.

![파이프라인 이름 변경](assets/fullstack-wknd-deploy-dev-pipeline.png)


또한 __소스 코드__ 탭에서 Repository 및 Git Branch 필드 값이 올바른지 확인하고 분기에 프런트 엔드 파이프라인 계약 변경 사항이 있는지 확인합니다.

![소스 코드 구성 파이프라인](assets/fullstack-wknd-source-code-config.png)


## 프런트엔드 파이프라인 만들기

종료 __전용__ 에서 프런트엔드 리소스 구축 및 배포 `ui.frontend` 모듈에서 다음 단계를 수행합니다.

1. Cloud Manager UI의 __파이프라인__ 섹션을 클릭합니다. __추가__ 단추를 누른 다음 __비프로덕션 파이프라인 추가__ 또는 __프로덕션 파이프라인 추가__)을 배포할 AEM as a Cloud Service 환경을 기반으로 합니다.

1. 에서 __비프로덕션 파이프라인 추가__ 대화 상자의 일부로 __구성__ 단계, __배포 파이프라인__ 옵션, 이름을 __개발에 프런트 엔드 WKND 배포__&#x200B;를 클릭하고 __계속__

![프런트 엔드 파이프라인 구성 만들기](assets/create-frontend-pipeline-configs.png)

1. 의 일부로 __소스 코드__ 단계, __프런트 엔드 코드__ 옵션을 선택하고 __적합한 배포 환경__. 에서 __소스 코드__ 섹션에서 리포지토리 및 Git 분기 필드 값이 올바르고 분기에 프런트 엔드 파이프라인 계약 변경 사항이 있는지 확인합니다.
및 __가장 중요하__ 대상 __코드 위치__ 필드: `/ui.frontend` 마지막으로 __저장__.

![프런트 엔드 파이프라인 소스 코드 만들기](assets/create-frontend-pipeline-source-code.png)


## 배포 시퀀스

* 먼저 새로 이름을 바꾼 __개발에 전체 스택 WKND 배포__ AEM 저장소에서 WKND clientlib 파일을 제거하는 파이프라인입니다. 그리고 가장 중요한 것은, __Sling 구성__ 파일(`SiteConfig`, `HtmlPageItemsConfig`).

![스타일이 지정되지 않은 WKND 사이트](assets/unstyled-wknd-site.png)

>[!WARNING]
>
>이후, __개발에 전체 스택 WKND 배포__ 파이프라인 완료 __스타일이 지정되지 않음__ WKND Site가 손상되었을 수 있습니다. 단 한 번의 전체 스택 파이프라인에서 프런트 엔드 파이프라인으로 첫 번째 스위치 동안 계획해야 하는 일회성 중단입니다.


* 마지막으로 __개발에 프런트 엔드 WKND 배포__ 파이프라인만 빌드 `ui.frontend` 프런트엔드 리소스를 CDN에 직접 모듈 및 배포합니다.

>[!IMPORTANT]
>
>이 경우 __스타일이 지정되지 않음__ WKND 사이트가 정상으로 돌아왔고 이번에는 __프런트엔드__ 파이프라인 실행이 전체 스택 파이프라인보다 훨씬 빠릅니다.

## 스타일 변경 및 새 게재 패러다임을 확인합니다

* WKND Site의 모든 페이지를 열면 텍스트 색상을 볼 수 있습니다. __Adobe 빨간색__ 및 프런트 엔드 리소스(CSS, JS) 파일은 CDN에서 전달됩니다. 리소스 요청 호스트 이름은 `https://static-pXX-eYY.p123-e456.adobeaemcloud.com/$HASH_VALUE$/theme/site.css` 마찬가지로, site.js 또는 `HtmlPageItemsConfig` 파일.


![새로 스타일이 지정된 WKND 사이트](assets/newly-styled-wknd-site.png)



>[!TIP]
>
>다음 `$HASH_VALUE$` 다음은 사용자가 __개발에 프런트 엔드 WKND 배포__  파이프라인 __컨텐츠 해시__ 필드. AEM은 프런트 엔드 리소스의 CDN URL에 대한 알림을 수신하며, 값은 에 저장됩니다. `/conf/wknd/sling:configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/jcr:content` 아래에 __prefixPath__ 속성을 사용합니다.


![해시 값 상관 관계](assets/hash-value-correlartion.png)



## 축하합니다! {#congratulations}

축하합니다. WKND Sites 프로젝트의 &#39;ui.frontend&#39; 모듈만 빌드하고 배포하는 프런트 엔드 파이프라인을 만들고, 실행하고, 확인했습니다. 이제 프런트 엔드 팀은 전체 AEM 프로젝트의 수명 주기 외에도 사이트의 디자인과 프런트 엔드 동작을 빠르게 반복할 수 있습니다.

## 다음 단계 {#next-steps}

다음 장에서 [고려 사항](considerations.md)프런트엔드 및 백엔드 개발 프로세스에 미치는 영향을 검토할 수 있습니다.
