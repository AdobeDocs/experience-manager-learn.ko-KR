---
title: 테마 워크플로우
seo-title: AEM Sites 시작하기 - 테마 워크플로우
description: Adobe Experience Manager 사이트의 테마 소스를 업데이트하여 브랜드별 스타일을 적용하는 방법을 살펴봅니다. 프록시 서버를 사용하여 CSS 및 Javascript 업데이트에 대한 실시간 미리 보기를 보는 방법을 살펴봅니다. 이 자습서에서는 GitHub 동작을 사용하여 AEM 사이트에 테마 업데이트를 배포하는 방법도 다룹니다.
sub-product: 사이트
version: Cloud Service
type: Tutorial
feature: 코어 구성 요소
topic: 컨텐츠 관리, 개발
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '581'
ht-degree: 0%

---


# 작업 과정 {#theming}

>[!CAUTION]
>
> 여기에 소개된 빠른 사이트 제작 기능은 2021년 하반기에 출시될 예정입니다. 관련 설명서는 미리 보기 목적으로 사용할 수 있습니다.

이 장에서는 Adobe Experience Manager 사이트의 테마 소스를 업데이트하여 브랜드 전용 스타일을 적용할 수 있습니다. 라이브 사이트에 대해 코딩할 때 프록시 서버를 사용하여 CSS 및 Javascript 업데이트 미리 보기를 보는 방법을 알아봅니다. 이 자습서에서는 GitHub 동작을 사용하여 AEM 사이트에 테마 업데이트를 배포하는 방법도 다룹니다.

마지막으로 WKND 브랜드에 맞는 스타일을 포함하도록 사이트를 업데이트할 예정입니다.

## 전제 조건 {#prerequisites}

이 자습서는 다중 부분으로 구성된 자습서이며 [페이지 템플릿](./page-templates.md) 장에 설명된 단계가 완료되었다고 가정합니다.

## 목표

1. 사이트의 테마 소스를 다운로드하고 수정할 수 있는 방법을 알아봅니다.
1. 실시간 미리 보기를 위해 라이브 사이트에서 코드를 작성하는 방법을 살펴볼 수 있습니다.
1. GitHub 동작을 사용하여 컴파일된 CSS 및 JavaScript 업데이트를 테마의 일부로 전달하는 엔드 투 엔드 워크플로우를 파악할 수 있습니다.

## 테마 {#theme-update} 업데이트

다음으로, 사이트가 WKND 브랜드의 모양과 느낌에 맞도록 테마 소스를 변경합니다.

>[!VIDEO](https://video.tv.adobe.com/v/332918/?quality=12&learn=on)

비디오에 대한 높은 단계:

1. 프록시 개발 서버에서 사용할 로컬 사용자를 AEM에서 만듭니다.
1. AEM에서 테마 소스를 다운로드하고 VSCode와 같은 로컬 IDE를 사용하여 엽니다.
1. 테마 소스를 수정하고 프록시 개발 서버를 사용하여 실시간으로 CSS 및 JavaScript 변경 사항을 미리 볼 수 있습니다.
1. 잡지 아티클이 WKND 브랜드 스타일 및 목업과 일치하도록 테마 소스를 업데이트합니다.

### 솔루션 파일

[WKND 테마](assets/theming/WKND-THEME-src.zip)에 대해 완료된 스타일을 다운로드합니다.

## 테마 {#deploy-theme} 배포

GitHub 동작을 사용하여 테마에 대한 업데이트를 AEM 환경에 배포합니다.

>[!VIDEO](https://video.tv.adobe.com/v/332919/?quality=12&learn=on)

비디오에 대한 높은 단계:

1. 테마 소스 프로젝트를 [GitHub에 새 저장소](https://docs.github.com/en/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line)로 추가합니다.
1. GitHub](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token)에서 개인 액세스 토큰을 만들고 보안 위치에 저장합니다.[
1. AEM 환경에서 GitHub 리포지토리를 가리키고 개인 액세스 토큰을 포함하도록 GitHub 설정을 구성합니다.
1. GitHub 리포지토리에 다음 [암호화된 비밀](https://docs.github.com/en/actions/reference/encrypted-secrets)을 만듭니다.
   * **AEM_SITE** - AEM 사이트의 루트(예:  `wknd`)
   * **AEM_URL**  - AEM 작성자 환경의 URL
   * **GIT_TOKEN**  - 2단계의 개인 액세스 토큰입니다.
1. GitHub 동작 실행:**Github 객체**&#x200B;를 빌드하고 배포합니다. 작업을 실행하면 다운스트림 효과가 발생합니다.**아티팩트 id**&#x200B;로 AEM의 테마 구성을 업데이트합니다. 이 구성 요소는 `AEM_URL` 및 `AEM_SITE`에서 지정한 대로 AEM 환경에 테마 소스를 배포합니다.

### 재게시 예

참조로 사용할 수 있는 GitHub 재게시 예는 다음과 같습니다.

* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e) - &quot;real-world&quot; 프로젝트의 예로 사용됨
* [https://github.com/godanny86/wknd-theme](https://github.com/godanny86/wknd-theme)  - 자습서 다음의 예에 사용됩니다.

## 축하합니다!{#congratulations}

축하합니다. 방금 업데이트한 테마를 AEM에 배포했습니다!

### 다음 단계 {#next-steps}

[AEM Project Tranype](../project-archetype/overview.md)을(를) 사용하여 사이트를 제작하여 AEM 개발에 더욱 깊이 관여하고 기본적인 기술에 대한 자세한 내용을 살펴볼 수 있습니다.
