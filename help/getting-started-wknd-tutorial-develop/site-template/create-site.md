---
title: 사이트 만들기 | AEM 빠른 사이트 생성
description: 사이트 생성 마법사를 사용하여 새 웹 사이트를 생성하는 방법에 대해 알아봅니다. Adobe에서 제공하는 표준 사이트 템플릿은 새 사이트의 시작점입니다.
version: Cloud Service
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
jira: KT-7496
thumbnail: KT-7496.jpg
doc-type: Tutorial
exl-id: 6d0fdc4d-d85f-4966-8f7d-d53506a7dd08
recommendations: noDisplay, noCatalog
duration: 265
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '959'
ht-degree: 0%

---

# 사이트 만들기 {#create-site}

빠른 사이트 생성 의 일부로, AEM Adobe Experience Manager의 사이트 생성 마법사를 사용하여 새 웹 사이트를 생성합니다. Adobe에서 제공하는 표준 사이트 템플릿은 새 사이트의 시작점으로 사용됩니다.

## 사전 요구 사항 {#prerequisites}

이 장의 단계는 Adobe Experience Manager as a Cloud Service 환경에서 수행됩니다. AEM 환경에 대한 관리 액세스 권한이 있는지 확인합니다. 를 사용하는 것이 좋습니다. [샌드박스 프로그램](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html) 및 [개발 환경](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html) 이 자습서를 완료할 때입니다.

[프로덕션 프로그램](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html) 이 자습서에서는 환경을 사용할 수도 있습니다. 그러나 이 자습서에서는 콘텐츠와 코드를 대상 AEM 환경에 배포하므로 이 자습서의 활동이 대상 환경에서 수행되는 작업에 영향을 주지 않는지 확인하십시오.

다음 [AEM SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html) 은 이 자습서의 일부에 사용할 수 있습니다. 다음과 같이 클라우드 서비스에 의존하는 이 튜토리얼의 측면 [cloud Manager의 프론트엔드 파이프라인으로 테마 배포](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html)AEM SDK에서는 을 수행할 수 없습니다.

리뷰 [온보딩 설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html) 을 참조하십시오.

## 목표 {#objective}

1. 사이트 생성 마법사를 사용하여 새 사이트를 생성하는 방법을 알아봅니다.
1. 사이트 템플릿의 역할을 이해합니다.
1. 생성된 AEM 사이트를 탐색합니다.

## Adobe Experience Manager 작성자에 로그인 {#author}

AEM 첫 번째 단계로 as a Cloud Service 환경에 로그인합니다. AEM 환경은 **Author 서비스** 및 a **서비스 게시**.

* **Author 서비스** - 사이트 콘텐츠의 생성, 관리 및 업데이트 위치 일반적으로 내부 사용자만 **Author 서비스** 로그인 화면 뒤에 있습니다.
* **서비스 게시** - 라이브 웹 사이트를 호스팅합니다. 최종 사용자가 볼 수 있는 서비스이며 일반적으로 공개적으로 사용할 수 있습니다.

대부분의 튜토리얼은 **Author 서비스**.

1. Adobe Experience Cloud으로 이동 [https://experience.adobe.com/](https://experience.adobe.com/). 개인 계정 또는 회사/학교 계정을 사용하여 로그인합니다.
1. 메뉴에서 올바른 조직이 선택되었는지 확인하고 **Experience Manager**.

   ![Experience Cloud 홈](assets/create-site/experience-cloud-home-screen.png)

1. 아래 **Cloud Manager** 클릭 **시작**.
1. 사용할 프로그램 위에 마우스를 놓고 **Cloud Manager 프로그램** 아이콘.

   ![Cloud Manager 프로그램 아이콘](assets/create-site/cloud-manager-program-icon.png)

1. 상단 메뉴에서 **환경** 프로비저닝된 환경을 봅니다.

1. 사용할 환경을 찾은 다음 **작성자 URL**.

   ![개발 작성자 액세스](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >를 사용하는 것이 좋습니다. **개발** 이 자습서에 대한 환경입니다.

1. AEM에 새 탭이 실행됩니다. **Author 서비스**. 클릭 **Adobe으로 로그인** 그리고 동일한 Experience Cloud 자격 증명으로 자동으로 로그인해야 합니다.

1. 리디렉션되고 인증되면 이제 AEM 시작 화면이 표시됩니다.

   ![AEM 시작 화면](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> Experience Manager 액세스에 문제가 있습니까? 리뷰 [온보딩 설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)

## 기본 사이트 템플릿 다운로드

사이트 템플릿은 새 사이트의 시작점을 제공합니다. 사이트 템플릿에는 몇 가지 기본 테마, 페이지 템플릿, 구성 및 샘플 콘텐츠가 포함됩니다. 사이트 템플릿에 포함된 내용은 정확히 개발자의 책임입니다. Adobe에서 다음을 제공합니다. **기본 사이트 템플릿** 새로운 구현을 가속화하기 위해

1. 새 브라우저 탭을 열고 GitHub의 기본 사이트 템플릿 프로젝트로 이동합니다. [https://github.com/adobe/aem-site-template-standard](https://github.com/adobe/aem-site-template-standard). 이 프로젝트는 오픈 소스로 제공되며 누구나 사용할 수 있도록 라이센스가 부여됩니다.
1. 클릭 **릴리스** 다음 위치로 이동 [최신 릴리스](https://github.com/adobe/aem-site-template-standard/releases/latest).
1. 확장 **에셋** 템플릿 zip 파일을 드롭다운하고 다운로드합니다.

   ![기본 사이트 템플릿 Zip](assets/create-site/template-basic-zip-file.png)

   이 zip 파일은 다음 연습에서 사용됩니다.

   >[!NOTE]
   >
   > 이 자습서는 버전을 사용하여 작성되었습니다. **1.1.0** 기본 사이트 템플릿 프로덕션 사용을 위해 새 프로젝트를 시작할 때 항상 최신 버전을 사용하는 것이 좋습니다.

## 새 사이트 만들기

그런 다음 이전 연습의 사이트 템플릿을 사용하여 새 사이트를 생성합니다.

1. AEM 환경으로 돌아갑니다. AEM 시작 화면에서 다음으로 이동합니다. **사이트**.
1. 오른쪽 상단에서 를 클릭합니다. **만들기** > **사이트(템플릿)**. 이렇게 하면 다음 항목이 표시됩니다. **사이트 생성 마법사**.
1. 아래 **사이트 템플릿 선택** 클릭: **가져오기** 단추를 클릭합니다.

   업로드 **.zip** 이전 연습에서 다운로드한 템플릿 파일입니다.

1. 다음 항목 선택 **기본 AEM 사이트 템플릿** 및 클릭 **다음**.

   ![사이트 템플릿 선택](assets/create-site/select-site-template.png)

1. 아래 **사이트 세부 정보** > **사이트 제목** 입력 `WKND Site`.

   실제 구현에서 &quot;WKND 사이트&quot;는 회사 또는 조직의 브랜드 이름으로 대체됩니다. 이 자습서에서는 가상 라이프스타일 브랜드 &quot;WKND&quot;에 대한 사이트 생성을 시뮬레이션합니다.

1. 아래 **사이트 이름** 입력 `wknd`.

   ![사이트 템플릿 세부 정보](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > 공유 AEM 환경을 사용하는 경우 고유 식별자를 **사이트 이름**. 예 `wknd-site-johndoe`. 이렇게 하면 여러 사용자가 충돌 없이 동일한 자습서를 완료할 수 있습니다.

1. 클릭 **만들기** 사이트를 생성합니다. 클릭 **완료** 다음에서 **성공** AEM이 웹 사이트 생성을 완료하면 대화 상자가 표시됩니다.

## 새 사이트 탐색

1. 아직 없는 경우 AEM Sites 콘솔로 이동합니다.
1. 새 항목 **WKND 사이트** 생성되었습니다. 여기에는 다국어 계층 구조를 가진 사이트 구조가 포함됩니다.
1. 를 엽니다. **영어** > **홈** 페이지를 선택하고 **편집** 메뉴 모음의 단추:

   ![WKND 사이트 계층](assets/create-site/wknd-site-starter-hierarchy.png)

1. 스타터 콘텐츠는 이미 만들어졌으며 여러 구성 요소를 페이지에 추가할 수 있습니다. 이러한 구성 요소를 실험하여 기능을 파악하십시오. 다음 장에서 구성 요소의 기본 사항에 대해 알아봅니다.

   ![홈 페이지 시작](assets/create-site/start-home-page.png)

   *사이트 템플릿에서 제공하는 샘플 콘텐츠*

## 축하합니다! {#congratulations}

축하합니다. 첫 번째 AEM 사이트를 방금 만들었습니다!

### 다음 단계 {#next-steps}

Adobe Experience Manager, AEM에서 페이지 편집기를 사용하여 [콘텐츠 작성 및 게시](author-content-publish.md) 챕터. 원자 구성 요소를 구성하여 콘텐츠를 업데이트하는 방법에 대해 알아봅니다. AEM 작성자와 게시 환경의 차이점을 이해하고 라이브 사이트에 업데이트를 게시하는 방법을 알아봅니다.
