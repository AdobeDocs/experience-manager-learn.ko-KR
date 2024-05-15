---
title: 테마 설정 워크플로 | AEM 빠른 사이트 생성
description: Adobe Experience Manager 사이트의 테마 소스를 업데이트하여 브랜드별 스타일을 적용하는 방법에 대해 알아봅니다. 프록시 서버를 사용하여 CSS 및 Javascript 업데이트의 실시간 미리 보기를 보는 방법에 대해 알아봅니다. 이 튜토리얼에서는 Adobe Cloud Manager의 프론트엔드 파이프라인을 사용하여 AEM 사이트에 테마 업데이트를 배포하는 방법에 대해서도 설명합니다.
version: Cloud Service
feature: Core Components
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-7498
thumbnail: KT-7498.jpg
doc-type: Tutorial
exl-id: 98946462-1536-45f9-94e2-9bc5d41902d4
recommendations: noDisplay, noCatalog
duration: 1275
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '458'
ht-degree: 1%

---

# 테마 설정 워크플로 {#theming}

이 장에서는 브랜드별 스타일을 적용하기 위해 Adobe Experience Manager 사이트의 테마 소스를 업데이트합니다. 라이브 사이트에 대해 코딩할 때 프록시 서버를 사용하여 CSS 및 Javascript 업데이트의 미리보기를 보는 방법에 대해 알아봅니다. 이 튜토리얼에서는 Adobe Cloud Manager의 프론트엔드 파이프라인을 사용하여 AEM 사이트에 테마 업데이트를 배포하는 방법에 대해서도 설명합니다.

결국 당사 사이트는 WKND 브랜드와 일치하는 스타일을 포함하도록 업데이트됩니다.

## 사전 요구 사항 {#prerequisites}

이 자습서는 여러 부분으로 구성되어 있으며 다음에 설명된 단계를 가정합니다. [페이지 템플릿](./page-templates.md) 챕터가 완료되었습니다.

## 목표

1. 사이트의 테마 소스를 다운로드하고 수정하는 방법에 대해 알아봅니다.
1. 실시간 미리 보기를 위해 코드를 라이브 사이트에 적용하는 방법에 대해 알아봅니다.
1. Adobe Cloud Manager의 프론트엔드 파이프라인을 사용하여 컴파일된 CSS 및 JavaScript 업데이트를 테마의 일부로 전달하는 종단간 워크플로를 이해합니다.

## 테마 업데이트 {#theme-update}

다음으로, 사이트가 WKND 브랜드의 모양과 느낌과 일치하도록 테마 소스를 변경합니다.

>[!VIDEO](https://video.tv.adobe.com/v/332918?quality=12&learn=on)

비디오의 높은 수준 단계:

1. 프록시 개발 서버와 함께 사용할 AEM에서 로컬 사용자를 만듭니다.
1. AEM에서 테마 소스를 다운로드하고 VSCode와 같은 로컬 IDE를 사용하여 엽니다.
1. 테마 소스를 수정하고 프록시 개발 서버를 사용하여 CSS 및 JavaScript 변경 내용을 실시간으로 미리 봅니다.
1. 잡지 기사가 WKND 브랜드 스타일 및 모형과 일치하도록 테마 소스를 업데이트합니다.

### 솔루션 파일

의 완료된 스타일 다운로드 [WKND 샘플 테마](assets/theming/WKND-THEME-src-1.1.zip)

## 프론트엔드 파이프라인을 사용하여 테마 배포 {#deploy-theme}

Cloud Manager의 프론트엔드 파이프라인을 사용하여 AEM 환경에 테마에 대한 업데이트를 배포합니다.

>[!VIDEO](https://video.tv.adobe.com/v/338722?quality=12&learn=on)

비디오의 높은 수준 단계:

1. 새 git 만들기 [cloud Manager의 저장소](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/managing-code/cloud-manager-repositories.html)
1. Cloud Manager git 저장소에 테마 소스 프로젝트를 추가합니다.

   ```shell
   $ cd <PATH_TO_THEME_SOURCES_FOLDER>
   $ git init -b main
   $ git add .
   $ git commit -m "initial commit"
   $ git remote add origin <CLOUD_MANAGER_GIT_REPOSITORY_URL>
   ```

1. 구성 [프론트엔드 파이프라인](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html) 프론트엔드 코드를 배포하려면 Cloud Manager를 사용하십시오.
1. 프론트엔드 파이프라인을 실행하여 타겟 AEM 환경에 업데이트를 배포합니다.

### 보고서 예

참조로 사용할 수 있는 두 가지 예제 GitHub 보고서가 있습니다.

* [aem-site-template-standard](https://github.com/adobe/aem-site-template-standard)
* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e) - &quot;실제&quot; 프로젝트의 예로 사용됩니다.

## 축하합니다! {#congratulations}

축하합니다. 방금 AEM에 테마를 업데이트하고 배포했습니다!

### 다음 단계 {#next-steps}

AEM 개발에 대해 자세히 알아보고 를 사용하여 사이트를 만들어 기본 기술에 대해 더 많이 이해합니다. [AEM Project Archetype](../project-archetype/overview.md).
