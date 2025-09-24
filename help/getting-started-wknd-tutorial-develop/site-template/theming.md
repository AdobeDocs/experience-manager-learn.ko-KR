---
title: 테마 설정 워크플로 | AEM 빠른 사이트 생성
description: Adobe Experience Manager 사이트의 테마 소스를 업데이트하여 브랜드별 스타일을 적용하는 방법을 알아봅니다. CSS와 Javascript 업데이트의 라이브 미리보기를 보기 위해 프록시 서버를 사용하는 방법을 알아봅니다. 이 튜토리얼은 Adobe Cloud Manager의 프론트엔드 파이프라인을 사용하여 AEM 사이트에 테마 업데이트를 배포하는 방법도 다룹니다.
version: Experience Manager as a Cloud Service
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '458'
ht-degree: 100%

---

# 테마 지정 워크플로 {#theming}

이 챕터에서는 Adobe Experience Manager 사이트의 테마 소스를 업데이트하여 브랜드별 스타일을 적용합니다. 라이브 사이트에서 코드를 작성하는 동안 CSS와 Javascript 업데이트를 미리 보기 위해 프록시 서버를 사용하는 방법을 알아봅니다. 이 튜토리얼은 Adobe Cloud Manager의 프론트엔드 파이프라인을 사용하여 AEM 사이트에 테마 업데이트를 배포하는 방법도 다룹니다.

결과적으로 당사 사이트는 WKND 브랜드에 맞는 스타일을 포함하도록 업데이트되었습니다.

## 사전 요구 사항 {#prerequisites}

이 튜토리얼은 여러 부분으로 구성되어 있으며, [페이지 템플릿](./page-templates.md) 챕터에 설명된 단계가 완료되었다고 가정합니다.

## 목표

1. 사이트의 테마 소스를 다운로드하고 수정하는 방법을 알아봅니다.
1. 실시간 미리보기를 위해 라이브 사이트에 대한 코드를 작성하는 방법을 알아봅니다.
1. Adobe Cloud Manager의 프론트엔드 파이프라인을 사용하여 테마의 일부로 컴파일된 CSS 및 JavaScript 업데이트를 제공하는 엔드 투 엔드 워크플로를 이해합니다.

## 테마 업데이트 {#theme-update}

다음으로 테마 소스를 변경하여 사이트가 WKND 브랜드의 모양과 느낌에 맞게 구성되도록 합니다.

>[!VIDEO](https://video.tv.adobe.com/v/332918?quality=12&learn=on)

비디오에 대한 상위 수준 단계:

1. 프록시 개발 서버와 함께 사용할 AEM에 로컬 사용자를 만듭니다.
1. AEM에서 테마 소스를 다운로드하고 VSCode와 같은 로컬 IDE를 사용하여 엽니다.
1. 테마 소스를 수정하고 프록시 개발 서버를 사용하여 CSS 및 JavaScript 변경 사항을 실시간으로 미리 봅니다.
1. 테마 소스를 업데이트하여 매거진 문서가 WKND 브랜드 스타일 및 목업과 일치하도록 합니다.

### 솔루션 파일

[WKND 샘플 테마](assets/theming/WKND-THEME-src-1.1.zip)에 대해 완성된 스타일 다운로드

## 프론트엔드 파이프라인을 사용하여 테마 배포 {#deploy-theme}

Cloud Manager의 프론트엔드 파이프라인을 사용하여 테마 업데이트를 AEM 환경에 배포합니다.

>[!VIDEO](https://video.tv.adobe.com/v/338722?quality=12&learn=on)

비디오에 대한 상위 수준 단계:

1. [Cloud Manager의 저장소](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/managing-code/cloud-manager-repositories.html)에 새로운 git 생성
1. 테마 소스 프로젝트를 Cloud Manager git 저장소에 추가

   ```shell
   $ cd <PATH_TO_THEME_SOURCES_FOLDER>
   $ git init -b main
   $ git add .
   $ git commit -m "initial commit"
   $ git remote add origin <CLOUD_MANAGER_GIT_REPOSITORY_URL>
   ```

1. Cloud Manager에서 [프론트엔드 파이프라인](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html)을 구성하여 프론트엔드 코드를 배포하도록 합니다.
1. 프론트엔드 파이프라인을 실행하여 대상 AEM 환경에 업데이트를 배포합니다.

### 저장소 예시

참조할 수 있는 몇 가지 GitHub 저장소 예시는 다음과 같습니다.

* [aem-site-template-standard](https://github.com/adobe/aem-site-template-standard)
* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e) - &#39;실제&#39; 프로젝트의 예시로 사용됩니다.

## 축하합니다! {#congratulations}

축하합니다! AEM에 테마를 업데이트하고 배포했습니다!

### 다음 단계 {#next-steps}

[AEM Project Archetype](../project-archetype/overview.md)을 사용하여 사이트를 생성함으로써 AEM 개발을 심층적으로 이해하고 기본 기술을 파악합니다.
