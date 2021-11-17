---
title: 모니터링 워크플로우 | AEM 빠른 사이트 만들기
description: Adobe Experience Manager 사이트의 테마 소스를 업데이트하여 브랜드별 스타일을 적용하는 방법을 알아봅니다. 프록시 서버를 사용하여 CSS 및 Javascript 업데이트의 라이브 미리 보기를 보는 방법을 알아봅니다. 또한 이 자습서에서는 Adobe Cloud Manager의 프런트 엔드 파이프라인을 사용하여 AEM 사이트에 테마 업데이트를 배포하는 방법을 다룹니다.
sub-product: sites
version: Cloud Service
type: Tutorial
feature: Core Components
topic: Content Management, Development
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
exl-id: 98946462-1536-45f9-94e2-9bc5d41902d4
source-git-commit: 04096fe3c99cdcce2d43b2b29899c2bbe37ac056
workflow-type: tm+mt
source-wordcount: '515'
ht-degree: 0%

---

# 모니터링 워크플로우 {#theming}

>[!CAUTION]
>
> 빠른 사이트 만들기 도구는 현재 기술 미리 보기입니다. 테스트 및 평가 목적으로 사용할 수 있으며, Adobe 지원에 동의하지 않는 프로덕션 용도에는 사용할 수 없습니다.

이 장에서는 Adobe Experience Manager 사이트의 테마 소스를 업데이트하여 브랜드별 스타일을 적용합니다. 라이브 사이트에 대해 코드를 지정하는 동안 프록시 서버를 사용하여 CSS 및 Javascript 업데이트 미리 보기를 보는 방법을 배웁니다. 또한 이 자습서에서는 Adobe Cloud Manager의 프런트 엔드 파이프라인을 사용하여 AEM 사이트에 테마 업데이트를 배포하는 방법을 다룹니다.

마지막에 WKND 브랜드에 일치하는 스타일을 포함하도록 사이트가 업데이트됩니다.

## 전제 조건 {#prerequisites}

이 내용은 여러 부분으로 구성된 자습서이며 [페이지 템플릿](./page-templates.md) 장이 완료되었습니다.

## 목표

1. 사이트의 테마 소스를 다운로드하여 수정하는 방법을 알아봅니다.
1. 실시간 미리 보기를 위해 라이브 사이트에 대해 코드가 실행되는 방법을 알아봅니다.
1. Cloud Manager의 프런트 엔드 파이프라인을 사용하여 컴파일된 CSS 및 JavaScript 업데이트를 테마의 일부로 전달하는 종단 간 워크플로우를 이해합니다.

## 테마 업데이트 {#theme-update}

다음으로, 사이트가 WKND 브랜드의 모양과 느낌과 일치하도록 테마 소스를 변경합니다.

>[!VIDEO](https://video.tv.adobe.com/v/332918/?quality=12&learn=on)

비디오에 대한 높은 수준 단계:

1. 프록시 개발 서버에서 사용할 AEM에서 로컬 사용자를 생성합니다.
1. AEM에서 테마 소스를 다운로드하고 VSCode와 같은 로컬 IDE를 사용하여 엽니다.
1. 테마 소스를 수정하고 프록시 개발 서버를 사용하여 CSS 및 JavaScript 변경 사항을 실시간으로 미리 봅니다.
1. 잡지 문서가 WKND 브랜드 스타일과 모집단 매트릭스와 일치하도록 테마 소스를 업데이트합니다.

### 솔루션 파일

에 대한 완성된 스타일을 다운로드합니다. [WKND 샘플 테마](assets/theming/WKND-THEME-src-1.1.zip)

## 프런트엔드 파이프라인을 사용하여 테마 배포 {#deploy-theme}

Cloud Manager의 프런트 엔드 파이프라인을 사용하여 AEM 환경에 테마에 대한 업데이트를 배포합니다.

>[!VIDEO](https://video.tv.adobe.com/v/338722/?quality=12&learn=on)

비디오에 대한 높은 수준 단계:

1. 새 Git 만들기 [Cloud Manager의 저장소](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/managing-code/cloud-manager-repositories.html)
1. 테마 소스 프로젝트를 Cloud Manager git 저장소에 추가합니다.

   ```shell
   $ cd <PATH_TO_THEME_SOURCES_FOLDER>
   $ git init -b main
   $ git add .
   $ git commit -m "initial commit"
   $ git remote add origin <CLOUD_MANAGER_GIT_REPOSITORY_URL>
   ```

1. 구성 [프런트엔드 파이프라인](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html) ( Cloud Manager 를 사용하여 프런트 엔드 코드를 배포합니다.)
1. 프런트 엔드 파이프라인을 실행하여 타겟 AEM 환경에 업데이트를 배포합니다.

### 보고서 예

참조로 사용할 수 있는 두 가지 GitHub 보고서 예는 다음과 같습니다.

* [aem-site-template-standard](https://github.com/adobe/aem-site-template-standard)
* [aem-site-template-basic-테마-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e) - &quot;실제&quot; 프로젝트의 예로 사용됩니다.

## 축하합니다! {#congratulations}

축하합니다. 방금 업데이트된 테마를 AEM에 배포했습니다!

### 다음 단계 {#next-steps}

를 사용하여 사이트를 제작하여 AEM 개발에 대해 자세히 알아보고 기본 기술을 더 많이 이해합니다 [AEM 프로젝트 원형](../project-archetype/overview.md).
