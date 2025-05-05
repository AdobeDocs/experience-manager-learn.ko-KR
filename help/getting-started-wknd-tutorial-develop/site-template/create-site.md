---
title: 사이트 만들기 | AEM 빠른 사이트 생성
description: 사이트 생성 마법사를 사용하여 새 웹 사이트를 생성하는 방법에 대해 알아봅니다. Adobe에서 제공하는 표준 사이트 템플릿은 새 사이트의 시작점입니다.
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
workflow-type: tm+mt
source-wordcount: '959'
ht-degree: 0%

---

# 사이트 만들기 {#create-site}

빠른 사이트 생성의 일부로 AEM Adobe Experience Manager의 사이트 생성 마법사를 사용하여 새 웹 사이트를 생성합니다. Adobe에서 제공하는 표준 사이트 템플릿은 새 사이트의 시작점으로 사용됩니다.

## 사전 요구 사항 {#prerequisites}

이 장의 단계는 Adobe Experience Manager as a Cloud Service 환경에서 수행됩니다. AEM 환경에 대한 관리 액세스 권한이 있는지 확인합니다. 이 자습서를 완료할 때는 [샌드박스 프로그램](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html?lang=ko) 및 [개발 환경](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html?lang=ko)을 사용하는 것이 좋습니다.

[프로덕션 프로그램](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html?lang=ko) 환경도 이 자습서에 사용할 수 있습니다. 그러나 이 자습서에서는 콘텐츠와 코드를 대상 AEM 환경에 배포하므로 이 자습서의 활동이 대상 환경에서 수행되는 작업에 영향을 주지 않는지 확인하십시오.

이 자습서의 일부에는 [AEM SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=ko)을(를) 사용할 수 있습니다. [Cloud Manager의 프론트엔드 파이프라인으로 테마 배포](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html?lang=ko)와 같이 클라우드 서비스를 사용하는 이 자습서의 특성은 AEM SDK에서 수행할 수 없습니다.

자세한 내용은 [온보딩 설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html?lang=ko)를 검토하십시오.

## 목표 {#objective}

1. 사이트 생성 마법사를 사용하여 새 사이트를 생성하는 방법을 알아봅니다.
1. 사이트 템플릿의 역할을 이해합니다.
1. 생성된 AEM 사이트를 탐색합니다.

## Adobe Experience Manager 작성자에 로그인 {#author}

첫 번째 단계로 AEM as a Cloud Service 환경에 로그인합니다. AEM 환경은 **작성자 서비스**&#x200B;와 **게시 서비스** 간에 분할됩니다.

* **작성자 서비스** - 사이트 콘텐츠를 만들고 관리하고 업데이트하는 곳입니다. 일반적으로 내부 사용자만 **작성자 서비스**&#x200B;에 액세스할 수 있으며 로그인 화면 뒤에 있습니다.
* **Publish 서비스** - 실시간 웹 사이트를 호스팅합니다. 최종 사용자가 볼 수 있는 서비스이며 일반적으로 공개적으로 사용할 수 있습니다.

대부분의 자습서는 **작성자 서비스**&#x200B;를 사용하여 수행됩니다.

1. Adobe Experience Cloud [https://experience.adobe.com/](https://experience.adobe.com/)&#x200B;(으)로 이동합니다. 개인 계정 또는 회사/학교 계정을 사용하여 로그인합니다.
1. 메뉴에서 올바른 조직이 선택되어 있는지 확인하고 **Experience Manager**&#x200B;을(를) 클릭합니다.

   ![Experience Cloud 홈](assets/create-site/experience-cloud-home-screen.png)

1. **Cloud Manager**&#x200B;에서 **시작**&#x200B;을 클릭합니다.
1. 사용할 프로그램을 마우스로 가리킨 다음 **Cloud Manager 프로그램** 아이콘을 클릭합니다.

   ![Cloud Manager 프로그램 아이콘](assets/create-site/cloud-manager-program-icon.png)

1. 위쪽 메뉴에서 **환경**&#x200B;을 클릭하여 제공된 환경을 봅니다.

1. 사용할 환경을 찾은 다음 **작성자 URL**&#x200B;을(를) 클릭합니다.

   ![개발 작성자 액세스](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >이 자습서에서는 **개발** 환경을 사용하는 것이 좋습니다.

1. AEM **작성자 서비스**&#x200B;에 새 탭이 시작됩니다. **Adobe으로 로그인**&#x200B;을 클릭하면 동일한 Experience Cloud 자격 증명으로 자동으로 로그인됩니다.

1. 리디렉션되고 인증되면 이제 AEM 시작 화면이 표시됩니다.

   ![AEM 시작 화면](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> Experience Manager에 액세스하는 데 문제가 있습니까? [온보딩 설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html?lang=ko) 검토

## 기본 사이트 템플릿 다운로드

사이트 템플릿은 새 사이트의 시작점을 제공합니다. 사이트 템플릿에는 몇 가지 기본 테마, 페이지 템플릿, 구성 및 샘플 콘텐츠가 포함됩니다. 사이트 템플릿에 포함된 내용은 정확히 개발자의 책임입니다. Adobe은 새로운 구현을 가속화하기 위해 **기본 사이트 템플릿**&#x200B;을 제공합니다.

1. 새 브라우저 탭을 열고 GitHub의 기본 사이트 템플릿 프로젝트로 이동합니다. [https://github.com/adobe/aem-site-template-standard](https://github.com/adobe/aem-site-template-standard). 이 프로젝트는 오픈 소스로 제공되며 누구나 사용할 수 있도록 라이센스가 부여됩니다.
1. **릴리스**&#x200B;를 클릭하고 [최신 릴리스](https://github.com/adobe/aem-site-template-standard/releases/latest)&#x200B;(으)로 이동합니다.
1. **Assets** 드롭다운을 확장하고 템플릿 zip 파일을 다운로드합니다.

   ![기본 사이트 템플릿 Zip](assets/create-site/template-basic-zip-file.png)

   이 zip 파일은 다음 연습에서 사용됩니다.

   >[!NOTE]
   >
   > 이 자습서는 기본 사이트 템플릿의 버전 **1.1.0**&#x200B;을(를) 사용하여 작성되었습니다. 프로덕션 사용을 위해 새 프로젝트를 시작할 때 항상 최신 버전을 사용하는 것이 좋습니다.

## 새 사이트 만들기

그런 다음 이전 연습의 사이트 템플릿을 사용하여 새 사이트를 생성합니다.

1. AEM 환경으로 돌아갑니다. AEM 시작 화면에서 **사이트**(으)로 이동합니다.
1. 오른쪽 상단 모서리에서 **만들기** > **사이트(템플릿)**&#x200B;를 클릭합니다. **사이트 만들기 마법사**&#x200B;가 실행됩니다.
1. **사이트 템플릿 선택**&#x200B;에서 **가져오기** 단추를 클릭합니다.

   이전 연습에서 다운로드한 **.zip** 템플릿 파일을 업로드합니다.

1. **기본 AEM 사이트 템플릿**&#x200B;을 선택하고 **다음**&#x200B;을 클릭합니다.

   ![사이트 템플릿 선택](assets/create-site/select-site-template.png)

1. **사이트 세부 정보** > **사이트 제목**&#x200B;에서 `WKND Site`을(를) 입력하십시오.

   실제 구현에서 &quot;WKND 사이트&quot;는 회사 또는 조직의 브랜드 이름으로 대체됩니다. 이 자습서에서는 가상 라이프스타일 브랜드 &quot;WKND&quot;에 대한 사이트 생성을 시뮬레이션합니다.

1. **사이트 이름**&#x200B;에서 `wknd`을(를) 입력하십시오.

   ![사이트 템플릿 세부 정보](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > 공유 AEM 환경을 사용하는 경우 **사이트 이름**&#x200B;에 고유 식별자를 추가하십시오. 예, `wknd-site-johndoe`. 이렇게 하면 여러 사용자가 충돌 없이 동일한 자습서를 완료할 수 있습니다.

1. 사이트를 생성하려면 **만들기**&#x200B;를 클릭하십시오. AEM에서 웹 사이트 만들기를 완료하면 **성공** 대화 상자에서 **완료**&#x200B;를 클릭합니다.

## 새 사이트 탐색

1. 아직 없는 경우 AEM Sites 콘솔로 이동합니다.
1. 새 **WKND 사이트**&#x200B;가 생성되었습니다. 여기에는 다국어 계층 구조를 가진 사이트 구조가 포함됩니다.
1. 페이지를 선택하고 메뉴 모음에서 **편집** 단추를 클릭하여 **영어** > **홈** 페이지를 엽니다.

   ![WKND 사이트 계층 구조](assets/create-site/wknd-site-starter-hierarchy.png)

1. 스타터 콘텐츠는 이미 만들어졌으며 여러 구성 요소를 페이지에 추가할 수 있습니다. 이러한 구성 요소를 실험하여 기능을 파악하십시오. 다음 장에서 구성 요소의 기본 사항에 대해 알아봅니다.

   ![홈 페이지 시작](assets/create-site/start-home-page.png)

   *사이트 템플릿에서 제공한 샘플 콘텐츠*

## 축하합니다! {#congratulations}

축하합니다. 첫 번째 AEM 사이트를 만들었습니다!

### 다음 단계 {#next-steps}

AEM Adobe Experience Manager의 페이지 편집기를 사용하여 [콘텐츠 작성 및 게시](author-content-publish.md) 장에서 사이트의 콘텐츠를 업데이트합니다. 원자 구성 요소를 구성하여 콘텐츠를 업데이트하는 방법에 대해 알아봅니다. AEM 작성자와 게시 환경의 차이점을 이해하고 라이브 사이트에 업데이트를 게시하는 방법을 알아봅니다.
