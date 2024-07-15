---
title: AEM Sites 시작하기 - 페이지 및 템플릿
description: 기본 페이지 구성 요소와 편집 가능한 템플릿 간의 관계에 대해 알아봅니다. 핵심 구성 요소가 프로젝트로 프록시되는 방식을 이해합니다. 편집 가능한 템플릿의 고급 정책 구성에 대해 알아보고 Adobe XD의 목차를 기반으로 잘 구조화된 문서 페이지 템플릿을 구축할 수 있습니다.
feature: Core Components, Editable Templates, Page Editor
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
jira: KT-4082
thumbnail: 30214.jpg
doc-type: Tutorial
exl-id: e9d06dc2-ac3b-48c5-ae00-fdaf5bb45b54
recommendations: noDisplay, noCatalog
duration: 2049
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '2855'
ht-degree: 0%

---

# 페이지 및 템플릿 {#pages-and-template}

{{edge-delivery-services-and-page-editor}}

이 장에서는 기본 페이지 구성 요소와 편집 가능한 템플릿 간의 관계를 살펴보겠습니다. [Adobe XD](https://helpx.adobe.com/support/xd.html)의 일부 모형을 기반으로 스타일이 지정되지 않은 문서 템플릿을 만드는 방법을 알아봅니다. 템플릿을 작성하는 과정에서 편집 가능한 템플릿의 핵심 구성 요소 및 고급 정책 구성에 대해 다룹니다.

## 사전 요구 사항 {#prerequisites}

[로컬 개발 환경](overview.md#local-dev-environment)을 설정하는 데 필요한 도구 및 지침을 검토하십시오.

### 스타터 프로젝트

>[!NOTE]
>
> 이전 장을 성공적으로 완료한 경우 프로젝트를 재사용하고 스타터 프로젝트 체크 아웃 단계를 건너뛸 수 있습니다.

자습서가 빌드하는 기본 코드 체크 아웃:

1. [GitHub](https://github.com/adobe/aem-guides-wknd)에서 `tutorial/pages-templates-start` 분기 확인

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ git checkout tutorial/pages-templates-start
   ```

1. Maven 기술을 사용하여 로컬 AEM 인스턴스에 코드 베이스를 배포합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > AEM 6.5 또는 6.4를 사용하는 경우 `classic` 프로필을 Maven 명령에 추가합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

[GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/pages-templates-solution)에서 완성된 코드를 항상 보거나 `tutorial/pages-templates-solution` 분기로 전환하여 코드를 로컬로 확인할 수 있습니다.

## 목표

1. Inspect Adobe XD에서 만든 페이지 디자인을 매핑하고 핵심 구성 요소에 매핑합니다.
1. 편집 가능한 템플릿의 세부 정보와 정책을 사용하여 페이지 콘텐츠를 세부적으로 제어하는 방법에 대해 알아봅니다.
1. 템플릿 및 페이지가 연결되는 방법 알아보기

## 빌드할 항목 {#what-build}

이 자습서 부분에서는 문서 페이지를 만들고 공통 구조에 맞게 조정하는 데 사용할 수 있는 새 문서 페이지 템플릿을 작성합니다. 문서 페이지 템플릿은 Adobe XD에서 생성된 디자인과 UI 키트를 기반으로 합니다. 이 장은 템플릿의 구조 또는 뼈대를 작성하는 데에만 중점을 둡니다. 스타일은 구현되지 않지만 템플릿과 페이지는 작동합니다.

![문서 페이지 디자인 및 스타일이 지정되지 않은 버전](assets/pages-templates/what-you-will-build.png)

## Adobe XD을 통한 UI 계획 {#adobexd}

일반적으로 새 웹 사이트에 대한 계획은 모형과 정적 디자인에서 시작됩니다. [Adobe XD](https://helpx.adobe.com/support/xd.html)은(는) 사용자 환경을 구축하는 디자인 도구입니다. 다음으로 문서 페이지 템플릿의 구조를 계획하는 데 도움이 되는 UI 키트 및 모형을 검사해 보겠습니다.

>[!VIDEO](https://video.tv.adobe.com/v/30214?quality=12&learn=on)

**[WKND 문서 디자인 파일을 다운로드합니다](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**.

>[!NOTE]
>
> 일반 [AEM 핵심 구성 요소 UI 키트 또한 사용자 지정 프로젝트의 시작점으로 사용할 수 있습니다](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd).

## 문서 페이지 템플릿 만들기

페이지를 만들 때 페이지를 만드는 기반으로 사용되는 템플릿을 선택해야 합니다. 템플릿은 결과 페이지, 초기 콘텐츠 및 허용된 구성 요소의 구조를 정의합니다.

[편집 가능한 템플릿](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html)에는 세 가지 기본 영역이 있습니다.

1. **구조** - 템플릿의 일부인 구성 요소를 정의합니다. 콘텐츠 작성자는 편집할 수 없습니다.
1. **초기 콘텐츠** - 템플릿이 시작되는 구성 요소를 정의하며 콘텐츠 작성자는 해당 구성 요소를 편집 및/또는 삭제할 수 있습니다.
1. **정책** - 구성 요소의 동작 방식과 작성자의 옵션에 대한 구성을 정의합니다.

그런 다음 AEM에서 모형의 구조와 일치하는 템플릿을 만듭니다. 이 문제는 AEM의 로컬 인스턴스에서 발생합니다. 아래 비디오에 나와 있는 단계를 따르십시오.

>[!VIDEO](https://video.tv.adobe.com/v/330991?quality=12&learn=on)

위의 비디오에 대한 높은 수준의 단계:

### 구조 구성

1. **페이지 템플릿 형식**(이름: **문서 페이지**)을(를) 사용하여 템플릿을 만듭니다.
1. **구조** 모드로 전환합니다.
1. 템플릿의 맨 위에서 **헤더** 역할을 할 **경험 조각** 구성 요소를 추가하십시오.
   * `/content/experience-fragments/wknd/us/en/site/header/master`을(를) 가리키도록 구성 요소를 구성합니다.
   * 정책을 **페이지 헤더**(으)로 설정하고 **기본 요소**&#x200B;이(가) `header`(으)로 설정되어 있는지 확인하십시오. `header`요소는 다음 장에서 CSS로 타깃팅됩니다.
1. 템플릿의 맨 아래에 **바닥글** 역할을 하는 **경험 조각** 구성 요소를 추가하십시오.
   * `/content/experience-fragments/wknd/us/en/site/footer/master`을(를) 가리키도록 구성 요소를 구성합니다.
   * 정책을 **페이지 바닥글**(으)로 설정하고 **기본 요소**&#x200B;가 `footer`(으)로 설정되어 있는지 확인하십시오. `footer` 요소는 다음 장에서 CSS로 타깃팅됩니다.
1. 템플릿을 처음 만들 때 포함된 **main** 컨테이너를 잠급니다.
   * 정책을 **Page Main**(으)로 설정하고 **기본 요소**&#x200B;이(가) `main`(으)로 설정되어 있는지 확인하십시오. `main` 요소는 다음 장에서 CSS로 타깃팅됩니다.
1. **Image** 구성 요소를 **main** 컨테이너에 추가합니다.
   * **이미지** 구성 요소의 잠금을 해제합니다.
1. 기본 컨테이너의 **이미지** 구성 요소 아래에 **이동 경로** 구성 요소를 추가합니다.
   * **문서 페이지 - 이동 경로**(이)라는 **이동 경로** 구성 요소에 대한 정책을 만듭니다. **탐색 시작 수준**&#x200B;을 **4**(으)로 설정합니다.
1. **이동 경로** 구성 요소 아래와 **main** 컨테이너 내에 **Container** 구성 요소를 추가하십시오. 이는 템플릿의 **콘텐츠 컨테이너** 역할을 합니다.
   * **Content** 컨테이너의 잠금을 해제합니다.
   * 정책을 **페이지 콘텐츠**(으)로 설정합니다.
1. **콘텐츠 컨테이너** 아래에 다른 **컨테이너** 구성 요소를 추가합니다. 이는 템플릿의 **측면 레일** 컨테이너로 작동합니다.
   * **측면 레일** 컨테이너의 잠금을 해제합니다.
   * 이름이 **문서 페이지 - 측면 레일**&#x200B;인 정책을 만듭니다.
   * **WKND Sites 프로젝트 - 콘텐츠**&#x200B;에서 **허용된 구성 요소**&#x200B;를 구성하여 다음을 포함합니다. **단추**, **다운로드**, **이미지**, **목록**, **구분 기호**, **소셜 미디어 공유**, **텍스트** 및 **제목**.
1. 페이지 루트 컨테이너의 정책을 업데이트합니다. 템플릿에서 가장 바깥쪽 컨테이너입니다. 정책을 **페이지 루트**(으)로 설정합니다.
   * **컨테이너 설정**&#x200B;에서 **레이아웃**&#x200B;을(를) **응답형 격자**(으)로 설정합니다.
1. **콘텐츠 컨테이너**&#x200B;에 대한 참여 레이아웃 모드입니다. 핸들을 오른쪽에서 왼쪽으로 드래그하고 컨테이너를 8열 너비로 축소합니다.
1. **측면 레일 컨테이너**&#x200B;에 대한 참여 레이아웃 모드입니다. 핸들을 오른쪽에서 왼쪽으로 드래그하고 컨테이너를 4열 너비로 축소합니다. 그런 다음 왼쪽 핸들을 왼쪽에서 오른쪽으로 한 열로 끌어 컨테이너를 3열 너비로 만들고 **콘텐츠 컨테이너** 사이에 1열 간격을 둡니다.
1. 모바일 에뮬레이터를 열고 모바일 중단점으로 전환합니다. 레이아웃 모드를 다시 시작하고 **콘텐츠 컨테이너** 및 **측면 레일 컨테이너**&#x200B;를 페이지의 전체 너비로 만드십시오. 이렇게 하면 모바일 중단점에서 컨테이너가 세로로 스택됩니다.
1. **콘텐츠 컨테이너**&#x200B;에서 **Text** 구성 요소의 정책을 업데이트합니다.
   * 정책을 **콘텐츠 텍스트**(으)로 설정합니다.
   * **플러그인** > **단락 스타일**&#x200B;에서 **단락 스타일 사용**&#x200B;을 선택하고 **견적 블록**&#x200B;이 활성화되어 있는지 확인하십시오.

### 초기 콘텐츠 구성

1. **초기 콘텐츠** 모드로 전환합니다.
1. **제목** 구성 요소를 **콘텐츠 컨테이너**&#x200B;에 추가합니다. 이 제목은 기사 제목으로 사용됩니다. 비워 두면 현재 페이지의 제목 이 자동으로 표시됩니다.
1. 첫 번째 제목 구성 요소 아래에 두 번째 **제목** 구성 요소를 추가합니다.
   * &quot;작성자별&quot;이라는 텍스트를 사용하여 구성 요소를 구성합니다. 텍스트 자리 표시자입니다.
   * 형식을 `H4`(으)로 설정하십시오.
1. **작성자별** 제목 구성 요소 아래에 **Text** 구성 요소를 추가합니다.
1. **제목** 구성 요소를 **측면 레일 컨테이너**&#x200B;에 추가합니다.
   * &quot;이 스토리 공유&quot;라는 텍스트를 사용하여 구성 요소를 구성합니다.
   * 형식을 `H5`(으)로 설정하십시오.
1. **이 스토리 공유** 제목 구성 요소 아래에 **소셜 미디어 공유** 구성 요소를 추가합니다.
1. **소셜 미디어 공유** 구성 요소 아래에 **구분 기호** 구성 요소를 추가합니다.
1. **구분 기호** 구성 요소 아래에 **다운로드** 구성 요소를 추가합니다.
1. **다운로드** 구성 요소 아래에 **목록** 구성 요소를 추가합니다.
1. 템플릿에 대한 **초기 페이지 속성**&#x200B;을(를) 업데이트합니다.
   * **소셜 미디어** > **소셜 미디어 공유**&#x200B;에서 **Facebook** 및 **Pinterest**&#x200B;을 확인하세요.

### 템플릿 활성화 및 썸네일 추가

1. [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)(으)로 이동하여 템플릿 콘솔에서 템플릿을 봅니다
1. 문서 페이지 템플릿을 **사용**&#x200B;합니다.
1. 문서 페이지 템플릿의 속성을 편집하고 다음 썸네일을 업로드하여 문서 페이지 템플릿을 사용하여 만든 페이지를 신속하게 식별할 수 있습니다.

   ![문서 페이지 템플릿 썸네일](assets/pages-templates/article-page-template-thumbnail.png)

## 경험 조각으로 머리글 및 바닥글 업데이트 {#experience-fragments}

머리글이나 바닥글과 같은 전역 콘텐츠를 만들 때 일반적으로 사용하는 방법은 [경험 조각](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html)입니다. 경험 조각 을 사용하면 여러 구성 요소를 결합하여 참조 가능한 단일 구성 요소를 만들 수 있습니다. 경험 조각은 다중 사이트 관리 및 [로컬라이제이션](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/experience-fragment.html?lang=en)을 지원할 수 있는 이점이 있습니다.

AEM Project Archetype에서 머리글과 바닥글을 생성했습니다. 그런 다음, mockup과 일치하도록 경험 조각 을 업데이트합니다. 아래 비디오에 나와 있는 단계를 따르십시오.

>[!VIDEO](https://video.tv.adobe.com/v/330992?quality=12&learn=on)

위의 비디오에 대한 높은 수준의 단계:

1. 샘플 콘텐츠 패키지 **[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets-1.1.zip)**&#x200B;을(를) 다운로드합니다.
1. [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)에서 패키지 관리자를 사용하여 콘텐츠 패키지를 업로드하고 설치합니다.
1. [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html)에서 경험 조각에 사용되는 템플릿인 웹 변형 템플릿을 업데이트합니다.
   * 템플릿의 **Container** 구성 요소에 대한 정책을 업데이트하십시오.
   * 정책을 **XF 루트**(으)로 설정합니다.
   * **허용된 구성 요소**&#x200B;에서 구성 요소 그룹 **WKND Sites 프로젝트 - 구조**&#x200B;를 선택하여 **언어 탐색**, **탐색** 및 **빠른 검색** 구성 요소를 포함합니다.

### 헤더 경험 조각 업데이트

1. [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html)에서 헤더를 렌더링하는 경험 조각을 엽니다.
1. 조각의 루트 **컨테이너**&#x200B;를 구성하십시오. 가장 바깥쪽 **컨테이너**&#x200B;입니다.
   * **레이아웃**&#x200B;을(를) **응답형 격자**(으)로 설정
1. **컨테이너**&#x200B;의 맨 위에 **WKND 어두운 로고**&#x200B;를 이미지로 추가합니다. 이전 단계에서 설치한 패키지에 로고가 포함되었습니다.
   * **WKND Dark Logo**&#x200B;의 레이아웃을 **두**&#x200B;열 너비로 수정하십시오. 오른쪽에서 왼쪽으로 핸들을 드래그합니다.
   * &quot;WKND 로고&quot;의 **대체 텍스트**&#x200B;로 로고를 구성합니다.
   * 로고를 홈 페이지의 **링크**&#x200B;에 구성`/content/wknd/us/en`합니다.
1. 페이지에 이미 배치된 **탐색** 구성 요소를 구성합니다.
   * **루트 수준 제외**&#x200B;를 **1**(으)로 설정합니다.
   * **탐색 구조 깊이**&#x200B;을(를) **1**(으)로 설정합니다.
   * **탐색** 구성 요소의 레이아웃을 **8**&#x200B;열 너비로 수정하십시오. 오른쪽에서 왼쪽으로 핸들을 드래그합니다.
1. **언어 탐색** 구성 요소를 제거합니다.
1. **검색** 구성 요소의 레이아웃을 **두**&#x200B;열 너비로 수정하십시오. 오른쪽에서 왼쪽으로 핸들을 드래그합니다. 이제 모든 구성 요소를 한 행에 수평으로 정렬해야 합니다.

### 바닥글 경험 조각 업데이트

1. [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)에서 바닥글을 렌더링하는 경험 조각을 엽니다.
1. 조각의 루트 **컨테이너**&#x200B;를 구성하십시오. 가장 바깥쪽 **컨테이너**&#x200B;입니다.
   * **레이아웃**&#x200B;을(를) **응답형 격자**(으)로 설정
1. **WKND Light 로고**&#x200B;를 **컨테이너**&#x200B;의 맨 위에 이미지로 추가합니다. 이전 단계에서 설치한 패키지에 로고가 포함되었습니다.
   * **WKND Light 로고**&#x200B;의 레이아웃을 **두**&#x200B;열 너비로 수정하십시오. 오른쪽에서 왼쪽으로 핸들을 드래그합니다.
   * &quot;WKND 로고 라이트&quot;의 **대체 텍스트**&#x200B;로 로고를 구성합니다.
   * 로고를 홈 페이지의 **링크**&#x200B;에 구성`/content/wknd/us/en`합니다.
1. 로고 아래에 **탐색** 구성 요소를 추가합니다. **탐색** 구성 요소 구성:
   * **루트 수준 제외**&#x200B;를 **1**(으)로 설정합니다.
   * **모든 하위 페이지 수집**&#x200B;을 선택 취소합니다.
   * **탐색 구조 깊이**&#x200B;을(를) **1**(으)로 설정합니다.
   * **탐색** 구성 요소의 레이아웃을 **8**&#x200B;열 너비로 수정하십시오. 오른쪽에서 왼쪽으로 핸들을 드래그합니다.

## 문서 페이지 만들기

그런 다음 문서 페이지 템플릿을 사용하여 페이지를 만듭니다. 사이트 모형과 일치하도록 페이지 콘텐츠를 작성합니다. 아래 비디오에 나와 있는 단계를 따르십시오.

>[!VIDEO](https://video.tv.adobe.com/v/330993?quality=12&learn=on)

위의 비디오에 대한 높은 수준의 단계:

1. [http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine)의 사이트 콘솔로 이동합니다.
1. **WKND** > **US** > **EN** > **매거진** 아래에 페이지를 만듭니다.
   * **문서 페이지** 템플릿을 선택하십시오.
   * **속성**&#x200B;에서 **제목**&#x200B;을 &quot;Ultimate Guide to LA Skateparks&quot;로 설정합니다.
   * **Name**&#x200B;을(를) &quot;가이드-라-스케이트보드장&quot;으로 설정합니다.
1. **작성자별** 제목을 &quot;Stacey Roswells별&quot;이라는 텍스트로 바꿉니다.
1. 문서를 채울 단락을 포함하도록 **Text** 구성 요소를 업데이트하십시오. 다음 텍스트 파일을 복사본으로 사용할 수 있습니다. [la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt).
1. 다른 **Text** 구성 요소를 추가합니다.
   * 견적을 포함하려면 구성 요소를 업데이트하십시오. &quot;Los Angeles보다 더 좋은 공유지는 없습니다.&quot;
   * 전체 화면 모드로 리치 텍스트 편집기를 편집하고 **견적 블록** 요소를 사용하도록 위의 견적을 수정하십시오.
1. 모형과 일치하도록 문서의 본문을 계속 채웁니다.
1. 문서의 PDF 버전을 사용하도록 **다운로드** 구성 요소를 구성합니다.
   * **다운로드** > **속성**&#x200B;에서 **DAM 에셋에서 제목 가져오기**&#x200B;에 대한 확인란을 클릭합니다.
   * **설명**&#x200B;을(를) &quot;전체 스토리 가져오기&quot;로 설정합니다.
   * **작업 텍스트**&#x200B;을(를) &quot;PDF 다운로드&quot;로 설정합니다.
1. **목록** 구성 요소를 구성합니다.
   * **목록 설정** > **목록 작성**&#x200B;에서 **하위 페이지**&#x200B;를 선택합니다.
   * **상위 페이지**&#x200B;을(를) `/content/wknd/us/en/magazine`(으)로 설정합니다.
   * 아래에서 **항목 설정**&#x200B;에서 **항목 연결**&#x200B;을 확인하고 **날짜 표시**&#x200B;를 확인합니다.

## 노드 구조 Inspect {#node-structure}

이 시점에서 기사 페이지는 확실히 스타일이 지정되지 않았습니다. 그러나 기본 구조는 제대로 되어 있습니다. 그런 다음 문서 페이지의 노드 구조를 검사하여 템플릿, 페이지 및 구성 요소의 역할을 더 잘 이해합니다.

로컬 AEM 인스턴스에서 CRXDE-Lite 도구를 사용하여 기본 노드 구조를 봅니다.

1. [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent)을(를) 열고 트리 탐색을 사용하여 `/content/wknd/us/en/magazine/guide-la-skateparks`(으)로 이동합니다.

1. `la-skateparks` 페이지 아래의 `jcr:content` 노드를 클릭하고 속성을 확인합니다.

   ![JCR 콘텐츠 속성](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   `cq:template`의 값을 확인합니다. 이 값은 이전에 만든 문서 페이지 템플릿인 `/conf/wknd/settings/wcm/templates/article-page`을(를) 가리킵니다.

   `wknd/components/page`을(를) 가리키는 `sling:resourceType`의 값도 확인합니다. AEM Project Archetype에서 만든 페이지 구성 요소이며 템플릿을 기반으로 페이지를 렌더링합니다.

1. `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` 아래의 `jcr:content` 노드를 확장하고 노드 계층 구조를 봅니다.

   ![JCR 콘텐츠 LA 스케이트보드장](assets/pages-templates/page-jcr-structure.png)

   작성된 구성 요소에 각 노드를 느슨하게 매핑할 수 있어야 합니다. `container` 접두사가 있는 노드를 검사하여 사용된 다른 레이아웃 컨테이너를 식별할 수 있는지 확인하십시오.

1. 그런 다음 `/apps/wknd/components/page`에서 페이지 구성 요소를 검사합니다. CRXDE Lite에서 구성 요소 속성 보기:

   ![페이지 구성 요소 속성](assets/pages-templates/page-component-properties.png)

   페이지 구성 요소 아래에는 HTL 스크립트 `customfooterlibs.html`과(와) `customheaderlibs.html`이(가) 두 개만 있습니다. *이 구성 요소가 페이지를 어떻게 렌더링합니까?*

   `sling:resourceSuperType` 속성은 `core/wcm/components/page/v2/page`을(를) 가리킵니다. 이 속성을 사용하면 WKND의 페이지 구성 요소는 핵심 구성 요소 페이지 구성 요소의 기능을 **모두**&#x200B;상속할 수 있습니다. 이는 [프록시 구성 요소 패턴](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern)이라는 이름의 첫 번째 예입니다. 자세한 내용은 [여기](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html)를 참조하세요.

1. WKND 구성 요소 내의 다른 구성 요소인 `/apps/wknd/components/breadcrumb`의 `Breadcrumb` 구성 요소를 Inspect에 추가했습니다. 동일한 `sling:resourceSuperType` 속성을 찾을 수 있지만 이번에는 `core/wcm/components/breadcrumb/v2/breadcrumb`을(를) 가리킵니다. 프록시 구성 요소 패턴을 사용하여 핵심 구성 요소를 포함하는 또 다른 예입니다. 실제로 WKND 코드 베이스의 모든 구성 요소는 AEM 핵심 구성 요소(사용자 지정 데모 HelloWorld 구성 요소 제외)의 프록시입니다. 사용자 지정 코드를 작성하기 *전*&#x200B;에 가능한 한 핵심 구성 요소의 기능을 많이 재사용하는 것이 좋습니다.

1. 다음으로 CRXDE Lite을 사용하여 `/libs/core/wcm/components/page/v2/page`의 핵심 구성 요소 페이지를 검사합니다.

   >[!NOTE]
   >
   > AEM 6.5/6.4에서 핵심 구성 요소는 `/apps/core/wcm/components` 아래에 있습니다. AEM as a Cloud Service에서 핵심 구성 요소는 `/libs` 아래에 있으며 자동으로 업데이트됩니다.

   ![핵심 구성 요소 페이지](assets/pages-templates/core-page-component-properties.png)

   이 페이지 아래에는 많은 스크립트 파일이 포함되어 있습니다. 핵심 구성 요소 페이지에는 다양한 기능이 포함되어 있습니다. 이 기능은 유지 관리 및 가독성을 높이기 위해 여러 스크립트로 나뉩니다. `page.html`을(를) 열고 `data-sly-include`을(를) 찾아 HTL 스크립트의 포함 여부를 추적할 수 있습니다.

   ```html
   <!--/* /libs/core/wcm/components/page/v2/page/page.html */-->
   <!DOCTYPE HTML>
   <html data-sly-use.page="com.adobe.cq.wcm.core.components.models.Page" lang="${page.language}"
       data-sly-use.head="head.html"
       data-sly-use.footer="footer.html"
       data-sly-use.redirect="redirect.html">
       <head data-sly-call="${head.head @ page = page}"></head>
       <body class="${page.cssClassNames}"
           id="${page.id}"
           data-cmp-data-layer-enabled="${page.data ? true : false}">
           <script data-sly-test.dataLayerEnabled="${page.data}">
           window.adobeDataLayer = window.adobeDataLayer || [];
           adobeDataLayer.push({
               page: JSON.parse("${page.data.json @ context='scriptString'}"),
               event:'cmp:show',
               eventInfo: {
                   path: 'page.${page.id @ context="scriptString"}'
               }
           });
           </script>
           <sly data-sly-test.isRedirectPage="${page.redirectTarget && (wcmmode.edit || wcmmode.preview)}"
               data-sly-call="${redirect.redirect @ redirectTarget = page.redirectTarget}"></sly>
           <sly data-sly-test="${!isRedirectPage}">
               <sly data-sly-include="body.skiptomaincontent.html"></sly>
               <sly data-sly-include="body.socialmedia_begin.html"></sly>
               <sly data-sly-include="body.html"></sly>
               <sly data-sly-call="${footer.footer @ page = page}"></sly>
               <sly data-sly-include="body.socialmedia_end.html"></sly>
           </sly>
       </body>
   </html>
   ```

   HTL을 여러 스크립트로 세분화하는 다른 이유는 프록시 구성 요소가 개별 스크립트를 재정의하여 사용자 지정 비즈니스 논리를 구현할 수 있도록 하기 위해서입니다. HTL 스크립트 `customfooterlibs.html` 및 `customheaderlibs.html`은(는) 프로젝트를 구현하여 재정의할 명시적 목적으로 만들어집니다.

   이 문서](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html)을(를) 읽고 Editable Template이 [콘텐츠 페이지의 렌더링에 어떤 영향을 미치는지 자세히 알아볼 수 있습니다.

1. `/libs/core/wcm/components/breadcrumb/v2/breadcrumb`의 탐색 표시와 같은 다른 핵심 구성 요소를 Inspect에 추가합니다. 이동 경로 구성 요소에 대한 마크업이 최종적으로 생성되는 방법을 이해하려면 `breadcrumb.html` 스크립트를 봅니다.

## Source 컨트롤에 구성 저장 {#configuration-persistence}

특히 AEM 프로젝트를 시작할 때 소스 제어에 템플릿 및 관련 콘텐츠 정책과 같은 구성을 유지하는 것이 중요합니다. 이렇게 하면 모든 개발자가 동일한 콘텐츠 및 구성 세트에 대해 작업하고 환경 간에 추가적인 일관성을 보장할 수 있습니다. 일단 프로젝트가 일정 수준의 완성도에 도달하면 템플릿을 관리하는 관행을 특별한 고급 사용자 그룹에게 이관할 수 있습니다.


지금은 템플릿이 다른 코드 조각처럼 처리되어 프로젝트의 일부로 **문서 페이지 템플릿**을(를) 동기화합니다.
지금까지 코드가 AEM 프로젝트에서 AEM의 로컬 인스턴스로 푸시되었습니다. **문서 페이지 템플릿**&#x200B;이(가) AEM의 로컬 인스턴스에서 직접 만들어졌으므로 템플릿을 AEM 프로젝트로 **가져오기**&#x200B;해야 합니다. **ui.content** 모듈은 이러한 특정 목적을 위해 AEM 프로젝트에 포함됩니다.

다음 몇 단계는 [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&amp;ssr=false#overview) 플러그인을 사용하여 VSCode IDE에서 수행됩니다. 그러나 **가져오기**&#x200B;하거나 AEM의 로컬 인스턴스에서 콘텐츠를 가져오도록 구성한 IDE를 사용하여 작업을 수행할 수 있습니다.

1. 에서 VSCode가 `aem-guides-wknd` 프로젝트를 엽니다.

1. 프로젝트 탐색기에서 **ui.content** 모듈을 확장합니다. `src` 폴더를 확장하고 `/conf/wknd/settings/wcm/templates`(으)로 이동합니다.

1. `templates` 폴더를 [!UICONTROL 마우스 오른쪽 단추로 클릭]하고 **AEM Server에서 가져오기**&#x200B;를 선택합니다.

   ![VSCode 가져오기 템플릿](assets/pages-templates/vscode-import-templates.png)

   `article-page`을(를) 가져와야 하며 `page-content`, `xf-web-variation` 템플릿도 업데이트해야 합니다.

   ![업데이트된 템플릿](assets/pages-templates/updated-templates.png)

1. 콘텐츠를 가져오는 단계를 반복하되 `/conf/wknd/settings/wcm/policies`에서 **정책** 폴더를 선택하십시오.

   ![VSCode 가져오기 정책](assets/pages-templates/policies-article-page-template.png)

1. `ui.content/src/main/content/META-INF/vault/filter.xml`에서 `filter.xml` 파일을 Inspect합니다.

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
   <workspaceFilter version="1.0">
       <filter root="/conf/wknd" mode="merge"/>
       <filter root="/content/wknd" mode="merge"/>
       <filter root="/content/dam/wknd" mode="merge"/>
       <filter root="/content/experience-fragments/wknd" mode="merge"/>
   </workspaceFilter>
   ```

   `filter.xml` 파일은 패키지와 함께 설치된 노드의 경로를 식별합니다. 각 필터에서 `mode="merge"`을(를) 확인합니다. 이는 기존 콘텐츠를 수정하지 않고 새 콘텐츠만 추가함을 나타냅니다. 콘텐츠 작성자가 이러한 경로를 업데이트할 수 있으므로 코드 배포는 콘텐츠를 **덮어쓰지**&#x200B;하지 않는 것이 중요합니다. 필터 요소 작업에 대한 자세한 내용은 [FileVault 설명서](https://jackrabbit.apache.org/filevault/filter.html)를 참조하십시오.

   `ui.content/src/main/content/META-INF/vault/filter.xml`과(와) `ui.apps/src/main/content/META-INF/vault/filter.xml`을(를) 비교하여 각 모듈에서 관리하는 다른 노드를 이해합니다.

   >[!WARNING]
   >
   > WKND 참조 사이트에 대한 일관된 배포를 보장하기 위해 프로젝트의 일부 분기가 `ui.content`에서 JCR의 모든 변경 사항을 덮어쓰도록 설정됩니다. 이는 코드/스타일이 특정 정책을 위해 작성되기 때문에 설계에 의해, 즉 솔루션 분기에 대해 수행됩니다.

## 축하합니다! {#congratulations}

축하합니다. Adobe Experience Manager Sites으로 템플릿과 페이지를 만들었습니다.

### 다음 단계 {#next-steps}

이 시점에서 기사 페이지는 확실히 스타일이 지정되지 않았습니다. [클라이언트측 라이브러리 및 프론트엔드 워크플로](client-side-libraries.md) 튜토리얼을 따라 CSS와 JavaScript을 포함하여 글로벌 스타일을 사이트에 적용하고 전용 프론트엔드 빌드를 통합하는 모범 사례에 대해 알아보십시오.

[GitHub](https://github.com/adobe/aem-guides-wknd)에서 완료된 코드를 보거나 Git 분기 `tutorial/pages-templates-solution`에서 로컬로 코드를 검토하고 배포합니다.

1. [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 리포지토리를 복제합니다.
1. `tutorial/pages-templates-solution` 분기를 확인하십시오.
