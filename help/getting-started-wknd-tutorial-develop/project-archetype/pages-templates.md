---
title: AEM Sites 시작하기 - 페이지 및 템플릿
seo-title: AEM Sites 시작하기 - 페이지 및 템플릿
description: 기본 페이지 구성 요소와 편집 가능한 템플릿 간의 관계에 대해 알아봅니다. 핵심 구성 요소가 프로젝트에 프록시되는 방법을 이해하고 편집 가능한 템플릿의 고급 정책 구성을 배워서 Adobe XD의 모집단 을 기반으로 잘 구성된 문서 페이지 템플릿을 작성합니다.
sub-product: 사이트
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: 핵심 구성 요소, 편집 가능한 템플릿, 페이지 편집기
topic: 컨텐츠 관리, 개발
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 4082
thumbnail: 30214.jpg
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '3100'
ht-degree: 0%

---


# 페이지 및 템플릿 {#pages-and-template}

이 장에서는 기본 페이지 구성 요소와 편집 가능한 템플릿 간의 관계를 살펴봅니다. [AdobeXD](https://www.adobe.com/products/xd.html)의 몇 가지 샘플을 기반으로 스타일이 지정되지 않은 문서 템플릿을 작성하겠습니다. 템플릿을 작성하는 과정에서 편집 가능한 템플릿의 핵심 구성 요소 및 고급 정책 구성이 다룹니다.

## 전제 조건 {#prerequisites}

[로컬 개발 환경](overview.md#local-dev-environment)을 설정하는 데 필요한 도구 및 지침을 검토하십시오.

### 스타터 프로젝트

>[!NOTE]
>
> 이전 장을 성공적으로 완료한 경우 프로젝트를 다시 사용하고 시작 프로젝트를 체크 아웃하는 단계를 건너뛸 수 있습니다.

자습서가 빌드하는 기본 라인 코드를 확인합니다.

1. [GitHub](https://github.com/adobe/aem-guides-wknd)에서 `tutorial/pages-templates-start` 분기를 확인하십시오

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

항상 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/pages-templates/solution)에서 완료된 코드를 보거나 분기 `tutorial/pages-templates-solution`로 전환하여 로컬로 코드를 체크 아웃할 수 있습니다.

## 목표

1. Inspect Adobe XD에서 만든 페이지 디자인을 만들어 핵심 구성 요소에 매핑합니다.
1. 편집 가능한 템플릿의 세부 사항과 정책을 사용하여 페이지 컨텐츠를 세부적으로 제어하는 방법을 알아봅니다.
1. 템플릿 및 페이지 연결 방법 알아보기

## 빌드할 내용 {#what-you-will-build}

자습서의 이 부분에서 새 문서 페이지를 만들고 공통 구조에 맞게 조정하는 데 사용할 수 있는 새 문서 페이지 템플릿을 만듭니다. 문서 페이지 템플릿은 Adobe XD에서 만들어진 UI 키트 및 디자인을 기반으로 합니다. 이 장은 템플릿의 구조나 골격을 빌드하는 데만 중점을 둡니다. 스타일이 구현되지 않지만 템플릿 및 페이지가 작동합니다.

![문서 페이지 디자인 및 스타일이 지정되지 않은 버전](assets/pages-templates/what-you-will-build.png)

## Adobe XD을 사용한 UI 계획 {#adobexd}

대부분의 경우, 새 웹 사이트를 계획하는 것은 모집단 및 정적 디자인부터 시작합니다. [Adobe ](https://www.adobe.com/products/xd.html) XD는 사용자 경험을 구축하는 디자인 도구입니다. 다음으로 문서 페이지 템플릿의 구조를 계획하는 데 도움이 되도록 UI 키트 및 샘플을 검사합니다.

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

**WKND  [문서 디자인 파일을 다운로드합니다](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**.

>[!NOTE]
>
> 일반 [AEM 코어 구성 요소 UI 키트도 사용자 지정 프로젝트의 시작점으로 사용할 수 있습니다](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd).

## 문서 페이지 템플릿 만들기

페이지를 생성할 때 새 페이지를 만드는 기준으로 사용할 템플릿을 선택해야 합니다. 템플릿은 결과 페이지, 초기 컨텐츠 및 허용된 구성 요소의 구조를 정의합니다.

[편집 가능한 템플릿](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html)의 세 가지 기본 영역이 있습니다.

1. **구조**  - 템플릿의 일부인 구성 요소를 정의합니다. 컨텐츠 작성자는 편집할 수 없습니다.
1. **초기 컨텐츠**  - 템플릿으로 시작할 구성 요소를 정의하고 컨텐츠 작성자가 편집 및/또는 삭제할 수 있습니다
1. **정책**  - 구성 요소의 동작 방식과 작성자가 사용할 수 있는 옵션에 대한 구성을 정의합니다.

다음으로, 모집단 구조와 일치하는 새 템플릿을 AEM에서 생성합니다. 이 문제는 AEM의 로컬 인스턴스에서 발생합니다. 아래 비디오에서 절차를 따르십시오.

>[!VIDEO](https://video.tv.adobe.com/v/330991/?quality=12&learn=on)

아래 비디오에 대한 높은 수준의 단계:

### 구조 구성

1. **페이지 템플릿 유형**(**문서 페이지**)을 사용하여 새 템플릿을 만듭니다.
1. **구조** 모드로 전환합니다.
1. 템플릿 맨 위에서 **헤더** 역할을 할 **경험 조각** 구성 요소를 추가합니다.
   * `/content/experience-fragments/wknd/us/en/site/header/master`을 가리키도록 구성 요소를 구성합니다.
   * 정책을 **페이지 헤더**&#x200B;로 설정하고 **기본 요소**&#x200B;가 `header`로 설정되어 있는지 확인합니다. `header`요소는 다음 장의 CSS를 사용하여 타깃팅됩니다.
1. 템플릿 하단에서 **바닥글** 역할을 할 **경험 조각** 구성 요소를 추가합니다.
   * `/content/experience-fragments/wknd/us/en/site/footer/master`을 가리키도록 구성 요소를 구성합니다.
   * 정책을 **페이지 바닥글**&#x200B;로 설정하고 **기본 요소**&#x200B;가 `footer`로 설정되어 있는지 확인합니다. `footer` 요소는 다음 장에서 CSS로 타깃팅됩니다.
1. 템플릿을 처음 만들 때 포함된 **main** 컨테이너를 잠급니다.
   * 정책을 **페이지 주**&#x200B;로 설정하고 **기본 요소**&#x200B;가 `main`로 설정되어 있는지 확인하십시오. `main` 요소는 다음 장에서 CSS로 타깃팅됩니다.
1. **Image** 구성 요소를 **main** 컨테이너에 추가합니다.
   * **Image** 구성 요소의 잠금을 해제합니다.
1. 기본 컨테이너의 **Image** 구성 요소 아래에 **탐색 표시** 구성 요소를 추가합니다.
   * **탐색 표시** 구성 요소에 대해 **문서 페이지 - 탐색 표시**&#x200B;에 대한 새 정책을 만듭니다. **탐색 시작 수준**&#x200B;을 **4**&#x200B;로 설정합니다.
1. **탐색 표시** 구성 요소 아래에 및 **주** 컨테이너 내에 **컨테이너** 구성 요소를 추가합니다. 이 작업은 템플릿에 대한 **컨텐츠 컨테이너** 역할을 합니다.
   * **Content** 컨테이너의 잠금을 해제합니다.
   * 정책을 **페이지 컨텐츠**&#x200B;로 설정합니다.
1. **컨텐츠 컨테이너** 아래에 다른 **컨테이너** 구성 요소를 추가합니다. 이는 템플릿의 **사이드 레일** 컨테이너 역할을 합니다.
   * **사이드 레일** 컨테이너의 잠금을 해제합니다.
   * **문서 페이지 - 사이드 레일**&#x200B;이라는 새 정책을 만듭니다.
   * **WKND Sites Project - Content**&#x200B;에서 **허용되는 구성 요소**&#x200B;를 구성하여 다음을 포함하도록 하십시오. **Button**, **다운로드**, **이미지**, **목록**, **구분 문자**, **소셜 미디어 공유**, **텍스트** 및 **제목**.
1. 페이지 루트 컨테이너의 정책을 업데이트합니다. 템플릿에서 가장 바깥쪽 컨테이너입니다. 정책을 **페이지 루트**&#x200B;로 설정합니다.
   * **컨테이너 설정**&#x200B;에서 **레이아웃**&#x200B;을 **응답형 그리드**&#x200B;로 설정합니다.
1. **컨텐츠 컨테이너**&#x200B;에 대해 레이아웃 모드를 사용합니다. 핸들을 오른쪽에서 왼쪽으로 드래그하고 컨테이너를 8열로 줄입니다.
1. **사이드 레일 컨테이너**&#x200B;에 대해 레이아웃 모드를 시작합니다. 핸들을 오른쪽에서 왼쪽으로 드래그하고 컨테이너를 4열로 줄입니다. 그런 다음 왼쪽 핸들을 왼쪽에서 오른쪽 1열로 드래그하여 컨테이너 3개의 열을 너비로 만들고 **컨텐츠 컨테이너** 사이에 1개의 열 간격을 둡니다.
1. 모바일 에뮬레이터를 열고 모바일 중단점으로 전환합니다. 레이아웃 모드를 다시 시작하고 **컨텐츠 컨테이너** 및 **사이드 레일 컨테이너**&#x200B;를 페이지의 전체 너비로 만듭니다. 이렇게 하면 모바일 중단점에 컨테이너가 세로로 스택됩니다.
1. **컨텐츠 컨테이너**&#x200B;에서 **Text** 구성 요소의 정책을 업데이트합니다.
   * 정책을 **컨텐츠 텍스트**&#x200B;로 설정합니다.
   * **Plugins** > **단락 스타일**&#x200B;에서 **단락 스타일 사용**&#x200B;을 선택하고 **Quote 블록**&#x200B;이 활성화되어 있는지 확인합니다.

### 초기 컨텐츠 구성

1. **초기 컨텐츠** 모드로 전환합니다.
1. **Title** 구성 요소를 **컨텐츠 컨테이너**&#x200B;에 추가합니다. 이것은 문서 제목으로서 작용합니다. 비워 두면 현재 페이지의 제목이 자동으로 표시됩니다.
1. 첫 번째 제목 구성 요소 아래에 두 번째 **Title** 구성 요소를 추가합니다.
   * 텍스트를 사용하여 구성 요소를 구성합니다. 작성자 텍스트 자리 표시자가 됩니다.
   * 유형을 `H4`으로 설정합니다.
1. **작성자** 제목 구성 요소 아래에 **텍스트** 구성 요소를 추가합니다.
1. **제목** 구성 요소를 **사이드 레일 컨테이너**&#x200B;에 추가합니다.
   * 텍스트를 사용하여 구성 요소를 구성합니다. &quot;이 스토리 공유&quot;
   * 유형을 `H5`으로 설정합니다.
1. **이 스토리** 제목 구성 요소 공유 아래에 **소셜 미디어 공유** 구성 요소를 추가합니다.
1. **소셜 미디어 공유** 구성 요소 아래에 **구분 기호** 구성 요소를 추가합니다.
1. **구분 기호** 구성 요소 아래에 **다운로드** 구성 요소를 추가합니다.
1. **다운로드** 구성 요소 아래에 **목록** 구성 요소를 추가합니다.
1. 템플릿에 대한 **초기 페이지 속성**&#x200B;을 업데이트합니다.
   * **소셜 미디어** > **소셜 미디어 공유**&#x200B;에서 **Facebook** 및 **Pinterest**&#x200B;을 확인하십시오

### 템플릿을 활성화하고 축소판 추가

1. [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)로 이동하여 템플릿 콘솔에서 템플릿을 봅니다
1. **** 문서 페이지 템플릿을 활성화합니다.
1. 문서 페이지 템플릿의 속성을 편집하고 다음 축소판을 업로드하여 문서 페이지 템플릿을 사용하여 만든 페이지를 빠르게 식별합니다.

   ![문서 페이지 템플릿 축소판](assets/pages-templates/article-page-template-thumbnail.png)

## 경험 조각으로 머리글 및 바닥글 업데이트 {#experience-fragments}

머리글 또는 바닥글과 같은 글로벌 컨텐츠를 만들 때 일반적으로 사용되는 방법은 [경험 조각](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html)을 사용하는 것입니다. 경험 조각 을 사용하면 여러 구성 요소를 결합하여 하나의 참조 가능한 구성 요소를 만들 수 있습니다. 경험 조각은 다중 사이트 관리 및 [로컬라이제이션](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure)을 지원하는 이점이 있습니다.

AEM 프로젝트 원형 이 머리글 및 바닥글을 생성했습니다. 그런 다음 경험 조각을 업데이트하여 비웃는 내용과 일치시킵니다. 아래 비디오에서 절차를 따르십시오.

>[!VIDEO](https://video.tv.adobe.com/v/330992/?quality=12&learn=on)

아래 비디오에 대한 높은 수준의 단계:

1. 샘플 컨텐츠 패키지 **[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets.zip)**&#x200B;을 다운로드합니다.
1. 패키지 관리자를 사용하여 [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)에 컨텐츠 패키지를 업로드하고 설치합니다
1. [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html)에서 경험 조각에 사용되는 템플릿인 웹 변형 템플릿을 업데이트합니다
   * 템플릿의 **Container** 구성 요소에서 정책을 업데이트합니다.
   * 정책을 **XF Root**&#x200B;로 설정합니다.
   * **허용된 구성 요소**&#x200B;에서 **언어 탐색**, **탐색** 및 **빠른 검색** 구성 요소를 포함하도록 구성 요소 그룹 **WKND 사이트 프로젝트 - 구조**&#x200B;를 선택합니다.

### 헤더 경험 조각 업데이트

1. [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html)에서 헤더를 렌더링하는 경험 조각을 엽니다.
1. 조각의 루트 **Container**&#x200B;를 구성합니다. 가장 바깥쪽 **컨테이너**&#x200B;입니다.
   * **레이아웃**&#x200B;을 **응답형 그리드**&#x200B;로 설정합니다.
1. **WKND Dark 로고**&#x200B;를 **Container**&#x200B;의 맨 위에 이미지로 추가합니다. 이전 단계에 설치된 패키지에 로고가 포함되어 있었습니다.
   * **WKND 다크 로고**&#x200B;의 레이아웃을 **2** 열 너비로 수정합니다. 핸들을 오른쪽에서 왼쪽으로 드래그합니다.
   * &quot;WKND 로고&quot;의 **대체 텍스트**&#x200B;로 로고를 구성합니다.
   * 홈 페이지의 **링크**&#x200B;에 로고를 구성하십시오.`/content/wknd/us/en`
1. 페이지에 이미 배치된 **탐색** 구성 요소를 구성합니다.
   * **루트 수준 제외**&#x200B;를 **1**&#x200B;로 설정합니다.
   * **탐색 구조 깊이**&#x200B;를 **1**&#x200B;로 설정합니다.
   * **탐색** 구성 요소의 레이아웃을 **8** 열 너비로 수정합니다. 핸들을 오른쪽에서 왼쪽으로 드래그합니다.
1. **언어 탐색** 구성 요소를 제거합니다.
1. **검색** 구성 요소의 레이아웃을 **2** 열 너비로 수정합니다. 핸들을 오른쪽에서 왼쪽으로 드래그합니다. 이제 모든 구성 요소를 단일 행에 수평 정렬해야 합니다.

### 바닥글 경험 조각 업데이트

1. [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)에서 바닥글을 렌더링하는 경험 조각을 엽니다.
1. 조각의 루트 **Container**&#x200B;를 구성합니다. 가장 바깥쪽 **컨테이너**&#x200B;입니다.
   * **레이아웃**&#x200B;을 **응답형 그리드**&#x200B;로 설정합니다.
1. **WKND Light 로고**&#x200B;를 **Container**&#x200B;의 맨 위에 이미지로 추가합니다. 이전 단계에 설치된 패키지에 로고가 포함되어 있었습니다.
   * **WKND Light 로고**&#x200B;의 레이아웃을 **2** 열 너비로 수정합니다. 핸들을 오른쪽에서 왼쪽으로 드래그합니다.
   * &quot;WKND 로고 표시등&quot;의 **대체 텍스트**&#x200B;로 로고를 구성합니다.
   * 홈 페이지의 **링크**&#x200B;에 로고를 구성하십시오.`/content/wknd/us/en`
1. 로고 아래에 **탐색** 구성 요소를 추가합니다. **탐색** 구성 요소를 구성합니다.
   * **루트 수준 제외**&#x200B;를 **1**&#x200B;로 설정합니다.
   * **모든 하위 페이지 수집**&#x200B;의 선택을 취소합니다.
   * **탐색 구조 깊이**&#x200B;를 **1**&#x200B;로 설정합니다.
   * **탐색** 구성 요소의 레이아웃을 **8** 열 너비로 수정합니다. 핸들을 오른쪽에서 왼쪽으로 드래그합니다.

## 문서 페이지 만들기

그런 다음 문서 페이지 템플릿을 사용하여 새 페이지를 만듭니다. 사이트의 컨텐츠를 작성하여 사이트 모집단 과 일치시킵니다. 아래 비디오에서 절차를 따르십시오.

>[!VIDEO](https://video.tv.adobe.com/v/330993/?quality=12&learn=on)

아래 비디오에 대한 높은 수준의 단계:

1. [http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine)의 사이트 콘솔로 이동합니다.
1. **WKND** > **US** > **EN** > **Magazine** 아래에 새 페이지를 만듭니다.
   * **문서 페이지** 템플릿을 선택합니다.
   * **속성** 아래에서 **제목**&#x200B;을 &quot;Ultimate Guide to LA Skatestand&quot; 로 설정합니다.
   * **이름**&#x200B;을 &quot;guide-la-skatestparks&quot;로 설정합니다.
1. **By Author** Title을 &quot;By Stacey Roswells&quot; 텍스트로 바꿉니다.
1. 단락을 포함하도록 **Text** 구성 요소를 업데이트하여 문서를 채웁니다. 다음 텍스트 파일을 복사본으로 사용할 수 있습니다. [la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt)
1. 다른 **Text** 구성 요소를 추가합니다.
   * 견적을 포함하도록 구성 요소를 업데이트합니다. &quot;LA를 떠날 곳이 없다.&quot;
   * 전체 화면 모드로 리치 텍스트 편집기를 편집하고 위의 따옴표를 수정하여 **견적 블록** 요소를 사용합니다.
1. 모집단 유사하게 문서의 본문을 계속 채우십시오.
1. 문서의 PDF 버전을 사용하도록 **다운로드** 구성 요소를 구성합니다.
   * **다운로드** > **속성**&#x200B;에서 확인란을 클릭하여 **DAM 자산에서 제목을 가져옵니다**.
   * **설명**&#x200B;을 다음과 같이 설정합니다. &quot;Get the Full Story&quot;.
   * **작업 텍스트**&#x200B;를 다음과 같이 설정합니다. &quot;PDF 다운로드&quot;.
1. **List** 구성 요소를 구성합니다.
   * **목록 설정** > **목록 작성**&#x200B;에서 **하위 페이지**&#x200B;를 선택합니다.
   * **상위 페이지**&#x200B;를 `/content/wknd/us/en/magazine`로 설정합니다.
   * **Item Settings**&#x200B;에서 **Link Items**&#x200B;를 선택하고 **Show date**&#x200B;를 선택합니다.

## Inspect 노드 구조 {#node-structure}

이 시점에서 기사 페이지는 분명히 스타일이 지정되지 않았습니다. 그러나 기본적인 구조가 제자리에 있습니다. 그런 다음 문서 페이지의 노드 구조를 검사하여 템플릿, 페이지 및 구성 요소의 역할을 더 잘 이해할 수 있습니다.

로컬 AEM 인스턴스에서 CRXDE-Lite 도구를 사용하여 기본 노드 구조를 확인합니다.

1. [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent)를 열고 트리 탐색 기능을 사용하여 `/content/wknd/us/en/magazine/guide-la-skateparks`로 이동합니다.

1. `la-skateparks` 페이지 아래의 `jcr:content` 노드를 클릭하고 속성을 확인합니다.

   ![JCR 컨텐츠 속성](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   `/conf/wknd/settings/wcm/templates/article-page`을 가리키는 `cq:template`에 대한 값을 보십시오. 이 값은 이전에 만든 문서 페이지 템플릿입니다.

   `wknd/components/page`을 가리키는 `sling:resourceType` 값도 확인합니다. AEM 프로젝트 원형에 의해 만들어진 페이지 구성 요소이며 템플릿을 기반으로 페이지 렌더링을 수행합니다.

1. `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` 아래의 `jcr:content` 노드를 확장하고 노드 계층을 확인합니다.

   ![JCR 콘텐츠 LA 스케이트보드장](assets/pages-templates/page-jcr-structure.png)

   각 노드를 작성된 구성 요소에 느슨하게 매핑할 수 있어야 합니다. `container` 접두사가 있는 노드를 검사하여 사용되는 다양한 레이아웃 컨테이너를 식별할 수 있는지 확인합니다.

1. 다음으로 `/apps/wknd/components/page`에서 페이지 구성 요소를 검사합니다. CRXDE Lite에서 구성 요소 속성 보기:

   ![페이지 구성 요소 속성](assets/pages-templates/page-component-properties.png)

   페이지 구성 요소 아래에는 HTL 스크립트 `customfooterlibs.html` 및 `customheaderlibs.html`만 2개 있습니다. *그러면 이 구성 요소는 페이지를 어떻게 렌더링합니까?*

   `sling:resourceSuperType` 속성은 `core/wcm/components/page/v2/page`을 가리킵니다. 이 속성을 사용하면 WKND의 페이지 구성 요소가 코어 구성 요소 페이지 구성 요소의 기능 **모든**&#x200B;을 상속할 수 있습니다. 이것은 [프록시 구성 요소 패턴](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern)이라는 이름의 첫 번째 예입니다. 자세한 내용은 [여기에서 찾을 수 있습니다.](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html)

1. Inspect 구성 요소 내의 다른 구성 요소인 `Breadcrumb` 구성 요소는 다음과 같습니다. `/apps/wknd/components/breadcrumb`. 동일한 `sling:resourceSuperType` 속성을 찾을 수 있지만 이번에는 `core/wcm/components/breadcrumb/v2/breadcrumb`을 가리킵니다. 프록시 구성 요소 패턴을 사용하여 코어 구성 요소를 포함하는 다른 예입니다. 실제로 WKND 코드 베이스의 모든 구성 요소는 AEM 코어 구성 요소의 프록시입니다(유명한 HelloWorld 구성 요소 제외). 사용자 지정 코드를 쓰기 전에 *핵심 구성 요소의 기능을 최대한 다시 사용하는 것이 좋습니다.*

1. 다음으로 CRXDE Lite을 사용하여 `/libs/core/wcm/components/page/v2/page`에 있는 코어 구성 요소 페이지를 검사합니다.

   >[!NOTE]
   >
   > AEM 6.5/6.4에서 코어 구성 요소는 `/apps/core/wcm/components` 아래에 있습니다. AEM as a Cloud Service에서 코어 구성 요소는 `/libs` 아래에 있으며 자동으로 업데이트됩니다.

   ![핵심 구성 요소 페이지](assets/pages-templates/core-page-component-properties.png)

   이 페이지 아래에는 더 많은 스크립트가 포함되어 있습니다. 핵심 구성 요소 페이지에는 많은 기능이 포함되어 있습니다. 이 기능은 유지 보수 및 가독성을 향상시키기 위해 여러 스크립트로 분할됩니다. `page.html`을 열고 `data-sly-include`을 찾아 HTL 스크립트 포함을 추적할 수 있습니다.

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

   HTL을 여러 스크립트로 분할하는 또 다른 이유는 프록시 구성 요소가 개별 스크립트를 재정의하여 사용자 지정 비즈니스 논리를 구현할 수 있도록 하기 위한 것입니다. HTL 스크립트 `customfooterlibs.html` 및 `customheaderlibs.html` 는 프로젝트를 구현하여 재정의할 명시적 목적으로 생성됩니다.

   편집 가능한 템플릿 요소가 [컨텐츠 페이지의 렌더링에 어떻게 영향을 주는지에 대해 이 문서](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html)를 읽어서 자세히 알 수 있습니다.

1. 다른 코어 구성 요소(예: `/libs/core/wcm/components/breadcrumb/v2/breadcrumb`의 탐색 표시)입니다. `breadcrumb.html` 스크립트를 보고 이동 경로 구성 요소에 대한 마크업이 최종적으로 어떻게 생성되는지 알아보십시오.

## 소스 컨트롤에 구성 저장 {#configuration-persistence}

대부분의 경우, 특히 AEM 프로젝트 시작 시 템플릿 및 관련 컨텐츠 정책과 같은 구성을 소스 제어에 유지하는 것이 중요합니다. 이렇게 하면 모든 개발자가 동일한 컨텐츠 및 구성 세트에 대해 작업하고 있으므로 환경 간에 추가적인 일관성을 유지할 수 있습니다. 프로젝트가 일정 수준의 성숙기에 도달하면 템플릿 관리 방법을 특수 사용자 그룹으로 전환할 수 있습니다.

이제 템플릿을 다른 코드 조각처럼 처리하고 **문서 페이지 템플릿** 다운을 프로젝트의 일부로 동기화합니다. 지금까지 AEM 프로젝트에서 AEM의 로컬 인스턴스로 **푸시된** 코드가 있습니다. **문서 페이지 템플릿**&#x200B;은(는) AEM의 로컬 인스턴스에서 직접 생성되었으므로 **템플릿을 AEM 프로젝트로 가져오기**&#x200B;해야 합니다. **ui.content** 모듈은 이러한 특정 목적을 위해 AEM 프로젝트에 포함되어 있습니다.

다음 몇 단계는 [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&amp;ssr=false#overview) 플러그인을 사용하여 VSCode IDE를 사용하여 수행되지만, **import**&#x200B;로 구성하거나 AEM의 로컬 인스턴스에서 콘텐츠를 가져오도록 구성한 IDE를 사용하여 수행할 수 있습니다.

1. VSCode에서 `aem-guides-wknd` 프로젝트를 엽니다.

1. 프로젝트 탐색기에서 **ui.content** 모듈을 확장합니다. `src` 폴더를 확장하고 `/conf/wknd/settings/wcm/templates` 로 이동합니다.

1. [!UICONTROL 마우스 오른쪽 ] 단추+폴더를  `templates` 클릭하고  **AEM Server에서 가져오기**&#x200B;를 선택합니다.

   ![VSCode 가져오기 템플릿](assets/pages-templates/vscode-import-templates.png)

   `article-page`을(를) 가져오고 `page-content`, `xf-web-variation` 템플릿도 업데이트해야 합니다.

   ![업데이트된 템플릿](assets/pages-templates/updated-templates.png)

1. 단계를 반복하여 컨텐츠를 가져오지만 `/conf/wknd/settings/wcm/policies`에 있는 **정책** 폴더를 선택합니다.

   ![VSCode 가져오기 정책](assets/pages-templates/policies-article-page-template.png)

1. Inspect은 `ui.content/src/main/content/META-INF/vault/filter.xml`에 있는 `filter.xml` 파일입니다.

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

   `filter.xml` 파일은 패키지와 함께 설치할 노드의 경로를 식별해야 합니다. 기존 컨텐츠가 수정되지 않고 새 컨텐츠만 추가된다는 것을 나타내는 각 필터에 대해 `mode="merge"`이 표시됩니다. 컨텐츠 작성자는 이러한 경로를 업데이트할 수 있으므로 코드 배포에서 **이 컨텐츠를 덮어쓰지 않는 것이 중요합니다.** 필터 요소 작업에 대한 자세한 내용은 [FileVault 설명서](https://jackrabbit.apache.org/filevault/filter.html)를 참조하십시오.

   `ui.content/src/main/content/META-INF/vault/filter.xml` 및 `ui.apps/src/main/content/META-INF/vault/filter.xml` 를 비교하여 각 모듈에서 관리하는 서로 다른 노드를 파악합니다.

   >[!WARNING]
   >
   > WKND 참조 사이트에 대한 일관된 배포를 위해 `ui.content`이 JCR의 모든 변경 사항을 덮어쓰도록 프로젝트의 일부 분기가 설정됩니다. 코드/스타일이 특정 정책에 대해 작성되므로 솔루션 분기의 디자인에서 결정됩니다.

## 축하합니다! {#congratulations}

축하합니다. Adobe Experience Manager Sites을 사용하여 새 템플릿과 페이지를 만들었습니다.

### 다음 단계 {#next-steps}

이 시점에서 기사 페이지는 분명히 스타일이 지정되지 않았습니다. CSS 및 Javascript를 포함하여 전역 스타일을 사이트에 적용하고 전용 프런트 엔드 빌드를 통합하는 모범 사례를 알아보려면 [클라이언트 측 라이브러리 및 프런트 엔드 워크플로우](client-side-libraries.md) 자습서를 따르십시오.

완료된 코드를 [GitHub](https://github.com/adobe/aem-guides-wknd)에서 확인하거나 Git brach `tutorial/pages-templates-solution`에서 로컬로 코드를 검토하고 배포합니다.

1. [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 리포지토리를 복제합니다.
1. `tutorial/pages-templates-solution` 분기를 확인하십시오.
