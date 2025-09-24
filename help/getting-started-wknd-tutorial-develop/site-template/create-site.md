---
title: 사이트 만들기 | AEM 빠른 사이트 생성
description: 사이트 생성 마법사를 사용하여 새 웹 사이트를 생성하는 방법을 알아봅니다. Adobe가 제공하는 표준 사이트 템플릿은 새로운 사이트를 만드는 시작점입니다.
version: Experience Manager as a Cloud Service
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
jira: KT-7496
thumbnail: KT-7496.jpg
doc-type: Tutorial
exl-id: 6d0fdc4d-d85f-4966-8f7d-d53506a7dd08
recommendations: noDisplay, noCatalog
duration: 198
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '959'
ht-degree: 100%

---

# 사이트 만들기 {#create-site}

빠른 사이트 생성의 일부로 Adobe Experience Manager(AEM)의 사이트 생성 마법사를 사용하여 새로운 웹 사이트를 만듭니다. Adobe가 제공하는 표준 사이트 템플릿은 새로운 사이트를 만드는 시작점입니다.

## 사전 요구 사항 {#prerequisites}

이 챕터에 소개된 단계는 Adobe Experience Manager as a Cloud Service 환경에서 진행됩니다. AEM 환경에 대한 관리 액세스 권한이 있는지 확인하십시오. 이 튜토리얼을 완료할 때 [샌드박스 프로그램](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html) 및 [개발 환경](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html)을 사용하는 것이 권장됩니다.

[프로덕션 프로그램](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html) 환경 또한 이 튜토리얼에 사용할 수 있습니다. 다만 이 튜토리얼은 콘텐츠와 코드를 대상 AEM 환경에 배포하기 때문에 튜토리얼의 활동이 타깃 환경에서 수행되는 작업에 영향을 미치지 않도록 주의해야 합니다.

[AEM SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html)는 튜토리얼 중 일부에 사용될 수 있습니다. [Cloud Manager의 프론트엔드 파이프라인을 통한 테마 배포](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html) 등 이 튜토리얼에서 클라우드 서비스를 사용하는 부분은 AEM SDK에서 실행되지 않습니다.

자세한 내용은 [온보딩 문서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)를 검토하시기 바랍니다.

## 목표 {#objective}

1. 사이트 생성 마법사를 사용하여 새 사이트를 생성하는 방법을 알아봅니다.
1. 사이트 템플릿의 역할을 이해합니다.
1. 생성된 AEM 사이트를 탐색합니다.

## Adobe Experience Manager 작성자에 로그인합니다. {#author}

첫 번째 단계로 AEM as a Cloud Service 환경에 로그인합니다. AEM 환경은 **작성 서비스** 및 **게시 서비스** 간에 분할됩니다.

* **작성 서비스** - 사이트 콘텐츠가 생성, 관리 및 업데이트됩니다. 일반적으로 내부 사용자만 **작성 서비스**&#x200B;에 액세스할 수 있으며 로그인 화면 뒤에 위치합니다.
* **게시 서비스** - 라이브 웹 사이트를 호스팅합니다. 최종 사용자가 보게 되는 서비스로, 보통 공개적으로 사용 가능합니다.

튜토리얼의 대부분은 **작성 서비스**&#x200B;를 사용하여 진행됩니다.

1. Adobe Experience Cloud [https://experience.adobe.com/](https://experience.adobe.com/)로 이동합니다. 개인 계정이나 회사/학교 계정을 사용하여 로그인합니다.
1. 메뉴에서 올바른 조직을 선택하고 **Experience Manager**&#x200B;를 클릭합니다.

   ![Experience Cloud 홈](assets/create-site/experience-cloud-home-screen.png)

1. **Cloud Manager**&#x200B;에서 **실행**&#x200B;을 클릭합니다.
1. 사용하려는 프로그램 위에 마우스를 올려놓고 **Cloud Manager 프로그램** 아이콘을 클릭합니다.

   ![Cloud Manager 프로그램 아이콘](assets/create-site/cloud-manager-program-icon.png)

1. 상단 메뉴에서 **환경**&#x200B;을 클릭하여 프로비저닝된 환경을 확인합니다.

1. 사용하고 싶은 환경을 찾아 **작성자 URL**&#x200B;을 클릭합니다.

   ![개발 작성자 액세스](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >이 튜토리얼에서는 **개발** 환경을 사용하는 것이 권장됩니다.

1. AEM **저자 서비스**&#x200B;에 새로운 탭이 실행됩니다. **Adobe로 로그인**&#x200B;을 클릭하면 동일한 Experience Cloud 자격 증명을 사용하여 자동으로 로그인됩니다.

1. 리디렉션 및 인증을 거치고 나면 AEM 시작 화면이 표시됩니다.

   ![AEM 시작 화면](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> Experience Manager에 액세스하는 데 문제가 있습니까? [온보딩 문서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html) 검토

## 기본 사이트 템플릿 다운로드

사이트 템플릿은 새로운 사이트를 만드는 데 있어 시작점을 제공합니다. 사이트 템플릿에는 몇 가지 기본 테마, 페이지 템플릿, 구성 및 샘플 콘텐츠가 포함되어 있습니다. 사이트 템플릿에 정확히 무엇이 포함되는지는 개발자가 결정합니다. Adobe는 새로운 구현을 가속화할 수 있는 **기본 사이트 템플릿**&#x200B;을 제공합니다.

1. 새 브라우저 탭을 열고 GitHub의 기본 사이트 템플릿 프로젝트 [https://github.com/adobe/aem-site-template-standard](https://github.com/adobe/aem-site-template-standard)로 이동합니다. 이 프로젝트는 오픈 소스이며 누구에게나 사용이 허가됩니다.
1. **릴리스**&#x200B;를 클릭하고 [최신 릴리스](https://github.com/adobe/aem-site-template-standard/releases/latest)로 이동합니다.
1. **자산** 드롭다운을 확장하고 템플릿 zip 파일을 다운로드합니다.

   ![기본 사이트 템플릿 Zip](assets/create-site/template-basic-zip-file.png)

   이 zip 파일은 다음 연습에서 사용됩니다.

   >[!NOTE]
   >
   > 이 튜토리얼은 기본 사이트 템플릿의 **1.1.0** 버전을 사용하여 작성되었습니다. 프로덕션 용도로 새 프로젝트를 시작할 때는 항상 최신 버전을 사용하는 것이 좋습니다.

## 새 사이트 만들기

다음으로 이전 연습에서 사용한 사이트 템플릿을 사용하여 새 사이트를 생성합니다.

1. AEM 환경으로 돌아갑니다. AEM 시작 화면에서 **사이트**&#x200B;로 이동합니다.
1. 오른쪽 상단 모서리에서 **생성** > **사이트(템플릿)**&#x200B;를 클릭합니다. 그러면 **사이트 생성 마법사**&#x200B;가 나타납니다.
1. **사이트 템플릿 선택**&#x200B;에서 **가져오기** 버튼을 클릭합니다.

   이전 연습에서 다운로드한 **.zip** 템플릿 파일을 업로드합니다.

1. **기본 AEM 사이트 템플릿**&#x200B;을 선택하고 **다음**&#x200B;을 클릭합니다.

   ![사이트 템플릿 선택](assets/create-site/select-site-template.png)

1. **사이트 세부 정보** > **사이트 제목** 아래에 `WKND Site`를 입력합니다.

   실제 구현에서 “WKND 사이트”는 회사 또는 조직의 브랜드 이름으로 대체됩니다. 이 튜토리얼에서는 가상의 라이프스타일 브랜드 &#39;WKND&#39;의 사이트를 만드는 과정을 시뮬레이션해 보겠습니다.

1. **사이트 이름** 아래에 `wknd`를 입력합니다.

   ![사이트 템플릿 세부 정보](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > 공유 AEM 환경을 사용하는 경우, **사이트 이름**&#x200B;에 고유 식별자를 추가합니다. 예, `wknd-site-johndoe`. 이렇게 하면 여러 사용자가 충돌 없이 동일한 튜토리얼을 완료할 수 있습니다.

1. 사이트를 생성하려면 **생성**&#x200B;을 클릭합니다. AEM에서 웹 사이트 생성을 완료하면 **성공** 대화 상자에서 **완료**&#x200B;를 클릭합니다.

## 새로운 사이트 탐색

1. 아직 AEM Sites 콘솔에 없다면 이 콘솔로 이동합니다.
1. 새로운 **WKND 사이트**&#x200B;가 생성되었습니다. 여기에는 다국어 계층을 갖춘 사이트 구조가 포함됩니다.
1. 메뉴 바에서 **편집** 버튼을 선택하여 **영어** > > **홈** 페이지를 엽니다.

   ![WKND 사이트 계층](assets/create-site/wknd-site-starter-hierarchy.png)

1. 스타터 콘텐츠는 이미 생성되었으며, 페이지에 추가할 수 있는 여러 구성 요소가 있습니다. 이러한 구성 요소를 실험해 보면 기능을 파악할 수 있습니다. 다음 챕터에서는 구성 요소의 기본 사항을 알아봅니다.

   ![홈 페이지 시작](assets/create-site/start-home-page.png)

   *사이트 템플릿에서 제공하는 샘플 콘텐츠*

## 축하합니다! {#congratulations}

축하합니다! 첫 번째 AEM 사이트를 만들었습니다.

### 다음 단계 {#next-steps}

Adobe Experience Manager(AEM)의 페이지 편집기를 사용하여 [작성자 콘텐츠 및 게시](author-content-publish.md) 챕터의 사이트 콘텐츠를 업데이트합니다. 작은 단위의 구성 요소를 구성하여 콘텐츠를 업데이트하는 방법을 알아봅니다. AEM 작성자 및 게시 환경의 차이점을 이해하고 라이브 사이트에 업데이트를 게시하는 방법을 알아봅니다.
