---
title: 사이트 만들기 | AEM 빠른 사이트 만들기
description: 사이트 만들기 마법사를 사용하여 새 웹 사이트를 생성하는 방법을 알아봅니다. Adobe이 제공하는 표준 사이트 템플릿은 새 사이트의 시작점입니다.
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
kt: 7496
thumbnail: KT-7496.jpg
exl-id: 6d0fdc4d-d85f-4966-8f7d-d53506a7dd08
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '912'
ht-degree: 1%

---

# 사이트 만들기 {#create-site}

빠른 사이트 만들기의 일부로, AEM의 Adobe Experience Manager에서 사이트 만들기 마법사를 사용하여 새 웹 사이트를 생성합니다. Adobe이 제공하는 표준 사이트 템플릿은 새 사이트의 시작점으로 사용됩니다.

## 사전 요구 사항 {#prerequisites}

이 장의 단계는 Adobe Experience Manager as a Cloud Service 환경에서 수행됩니다. AEM 환경에 대한 관리자 액세스 권한이 있는지 확인합니다. 을 사용하는 것이 좋습니다 [샌드박스 프로그램](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html) 및 [개발 환경](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html) 이 자습서를 완료할 때.

를 검토합니다. [온보딩 설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html) 자세한 내용

## 목표 {#objective}

1. 사이트 만들기 마법사를 사용하여 새 사이트를 생성하는 방법을 알아봅니다.
1. 사이트 템플릿의 역할을 이해합니다.
1. 생성된 AEM 사이트를 탐색합니다.

## Adobe Experience Manager 작성자에게 로그인 {#author}

첫 번째 단계로 AEM as a Cloud Service 환경에 로그인합니다. AEM 환경은 **작성자 서비스** 그리고 **게시 서비스**.

* **작성자 서비스** - 사이트 컨텐츠를 작성, 관리 및 업데이트하는 위치. 일반적으로 내부 사용자만 **작성자 서비스** 로그인 화면 뒤에 있습니다.
* **게시 서비스** - 라이브 웹 사이트를 호스팅합니다. 최종 사용자가 볼 수 있는 서비스이며 일반적으로 공개적으로 사용할 수 있습니다.

대부분의 자습서는 **작성자 서비스**.

1. Adobe Experience Cloud으로 이동합니다 [https://experience.adobe.com/](https://experience.adobe.com/). 개인 계정 또는 회사/학교 계정을 사용하여 로그인합니다.
1. 메뉴에서 올바른 조직을 선택했는지 확인하고 **Experience Manager**.

   ![Experience Cloud 홈](assets/create-site/experience-cloud-home-screen.png)

1. 아래 **Cloud Manager** click **Launch**.
1. 사용할 프로그램을 마우스로 가리킨 다음 **Cloud Manager 프로그램** 아이콘.

   ![Cloud Manager 프로그램 아이콘](assets/create-site/cloud-manager-program-icon.png)

1. 상단 메뉴에서 **환경** 제공된 환경을 확인합니다.

1. 사용할 환경을 찾아 **작성자 URL**.

   ![개발 작성자 액세스](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >을 사용하는 것이 좋습니다 **개발** 환경 을 참조하십시오.

1. AEM에 새 탭이 실행됩니다 **작성자 서비스**. 클릭 **Adobe으로 로그인** 또한 동일한 Experience Cloud 자격 증명으로 자동으로 로그인해야 합니다.

1. 이제 리디렉션되고 인증되면 AEM 시작 화면이 표시됩니다.

   ![AEM 시작 화면](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> Experience Manager 액세스에 문제가 있습니까? 를 검토합니다. [온보딩 설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)

## 기본 사이트 템플릿 다운로드

사이트 템플릿은 새 사이트의 시작점을 제공합니다. 사이트 템플릿에는 몇 가지 기본 테마, 페이지 템플릿, 구성 및 샘플 컨텐츠가 포함되어 있습니다. 사이트 템플릿에 포함된 것은 개발자에게 달려 있습니다. Adobe은 다음을 제공합니다 **기본 사이트 템플릿** 를 사용하십시오.

1. 새 브라우저 탭을 열고 GitHub의 기본 사이트 템플릿 프로젝트로 이동합니다. [https://github.com/adobe/aem-site-template-standard](https://github.com/adobe/aem-site-template-standard). 이 프로젝트는 오픈 소싱되며 누구나 사용할 수 있는 라이센스가 부여됩니다.
1. 클릭 **릴리스** 로 이동하여 [최신 릴리스](https://github.com/adobe/aem-site-template-standard/releases/최신).
1. 를 확장합니다. **자산** 드롭다운 및 템플릿 zip 파일 다운로드:

   ![기본 사이트 템플릿 Zip](assets/create-site/template-basic-zip-file.png)

   이 zip 파일은 다음 연습에서 사용됩니다.

   >[!NOTE]
   >
   > 이 자습서는 버전을 사용하여 작성됩니다 **1.1.0** 기본 사이트 템플릿 프로덕션 사용을 위해 새 프로젝트를 시작할 때는 항상 최신 버전을 사용하는 것이 좋습니다.

## 새 사이트 만들기

그런 다음 이전 연습에서 사이트 템플릿 을 사용하여 새 사이트를 생성합니다.

1. AEM 환경으로 돌아갑니다. AEM 시작 화면에서 로 이동합니다. **Sites**.
1. 오른쪽 상단 모서리에서 을(를) 클릭합니다. **만들기** > **사이트(템플릿)**. 그러면 **사이트 만들기 마법사**.
1. 아래 **사이트 템플릿 선택** 를 클릭합니다. **가져오기** 버튼을 클릭합니다.

   업로드 **.zip** 이전 연습에서 다운로드한 템플릿 파일입니다.

1. 을(를) 선택합니다 **기본 AEM 사이트 템플릿** 을(를) 클릭합니다. **다음**.

   ![사이트 템플릿 선택](assets/create-site/select-site-template.png)

1. 아래 **사이트 세부 사항** > **사이트 제목** enter `WKND Site`.

   실제 구현에서 &quot;WKND 사이트&quot;는 회사 또는 조직의 브랜드 이름으로 대체됩니다. 이 자습서에서는 매우 변덕스러운 라이프스타일 브랜드 &quot;WKND&quot;를 위한 사이트 만들기를 시뮬레이션하고 있습니다.

1. 아래 **사이트 이름** enter `wknd`.

   ![사이트 템플릿 세부 사항](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > 공유 AEM 환경을 사용하는 경우 고유 식별자를 **사이트 이름**. 예 `wknd-site-johndoe`. 이렇게 하면 여러 사용자가 충돌 없이 동일한 자습서를 완료할 수 있습니다.

1. 클릭 **만들기** 를 눌러 사이트를 생성합니다. 클릭 **완료** 에서 **성공** AEM에서 웹 사이트 만들기를 완료하면 대화 상자가 표시됩니다.

## 새 사이트 탐색

1. 아직 없는 경우 AEM Sites 콘솔으로 이동합니다.
1. 새로운 **WKND 사이트** 이(가) 생성되었습니다. 여기서는 다중 언어 계층 구조를 사용하는 사이트 구조를 포함합니다.
1. 를 엽니다. **영어** > **홈** 페이지를 선택하고 **편집** 메뉴 모음의 단추:

   ![WKND 사이트 계층](assets/create-site/wknd-site-starter-hierarchy.png)

1. 스타터 컨텐츠가 이미 만들어졌고 페이지에 여러 구성 요소를 추가할 수 있습니다. 이러한 구성 요소를 실험하여 기능에 대한 아이디어를 얻습니다. 다음 장에서는 구성 요소의 기본 사항을 학습합니다.

   ![홈 페이지 시작](assets/create-site/start-home-page.png)

   *사이트 템플릿에서 제공한 샘플 컨텐츠*

## 축하합니다! {#congratulations}

축하합니다. 첫 번째 AEM 사이트를 만들었습니다!

### 다음 단계 {#next-steps}

AEM의 Adobe Experience Manager에서 페이지 편집기를 사용하여 [컨텐츠 작성 및 게시](author-content-publish.md) 제2장. 내용을 업데이트하도록 원자 구성 요소를 구성하는 방법을 알아봅니다. AEM 작성자 및 게시 환경의 차이점을 파악하고 라이브 사이트에 업데이트를 게시하는 방법을 알아봅니다.
