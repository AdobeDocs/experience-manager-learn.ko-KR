---
title: AEM 사이트 시작하기 - 페이지 및 템플릿
description: 기본 페이지 구성 요소와 편집 가능한 템플릿 간의 관계에 대해 알아봅니다. 핵심 구성 요소가 프로젝트에서 어떻게 프록시로 기능하는지 이해합니다. Adobe XD의 목업을 기반으로 잘 구성된 문서 페이지 템플릿을 빌드하기 위해 편집 가능한 템플릿의 고급 정책 구성을 알아봅니다.
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
workflow-type: ht
source-wordcount: '2898'
ht-degree: 100%

---

# 페이지 및 템플릿 {#pages-and-template}

{{edge-delivery-services-and-page-editor}}

이 챕터에서는 기본 페이지 구성 요소와 편집 가능한 템플릿 간의 관계를 살펴보겠습니다. [Adobe XD](https://helpx.adobe.com/kr/support/xd.html)의 일부 목업을 기반으로 스타일이 지정되지 않은 기사 템플릿을 만드는 방법을 알아봅니다. 템플릿을 빌드하는 과정에서 편집 가능한 템플릿의 핵심 구성 요소와 고급 정책 구성에 대해 다룹니다.

## 사전 요구 사항 {#prerequisites}

[로컬 개발 환경](overview.md#local-dev-environment)을 설정하는 데 필요한 도구와 지침을 검토합니다.

### 스타터 프로젝트

>[!NOTE]
>
> 이전 챕터를 성공적으로 완료했다면 프로젝트를 재사용하고 스타터 프로젝트를 체크아웃하는 단계를 건너뛸 수 있습니다.

튜토리얼에서 기반으로 삼는 기본 코드를 확인합니다.

1. `tutorial/pages-templates-start`[GitHub](https://github.com/adobe/aem-guides-wknd)의 분기를 확인합니다.

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
   > AEM 6.5 또는 6.4를 사용하는 경우 `classic` 프로필을 모든 Maven 명령에 추가합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

완성된 코드는 언제든지 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/pages-templates-solution)에서 볼 수 있으며 `tutorial/pages-templates-solution` 분기로 전환하여 로컬에서 코드를 확인할 수도 있습니다.

## 목표

1. Adobe XD에서 만든 페이지 디자인을 검사하고 이를 핵심 구성 요소에 매핑합니다.
1. 편집 가능한 템플릿의 세부 사항과 정책을 사용하여 페이지 콘텐츠에 대한 세부적인 제어를 시행하는 방법을 알아봅니다.
1. 템플릿과 페이지가 어떻게 연결되는지 알아봅니다.

## 빌드 결과물 {#what-build}

튜토리얼의 이 부분에서는 문서 페이지를 만드는 데 사용할 수 있고 일반적인 구조에 맞는 새로운 문서 페이지 템플릿을 만들어 보겠습니다. 문서 페이지 템플릿은 Adobe XD에서 제작된 디자인과 UI 키트를 기반으로 합니다. 이 챕터에서는 템플릿의 구조나 뼈대를 구성하는 데에만 초점을 맞춥니다. 스타일은 구현되지 않았지만 템플릿과 페이지는 기능적입니다.

![문서 페이지 디자인 및 스타일이 지정되지 않은 버전](assets/pages-templates/what-you-will-build.png)

## Adobe XD를 사용한 UI 계획 수립 {#adobexd}

일반적으로 새로운 웹 사이트를 계획하는 작업은 목업 및 정적인 디자인에서 시작합니다. [Adobe XD](https://helpx.adobe.com/kr/support/xd.html)는 사용자 경험을 빌드하는 디자인 도구입니다. 다음으로는 문서 페이지 템플릿의 구조를 계획하는 데 도움이 되는 UI 키트와 목업을 살펴보겠습니다.

>[!VIDEO](https://video.tv.adobe.com/v/36124?quality=12&learn=on&captions=kor)

**[WKND 문서 디자인 파일](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)을 다운로드**&#x200B;합니다.

>[!NOTE]
>
> 일반적인 [AEM 핵심 구성 요소 UI 키트를 사용](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd?lang=ko)하여 사용자 정의 프로젝트를 시작할 수도 있습니다.

## 문서 페이지 템플릿 만들기

페이지를 만들 때는 페이지를 만드는 기준으로 사용할 템플릿을 선택해야 합니다. 템플릿은 결과 페이지의 구조, 초기 콘텐츠 및 허용되는 구성 요소를 정의합니다.

[편집 가능한 템플릿](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html?lang=ko)에는 세 가지 주요 영역이 있습니다.

1. **구조** - 템플릿의 일부인 구성 요소를 정의합니다. 이러한 구성 요소는 콘텐츠 작성자가 편집할 수 없습니다.
1. **초기 콘텐츠** - 템플릿에서 시작 단계에 사용하는 구성 요소를 정의합니다. 이러한 구성 요소는 콘텐츠 작성자가 편집 및/또는 삭제할 수 있습니다.
1. **정책** - 구성 요소의 동작 방식과 작성자가 사용할 수 있는 옵션에 대한 구성을 정의합니다.

다음으로 목업의 구조와 일치하는 템플릿을 AEM에서 만듭니다. 이 작업은 AEM의 로컬 인스턴스에서 이루어집니다. 아래의 비디오에서 안내된 단계를 따릅니다.

>[!VIDEO](https://video.tv.adobe.com/v/36123?quality=12&learn=on&captions=kor)

위 비디오에 대한 상위 수준 단계:

### 구조 구성

1. **문서 페이지**&#x200B;라는 **페이지 템플릿 유형**&#x200B;을 사용하여 템플릿을 만듭니다.
1. **구조** 모드로 전환합니다.
1. 템플릿 상단에서 **머리글** 역할을 하는 **경험 조각** 구성 요소를 추가합니다.
   * 구성 요소가 `/content/experience-fragments/wknd/us/en/site/header/master`를 가리키도록 구성합니다.
   * **페이지 머리글**&#x200B;에 대한 정책을 설정하고 **기본 구성 요소**&#x200B;가 `header`로 설정되도록 합니다. 다음 챕터에서 `header` 구성 요소는 CSS를 통해 타기팅됩니다.
1. 템플릿 하단에서 **바닥글** 역할을 하는 **경험 조각** 구성 요소를 추가합니다.
   * 구성 요소가 `/content/experience-fragments/wknd/us/en/site/footer/master`를 가리키도록 구성합니다.
   * **페이지 바닥글**&#x200B;에 대한 정책을 설정하고 **기본 구성 요소**&#x200B;가 `footer`로 설정되도록 합니다. 다음 챕터에서 `footer` 구성 요소는 CSS를 통해 타기팅됩니다.
1. 템플릿이 처음 생성될 때 포함된 **주** 컨테이너를 잠급니다.
   * **주 페이지**&#x200B;에 대한 정책을 설정하고 **기본 구성 요소**&#x200B;가 `main`로 설정되도록 합니다. 다음 챕터에서 `main` 구성 요소는 CSS를 통해 타기팅됩니다.
1. **이미지** 구성 요소를 **주** 컨테이너에 추가합니다.
   * **이미지** 구성 요소를 잠금 해제합니다.
1. 주 컨테이너의 **이미지** 구성 요소 아래에 **이동 경로** 구성 요소를 추가합니다.
   * **문서 페이지 - 이동 경로**&#x200B;라는 **이동 경로** 구성 요소에 대한 정책을 만듭니다. **탐색 시작 수준**&#x200B;을 **4**&#x200B;로 설정합니다.
1. **이동 경로** 구성 요소 아래 및 **주** 컨테이너 내부에 **컨테이너** 구성 요소를 추가합니다. 이 구성 요소는 템플릿에 대해 **콘텐츠 컨테이너** 역할을 담당합니다.
   * **콘텐츠** 컨테이너를 잠금 해제합니다.
   * **페이지 콘텐츠**&#x200B;에 대한 정책을 설정합니다.
1. **콘텐츠 컨테이너** 아래에 또 다른 **컨테이너** 구성 요소를 추가합니다. 이 구성 요소는 템플릿에 대한 **측면 레일** 컨테이너 역할을 담당합니다.
   * **측면 레일** 컨테이너를 잠금 해제합니다.
   * **문서 페이지 - 측면 레일**&#x200B;이라는 정책을 만듭니다.
   * **WKND Sites 프로젝트 - 콘텐츠** 아래에서 **허용된 구성 요소**&#x200B;를 구성하여 **버튼**, **다운로드**, **이미지**, **목록**, **구분자**, **소셜 미디어 공유**, **텍스트**, **제목**&#x200B;을 포함하도록 합니다.
1. 페이지 루트 컨테이너의 정책을 업데이트합니다. 이 컨테이너는 템플릿의 가장 바깥쪽 컨테이너입니다. **페이지 루트**&#x200B;에 대한 정책을 설정합니다.
   * **컨테이너 설정**&#x200B;에서 **레이아웃**&#x200B;을 **반응형 그리드**&#x200B;로 설정합니다.
1. **콘텐츠 컨테이너**&#x200B;에 대한 레이아웃 모드를 활성화합니다. 핸들을 오른쪽에서 왼쪽으로 끌어서 컨테이너를 8열 너비로 줄입니다.
1. **측면 레일 컨테이너**&#x200B;에 대한 레이아웃 모드를 활성화합니다. 핸들을 오른쪽에서 왼쪽으로 끌어서 컨테이너를 4열 너비로 줄입니다. 그런 다음 왼쪽 핸들을 왼쪽에서 오른쪽으로 한 열 끌어서 컨테이너 너비를 3열로 만들고 **콘텐츠 컨테이너** 사이에 1열 간격을 둡니다.
1. 모바일 에뮬레이터를 열고 모바일 중단점으로 전환합니다. 레이아웃 모드를 다시 실행하고 **콘텐츠 컨테이너**&#x200B;와 **측면 레일 컨테이너**&#x200B;를 페이지의 전체 너비로 설정합니다. 이렇게 하면 모바일 중단점에서 컨테이너가 수직으로 쌓입니다.
1. **콘텐츠 컨테이너**&#x200B;에 위치한 **텍스트** 구성 요소의 정책을 업데이트합니다.
   * **콘텐츠 텍스트**&#x200B;에 대한 정책을 설정합니다.
   * **플러그인** > **단락 스타일**&#x200B;에서 **단락 스타일 활성화**&#x200B;를 선택하고 **인용 블록**&#x200B;이 활성화되어 있는지 확인합니다.

### 초기 콘텐츠 구성

1. **초기 콘텐츠** 모드로 전환합니다.
1. **제목** 구성 요소를 **콘텐츠 컨테이너**&#x200B;에 추가합니다. 이 구성 요소는 문서 제목으로 사용됩니다. 비어 있는 경우 현재 페이지의 제목이 자동으로 표시됩니다.
1. 첫 번째 제목 구성 요소 아래에 두 번째 **제목** 구성 요소를 추가합니다.
   * &#39;작성자별&#39;이라는 텍스트로 구성 요소를 구성합니다. 이는 텍스트 플레이스홀더입니다.
   * 유형을 `H4`로 설정합니다.
1. **작성자별** 제목 구성 요소 아래에 **텍스트** 구성 요소를 추가합니다.
1. **제목** 구성 요소를 **측면 레일 컨테이너**&#x200B;에 추가합니다.
   * &#39;이 스토리 공유&#39;라는 텍스트로 구성 요소를 구성합니다.
   * 유형을 `H5`로 설정합니다.
1. **이 스토리 공유** 제목 구성 요소 아래에 **소셜 미디어 공유** 구성 요소를 추가합니다.
1. **소셜 미디어 공유** 구성 요소 아래에 **구분자** 구성 요소를 추가합니다.
1. **구분자** 구성 요소 아래에 **다운로드** 구성 요소를 추가합니다.
1. **다운로드** 구성 요소 아래에 **목록** 구성 요소를 추가합니다.
1. 템플릿의 **초기 페이지 속성**&#x200B;을 업데이트합니다.
   * **소셜 미디어** > **소셜 미디어 공유**&#x200B;에서 **Facebook** 및 **Pinterest**&#x200B;를 선택합니다.

### 템플릿을 활성화하고 썸네일을 추가합니다.

1. [http://localhost:4502libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)로 이동하여 템플릿 콘솔에서 템플릿을 확인합니다.
1. 문서 페이지 템플릿을 **활성화**&#x200B;합니다.
1. 문서 페이지 템플릿의 속성을 편집하고 다음 썸네일을 업로드하면 문서 페이지 템플릿을 사용하여 만든 페이지를 빠르게 식별할 수 있습니다.

   ![문서 페이지 템플릿 썸네일](assets/pages-templates/article-page-template-thumbnail.png)

## 경험 조각으로 머리글 및 바닥글 업데이트 {#experience-fragments}

헤더나 푸터와 같은 글로벌 콘텐츠를 만들 때 일반적인 관행은 [경험 조각](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=ko)을 사용하는 것입니다. 경험 조각을 사용하면 사용자가 여러 구성 요소를 결합하여 참조 가능한 단일 구성 요소를 만들 수 있습니다. 경험 조각은 다중 사이트 관리 및 [현지화](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/experience-fragment.html?lang=ko)를 지원한다는 장점이 있습니다.

AEM Project Archetype이 머리글과 바닥글을 생성했습니다. 다음으로 목업과 일치하도록 경험 조각을 업데이트합니다. 아래의 비디오에서 안내된 단계를 따릅니다.

>[!VIDEO](https://video.tv.adobe.com/v/3447496?quality=12&learn=on&captions=kor)

위 비디오에 대한 상위 수준 단계:

1. 샘플 콘텐츠 패키지인 **[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets-1.1.zip)**&#x200B;을 다운로드합니다.
1. [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)에서 패키지 관리자를 사용하여 콘텐츠 패키지를 업로드하고 설치합니다.
1. [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html)에서 경험 조각에 사용되는 템플릿인 웹 베리에이션 템플릿을 업데이트합니다.
   * 템플릿에서 **컨테이너** 구성 요소 정책을 업데이트합니다.
   * **XF 루트**&#x200B;에 대한 정책을 설정합니다.
   * **허용된 구성 요소**&#x200B;에서 구성 요소 그룹 **WKND Sites 프로젝트 - 구조**&#x200B;를 선택하여 **언어 탐색**, **탐색**, **빠른 검색** 구성 요소를 포함합니다.

### 머리글 경험 조각 업데이트

1. [http://localhost:4502editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html)에서 머리글을 렌더링하는 경험 조각을 엽니다.
1. 조각의 루트 **컨테이너**&#x200B;를 구성합니다. 이 컨테이너는 가장 바깥쪽 **컨테이너**&#x200B;입니다.
   * **레이아웃**&#x200B;을 **반응형 그리드**&#x200B;로 설정합니다.
1. **컨테이너**&#x200B;의 상단에 **WKND 다크 로고**&#x200B;를 이미지로 추가합니다. 로고는 이전 단계에서 설치된 패키지에 포함되었습니다.
   * **WKND 다크 로고**&#x200B;의 레이아웃을 **2**&#x200B;열 너비로 수정합니다. 핸들을 오른쪽에서 왼쪽으로 드래그합니다.
   * 로고를 &#39;WKND 로고&#39;라는 **대체 텍스트**&#x200B;로 로고를 구성합니다.
   * `/content/wknd/us/en` 홈 페이지로 **연결**&#x200B;되도록 로고를 구성합니다.
1. 이미 페이지에 배치된 **탐색** 구성 요소를 구성합니다.
   * **루트 수준 제외**&#x200B;를 **1**&#x200B;로 설정합니다.
   * **탐색 구조 깊이**&#x200B;를 **1**&#x200B;로 설정합니다.
   * **탐색** 구성 요소의 레이아웃을 **8**&#x200B;열 너비로 수정합니다. 핸들을 오른쪽에서 왼쪽으로 드래그합니다.
1. **언어 탐색** 구성 요소를 제거합니다.
1. **검색** 구성 요소의 레이아웃을 **2**&#x200B;열 너비로 수정합니다. 핸들을 오른쪽에서 왼쪽으로 드래그합니다. 이제 모든 구성 요소를 한 행에 수평으로 정렬해야 합니다.

### 바닥글 경험 조각 업데이트

1. [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)에서 바닥글을 렌더링하는 경험 조각을 엽니다.
1. 조각의 루트 **컨테이너**&#x200B;를 구성합니다. 이 컨테이너는 가장 바깥쪽 **컨테이너**&#x200B;입니다.
   * **레이아웃**&#x200B;을 **반응형 그리드**&#x200B;로 설정합니다.
1. **컨테이너**&#x200B;의 상단에 **WKND 라이트 로고**&#x200B;를 이미지로 추가합니다. 로고는 이전 단계에서 설치된 패키지에 포함되었습니다.
   * **WKND 라이트 로고**&#x200B;의 레이아웃을 **2**&#x200B;열 너비로 수정합니다. 핸들을 오른쪽에서 왼쪽으로 드래그합니다.
   * 로고를 &#39;WKND 로고 라이트&#39;라는 **대체 텍스트**&#x200B;로 로고를 구성합니다.
   * `/content/wknd/us/en` 홈 페이지로 **연결**&#x200B;되도록 로고를 구성합니다.
1. 로고 아래에 **탐색** 구성 요소를 추가합니다. **탐색** 구성 요소를 다음과 같이 구성합니다.
   * **루트 수준 제외**&#x200B;를 **1**&#x200B;로 설정합니다.
   * **모든 하위 페이지 수집**&#x200B;을 선택 해제합니다.
   * **탐색 구조 깊이**&#x200B;를 **1**&#x200B;로 설정합니다.
   * **탐색** 구성 요소의 레이아웃을 **8**&#x200B;열 너비로 수정합니다. 핸들을 오른쪽에서 왼쪽으로 드래그합니다.

## 문서 페이지 만들기

다음으로, 문서 페이지 템플릿을 사용하여 페이지를 만듭니다. 사이트 목업에 맞게 페이지 콘텐츠를 작성합니다. 아래의 비디오에서 안내된 단계를 따릅니다.

>[!VIDEO](https://video.tv.adobe.com/v/3446449?quality=12&learn=on&captions=kor)

위 비디오에 대한 상위 수준 단계:

1. [http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine)에서 사이트 콘솔로 이동합니다.
1. **WKND** > **US** > **EN** > **매거진** 아래에서 페이지를 만듭니다.
   * **문서 페이지** 템플릿을 선택합니다.
   * **속성**&#x200B;에서 **제목**&#x200B;을 &#39;LA 스케이트보드장 완벽 가이드&#39;로 설정합니다.
   * **이름**&#x200B;을 &#39;guide-la-skateparks&#39;로 설정합니다.
1. **작성자별** 제목을 &#39;Stacey Roswells 작성&#39;으로 바꿉니다.
1. 문서를 채울 문단을 포함하도록 **텍스트** 구성 요소를 업데이트합니다. [la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt) 텍스트 파일을 복사본으로 사용할 수 있습니다.
1. 또 다른 **텍스트** 구성 요소를 추가합니다.
   * 구성 요소를 업데이트하여 &#39;로스앤젤레스보다 기술을 선보이기에 더 좋은 곳은 없습니다.&#39;라는 인용문을 포함합니다.
   * 전체 화면 모드에서 리치 텍스트 편집기를 편집하고 위의 인용문을 수정하여 **인용 블록** 요소를 사용합니다.
1. 목업에 맞게 문서 본문을 계속 채웁니다.
1. PDF 버전의 문서를 사용하려면 **다운로드** 구성 요소를 구성합니다.
   * **다운로드** > **속성**&#x200B;에서 확인란을 클릭하여 **DAM 자산에서 제목을 가져옵니다**.
   * **설명**&#x200B;을 &#39;전체 스토리 보기&#39;로 설정합니다.
   * **작업 텍스트**&#x200B;를 &#39;PDF 다운로드&#39;로 설정합니다.
1. **목록** 구성 요소를 구성합니다.
   * **목록 설정** > **목록 작성 도구**&#x200B;에서 **하위 페이지**&#x200B;를 선택합니다.
   * **상위 페이지**&#x200B;를 `/content/wknd/us/en/magazine`으로 설정합니다.
   * **항목 설정**&#x200B;에서 **항목 연결**&#x200B;을 선택하고 **날짜 표시**&#x200B;를 선택합니다.

## 노드 구조 검사 {#node-structure}

이 시점에서는 문서 페이지 스타일이 지정되지 않은 상태입니다. 하지만 기본 구조는 이미 갖춰져 있습니다. 다음으로 템플릿, 페이지, 구성 요소의 역할을 더 잘 이해하기 위해 문서 페이지의 노드 구조를 살펴봅니다.

로컬 AEM 인스턴스에서 CRXDE-Lite 도구를 사용하여 기본 노드 구조를 확인합니다.

1. [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent)를 열고 트리 탐색을 사용하여 `/content/wknd/us/en/magazine/guide-la-skateparks`로 이동합니다.

1. `la-skateparks` 페이지 아래에서 `jcr:content` 노드를 클릭하고 속성을 확인합니다.

   ![JCR 콘텐츠 속성](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   앞서 만든 문서 페이지 템플릿인 `/conf/wknd/settings/wcm/templates/article-page`를 가리키는 `cq:template`의 값을 확인합니다.

   또한 `wknd/components/page`를 가리키는 `sling:resourceType`의 값도 확인합니다. AEM Project Archetype에 의해 생성된 페이지 구성 요소로, 템플릿을 기반으로 페이지를 렌더링하는 역할을 합니다.

1. `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` 아래에서 `jcr:content`를 확장하고 노드 계층을 확인합니다.

   ![JCR 콘텐츠 LA 스케이트보드장](assets/pages-templates/page-jcr-structure.png)

   각 노드는 작성된 구성 요소에 느슨하게 매핑할 수 있습니다. `container` 접두사가 붙은 노드를 검사하여 다양한 레이아웃 컨테이너가 사용되는지 확인합니다.

1. 다음으로 `/apps/wknd/components/page`에서 페이지 구성 요소를 검사합니다. CRXDE Lite에서 구성 요소 속성을 확인합니다.

   ![페이지 구성 요소 속성](assets/pages-templates/page-component-properties.png)

   HTL 스크립트는 페이지 구성 요소 아래에 `customfooterlibs.html` 및 `customheaderlibs.html` 두 개만 있습니다. *그렇다면 이 구성 요소는 페이지를 어떻게 렌더링할까요?*

   `sling:resourceSuperType` 속성은 `core/wcm/components/page/v2/page`를 가리킵니다. 이 속성을 사용하면 WKND의 페이지 구성 요소가 핵심 구성 요소 페이지 구성 요소의 **모든** 기능을 상속할 수 있습니다. 이는 [프록시 구성 요소 패턴](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html?lang=ko#ProxyComponentPattern)이라 불리는 경우의 첫 번째 예시입니다. 자세한 내용은 [여기](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html?lang=ko)에서 확인할 수 있습니다.

1. `/apps/wknd/components/breadcrumb`에서 WKND 구성 요소인 `Breadcrumb` 구성 요소 내 또 다른 구성 요소를 검사합니다. 동일한 `sling:resourceSuperType` 속성을 찾을 수 있지만 이번에는 이 속성이 `core/wcm/components/breadcrumb/v2/breadcrumb`를 가리킵니다. 이는 핵심 구성 요소를 포함하기 위해 프록시 구성 요소 패턴을 사용하는 또 다른 예시입니다. 실제로 WKND 코드 베이스의 모든 구성 요소는 AEM 핵심 구성 요소의 프록시입니다(사용자 정의 데모 HelloWorld 구성 요소 제외). 사용자 정의 코드를 작성하기 *전*&#x200B;에 가능한 한 핵심 구성 요소의 기능을 재사용하는 것이 가장 좋습니다.

1. 다음으로 `/libs/core/wcm/components/page/v2/page`에서 CRXDE Lite를 사용하여 핵심 구성 요소를 검사합니다.

   >[!NOTE]
   >
   > AEM 6.5/6.4에서는 핵심 구성 요소가 `/apps/core/wcm/components`에 위치합니다. AEM as a Cloud Service에서 핵심 구성 요소는 `/libs`에 위치하며 자동으로 업데이트됩니다.

   ![핵심 구성 요소 페이지](assets/pages-templates/core-page-component-properties.png)

   이 페이지 아래에는 많은 스크립트 파일이 포함되어 있습니다. 핵심 구성 요소 페이지에는 다양한 기능이 포함되어 있습니다. 이 기능은 유지 관리와 가독성을 높이기 위해 여러 스크립트로 나뉩니다. HTL 스크립트가 포함되었는지 추적하려면 `page.html`을 열고 `data-sly-include`를 찾습니다.

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

   HTL을 여러 스크립트로 나누는 또 다른 이유는 프록시 구성 요소가 개별 스크립트를 재정의하여 사용자 정의 비즈니스 로직을 구현할 수 있도록 하기 위함입니다. HTL 스크립트 `customfooterlibs.html` 및 `customheaderlibs.html`은 구현 프로젝트에서 재정의하기 위함이라는 명시적인 목적에 따라 만들어졌습니다.

   [이 문서를 읽으면 콘텐츠 페이지](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html?lang=ko)의 렌더링에 편집 가능한 템플릿 요소가 어떻게 적용되는지 자세히 알아볼 수 있습니다.

1. `/libs/core/wcm/components/breadcrumb/v2/breadcrumb`에서 이동 경로와 같은 다른 핵심 구성 요소를 검사합니다. `breadcrumb.html` 스크립트를 확인하여 이동 경로 구성 요소에 대한 마크업이 최종적으로 어떻게 생성되는지 이해합니다.

## 소스 제어에 대한 구성 저장 {#configuration-persistence}

특히 AEM 프로젝트를 시작할 때 템플릿과 관련 콘텐츠 정책 등의 구성을 소스 제어에 유지하는 것이 중요합니다. 이렇게 하면 모든 개발자가 동일한 콘텐츠와 구성으로 작업할 수 있으며 환경 간의 일관성을 더욱 높일 수 있습니다. 프로젝트가 일정 수준의 성숙도에 도달하면 템플릿 관리 업무를 특정 전문 사용자 그룹에 인계할 수 있습니다.


지금은 템플릿이 다른 코드 조각처럼 처리되고 **문서 페이지 템플릿**이 프로젝트의 일부로 원격 동기화됩니다.
지금까지 코드는 AEM 프로젝트에서 AEM의 로컬 인스턴스로 푸시되었습니다. **문서 페이지 템플릿**&#x200B;은 AEM의 로컬 인스턴스에서 직접 생성되었으므로 템플릿을 AEM 프로젝트로 **가져오기**&#x200B;해야 합니다. **ui.content** 모듈은 이 특정 목적을 위해 AEM 프로젝트에 포함되었습니다.

다음 몇 단계는 VSCode IDE에서 [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&ssr=false#overview) 플러그인을 사용하여 수행됩니다. 한편 이 단계들은 AEM 로컬 인스턴스에서 콘텐츠를 **가져오기** 또는 내보내기할 수 있도록 구성한 IDE를 사용하여 수행할 수도 있습니다.

1. VSCode에서 `aem-guides-wknd` 프로젝트를 엽니다.

1. 프로젝트 탐색기에서 **ui.content** 모듈을 확장합니다. `src` 폴더를 확장하여 `/conf/wknd/settings/wcm/templates`로 이동합니다.

1. `templates` 폴더를 [!UICONTROL 마우스 오른쪽 버튼으로 클릭]라고 **AEM 서버에서 가져오기**&#x200B;를 선택합니다.

   ![VSCode 가져오기 템플릿](assets/pages-templates/vscode-import-templates.png)

   `article-page` 가져오기가 완료되며 `page-content`, `xf-web-variation` 템플릿 또한 업데이트됩니다.

   ![업데이트된 템플릿](assets/pages-templates/updated-templates.png)

1. 콘텐츠를 가져오는 단계를 반복하되 이번에는 `/conf/wknd/settings/wcm/policies`의 **정책** 폴더를 선택합니다.

   ![VSCode 가져오기 정책](assets/pages-templates/policies-article-page-template.png)

1. `ui.content/src/main/content/META-INF/vault/filter.xml`의 `filter.xml` 파일을 검사합니다.

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

   `filter.xml` 파일은 패키지와 함께 설치된 노드의 경로를 식별하는 역할을 합니다. 각 필터에서 기존 콘텐츠는 수정되지 않고 새로운 콘텐츠만 추가되었음을 의미하는 `mode="merge"`를 확인합니다. 콘텐츠 작성자가 이러한 경로를 업데이트할 수 있으므로 코드 배포 시 콘텐츠를 덮어쓰지 **않는** 것이 중요합니다. 필터 요소를 사용하는 작업에 대한 자세한 내용은 [FileVault 문서](https://jackrabbit.apache.org/filevault/filter.html)에서 확인할 수 있습니다.

   `ui.content/src/main/content/META-INF/vault/filter.xml` 및 `ui.apps/src/main/content/META-INF/vault/filter.xml`을 비교하여 각 모듈에서 관리하는 다양한 노드를 이해합니다.

   >[!WARNING]
   >
   > WKND 참조 사이트의 일관된 배포를 보장하기 위해 프로젝트의 일부 분기에서는 `ui.content`가 JCR의 모든 변경 사항을 덮어쓰도록 설정되었습니다. 이는 솔루션 분기를 위한 설계상 설정으로, 코드/스타일은 특정 정책에 맞게 작성됩니다.

## 축하합니다! {#congratulations}

축하합니다. Adobe Experience Manager Sites를 사용하여 템플릿과 페이지를 만들었습니다.

### 다음 단계 {#next-steps}

이 시점에서는 문서 페이지 스타일이 지정되지 않은 상태입니다. [클라이언트측 라이브러리 및 프론트엔드 워크플로](client-side-libraries.md) 튜토리얼을 참고하여 사이트에 전역적으로 스타일을 적용하고 전용 프론트엔드 빌드를 통합하기 위해 CSS와 JavaScript를 포함하는 모범 사례를 알아보시기 바랍니다.

완성된 코드를 [GitHub](https://github.com/adobe/aem-guides-wknd)에서 확인하거나 로컬로 `tutorial/pages-templates-solution` Git 분기에서 코드를 검토 및 배포할 수 있습니다.

1. [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 저장소를 복제합니다.
1. `tutorial/pages-templates-solution` 분기를 확인합니다.
