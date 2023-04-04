---
title: AEM Sites 시작하기 - 페이지 및 템플릿
description: 기본 페이지 구성 요소와 편집 가능한 템플릿 간의 관계에 대해 알아봅니다. 핵심 구성 요소가 프로젝트에 프록시되는 방법을 이해합니다. Adobe XD의 샘플을 기반으로 잘 구성된 문서 페이지 템플릿을 작성하기 위한 편집 가능한 템플릿의 고급 정책 구성을 알아봅니다.
feature: Core Components, Editable Templates, Page Editor
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 4082
thumbnail: 30214.jpg
exl-id: e9d06dc2-ac3b-48c5-ae00-fdaf5bb45b54
recommendations: noDisplay, noCatalog
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '3040'
ht-degree: 1%

---

# 페이지 및 템플릿 {#pages-and-template}

이 장에서 기본 페이지 구성 요소와 편집 가능한 템플릿 간의 관계를 살펴보겠습니다. 의 몇 가지 모집단 설명을 기반으로 스타일이 지정되지 않은 문서 템플릿을 만드는 방법을 알아봅니다. [Adobe XD](https://helpx.adobe.com/support/xd.html). 템플릿을 작성하는 과정에서 편집 가능한 템플릿의 핵심 구성 요소 및 고급 정책 구성이 다룹니다.

## 사전 요구 사항 {#prerequisites}

설정에 필요한 도구 및 지침을 검토합니다. [로컬 개발 환경](overview.md#local-dev-environment).

### 스타터 프로젝트

>[!NOTE]
>
> 이전 장을 성공적으로 완료한 경우 프로젝트를 다시 사용하고 시작 프로젝트를 체크 아웃하는 단계를 건너뛸 수 있습니다.

자습서가 빌드하는 기본 라인 코드를 확인합니다.

1. 다음을 확인하십시오 `tutorial/pages-templates-start` 분기 [GitHub](https://github.com/adobe/aem-guides-wknd)

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
   > AEM 6.5 또는 6.4를 사용하는 경우 `classic` 프로파일을 Maven 명령에 추가합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

항상 완료된 코드를 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/pages-templates-solution) 또는 분기로 전환하여 로컬로 코드를 체크 아웃합니다 `tutorial/pages-templates-solution`.

## 목표

1. Inspect Adobe XD에서 만든 페이지 디자인을 만들어 핵심 구성 요소에 매핑합니다.
1. 편집 가능한 템플릿의 세부 사항과 정책을 사용하여 페이지 컨텐츠를 세부적으로 제어하는 방법을 알아봅니다.
1. 템플릿 및 페이지 연결 방법 알아보기

## 빌드할 내용 {#what-build}

자습서의 이 부분에서 문서 페이지를 만들고 공통 구조에 맞게 조정하는 데 사용할 수 있는 새 문서 페이지 템플릿을 만듭니다. 문서 페이지 템플릿은 Adobe XD에서 만들어진 UI 키트 및 디자인을 기반으로 합니다. 이 장은 템플릿의 구조나 골격을 빌드하는 데만 중점을 둡니다. 스타일이 구현되지 않지만 템플릿 및 페이지가 작동합니다.

![문서 페이지 디자인 및 스타일이 지정되지 않은 버전](assets/pages-templates/what-you-will-build.png)

## Adobe XD을 사용한 UI 계획 {#adobexd}

일반적으로, 새 웹 사이트를 계획하는 것은 모집단 및 정적 디자인부터 시작됩니다. [Adobe XD](https://helpx.adobe.com/support/xd.html) 사용자 경험을 구축하는 디자인 도구입니다. 다음으로 문서 페이지 템플릿의 구조를 계획하는 데 도움이 되도록 UI 키트 및 샘플을 살펴보겠습니다.

>[!VIDEO](https://video.tv.adobe.com/v/30214?quality=12&learn=on)

**다운로드 [WKND 문서 디자인 파일](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**.

>[!NOTE]
>
> 일반 [AEM 코어 구성 요소 UI 키트도 사용할 수 있습니다](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd) 을 사용자 지정 프로젝트의 시작점으로 사용할 수 있습니다.

## 문서 페이지 템플릿 만들기

페이지를 생성할 때 페이지 작성 기준으로 사용되는 템플릿을 선택해야 합니다. 템플릿은 결과 페이지, 초기 컨텐츠 및 허용된 구성 요소의 구조를 정의합니다.

의 주요 영역은 세 가지입니다 [편집 가능한 템플릿](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html):

1. **구조** - 템플릿의 일부인 구성 요소를 정의합니다. 이러한 템플릿은 컨텐츠 작성자가 편집할 수 없습니다.
1. **초기 컨텐츠** - 템플릿으로 시작하는 구성 요소를 정의하며 컨텐츠 작성자가 편집 및/또는 삭제할 수 있습니다
1. **정책** - 구성 요소의 동작 방식과 작성자가 가지는 옵션에 대한 구성을 정의합니다.

다음으로, 모집단 구조와 일치하는 템플릿을 AEM에서 생성합니다. 이 이벤트는 AEM의 로컬 인스턴스에서 발생합니다. 아래 비디오에서 절차를 따르십시오.

>[!VIDEO](https://video.tv.adobe.com/v/330991?quality=12&learn=on)

위의 비디오에 대한 높은 수준의 단계:

### 구조 구성

1. 을 사용하여 템플릿 만들기 **페이지 템플릿 유형**, 이름이 지정됨 **문서 페이지**.
1. 로 전환 **구조** 모드.
1. 추가 **경험 조각** 역할을 수행할 구성 요소 **Header** 를 클릭합니다.
   * 을 가리키도록 구성 요소 구성 `/content/experience-fragments/wknd/us/en/site/header/master`.
   * 정책을 (으)로 설정합니다. **페이지 머리글** 그리고 **기본 요소** 가 로 설정되어 있습니다. `header`. 다음 `header`요소는 다음 장에서 CSS를 사용하여 타깃팅됩니다.
1. 추가 **경험 조각** 역할을 수행할 구성 요소 **바닥글** at the bottom of the template.
   * 을 가리키도록 구성 요소 구성 `/content/experience-fragments/wknd/us/en/site/footer/master`.
   * 정책을 (으)로 설정합니다. **페이지 바닥글** 그리고 **기본 요소** 가 로 설정되어 있습니다. `footer`. 다음 `footer` 요소는 다음 장에서 CSS를 사용하여 타깃팅됩니다.
1. 잠금 **main** 템플릿을 처음 만들 때 포함된 컨테이너입니다.
   * 정책을 (으)로 설정합니다. **페이지 기본** 그리고 **기본 요소** 가 로 설정되어 있습니다. `main`. 다음 `main` 요소는 다음 장에서 CSS를 사용하여 타깃팅됩니다.
1. 추가 **이미지** 구성 요소를 **main** 컨테이너.
   * 잠금 해제 **이미지** 구성 요소.
1. 추가 **탐색 표시** 아래의 구성 요소 **이미지** 구성 요소를 포함할 수 없습니다.
   * 에 대한 정책 만들기 **탐색 표시** 이름이 지정된 구성 요소 **문서 페이지 - 탐색 표시**. 설정 **탐색 시작 수준** to **4**.
1. 추가 **컨테이너** 아래의 구성 요소 **탐색 표시** 구성 요소 및 내부 **main** 컨테이너. 이 기능은 **컨텐츠 컨테이너** 참조하십시오.
   * 잠금 해제 **컨텐츠** 컨테이너.
   * 정책을 (으)로 설정합니다. **페이지 컨텐츠**.
1. 다른 추가 **컨테이너** 아래의 구성 요소 **컨텐츠 컨테이너**. 이 기능은 **사이드 레일** 템플릿의 컨테이너입니다.
   * 잠금 해제 **사이드 레일** 컨테이너.
   * 이름이 지정된 정책 만들기 **문서 페이지 - 사이드 레일**.
   * 구성 **허용된 구성 요소** 아래에 **WKND Sites 프로젝트 - 컨텐츠** 다음을 포함합니다. **단추**, **다운로드**, **이미지**, **목록**, **구분 기호**, **소셜 미디어 공유**, **텍스트**, 및 **제목**.
1. 페이지 루트 컨테이너의 정책을 업데이트합니다. 템플릿에서 가장 바깥쪽 컨테이너입니다. 정책을 (으)로 설정합니다. **페이지 루트**.
   * 아래 **컨테이너 설정**, 설정 **레이아웃** to **응답형 그리드**.
1. 에 대한 레이아웃 모드 참여 **컨텐츠 컨테이너**. 핸들을 오른쪽에서 왼쪽으로 드래그하고 컨테이너를 8열 너비로 줄입니다.
1. 에 대한 레이아웃 모드 참여 **사이드 레일 컨테이너**. 핸들을 오른쪽에서 왼쪽으로 드래그하고 컨테이너를 너비가 4열로 줄입니다. 그런 다음 왼쪽 핸들을 왼쪽에서 오른쪽으로 한 열로 드래그하여 컨테이너 3열의 너비를 만들고 컨테이너 사이에 1열 간격을 둡니다 **컨텐츠 컨테이너**.
1. 모바일 에뮬레이터를 열고 모바일 중단점으로 전환합니다. 레이아웃 모드를 다시 시작하고 **컨텐츠 컨테이너** 그리고 **사이드 레일 컨테이너** 페이지의 전체 너비. 이렇게 하면 모바일 중단점에 컨테이너가 세로로 스택됩니다.
1. 의 정책 업데이트 **텍스트** 의 구성 요소 **컨텐츠 컨테이너**.
   * 정책을 (으)로 설정합니다. **컨텐츠 텍스트**.
   * 아래 **Plugins** > **단락 스타일**, check **단락 스타일 활성화** 그리고 **견적 블록** 이 활성화되어 있습니다.

### 초기 컨텐츠 구성

1. 다음으로 전환 **초기 컨텐츠** 모드.
1. 추가 **제목** 구성 요소를 **컨텐츠 컨테이너**. 문서 제목 역할을 합니다. 비워 두면 현재 페이지의 제목이 자동으로 표시됩니다.
1. 초 추가 **제목** 첫 번째 제목 구성 요소 아래에 있는 구성 요소입니다.
   * 텍스트를 사용하여 구성 요소를 구성합니다. 작성자 텍스트 자리 표시자입니다.
   * 유형을 (으)로 설정합니다. `H4`.
1. 추가 **텍스트** 아래의 구성 요소 **작성자** 제목 구성 요소입니다.
1. 추가 **제목** 구성 요소를 **사이드 레일 컨테이너**.
   * 텍스트를 사용하여 구성 요소를 구성합니다. &quot;이 스토리 공유&quot;
   * 유형을 (으)로 설정합니다. `H5`.
1. 추가 **소셜 미디어 공유** 아래의 구성 요소 **이 스토리 공유** 제목 구성 요소입니다.
1. 추가 **구분 기호** 아래의 구성 요소 **소셜 미디어 공유** 구성 요소.
1. 추가 **다운로드** 아래의 구성 요소 **구분 기호** 구성 요소.
1. 추가 **목록** 아래의 구성 요소 **다운로드** 구성 요소.
1. 업데이트 **초기 페이지 속성** 참조하십시오.
   * 아래 **소셜 미디어** > **소셜 미디어 공유**, check **Facebook** 및 **Pinterest**

### 템플릿을 활성화하고 축소판 추가

1. 으로 이동하여 템플릿 콘솔에서 템플릿 보기 [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)
1. **활성화** 문서 페이지 템플릿.
1. 문서 페이지 템플릿의 속성을 편집하고 다음 축소판을 업로드하여 문서 페이지 템플릿을 사용하여 만든 페이지를 빠르게 식별합니다.

   ![문서 페이지 템플릿 축소판](assets/pages-templates/article-page-template-thumbnail.png)

## 경험 조각으로 머리글 및 바닥글 업데이트 {#experience-fragments}

머리글이나 바닥글과 같은 글로벌 컨텐츠를 만들 때 일반적으로 사용할 때는 [경험 조각](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). 경험 조각 을 사용하면 여러 구성 요소를 결합하여 하나의 참조 가능한 구성 요소를 만들 수 있습니다. 경험 조각은 다중 사이트 관리를 지원하고 [로컬라이제이션](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/experience-fragment.html?lang=en).

AEM 프로젝트 원형 이 머리글 및 바닥글을 생성했습니다. 그런 다음 경험 조각을 업데이트하여 비웃는 내용과 일치시킵니다. 아래 비디오에서 절차를 따르십시오.

>[!VIDEO](https://video.tv.adobe.com/v/330992?quality=12&learn=on)

위의 비디오에 대한 높은 수준의 단계:

1. 샘플 컨텐츠 패키지 다운로드 **[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets-1.1.zip)**.
1. 에서 패키지 관리자를 사용하여 컨텐츠 패키지를 업로드하고 설치합니다. [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. 경험 조각에 사용되는 템플릿인 웹 변형 템플릿을 업데이트합니다. [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html)
   * 정책 업데이트 **컨테이너** 구성 요소를 생성하지 않습니다.
   * 정책을 (으)로 설정합니다. **XF 루트**.
   * 아래에 있는 **허용된 구성 요소** 구성 요소 그룹 선택 **WKND Sites 프로젝트 - 구조** 다음을 포함합니다. **언어 탐색**, **탐색**, 및 **빠른 검색** 구성 요소.

### 헤더 경험 조각 업데이트

1. 에서 헤더를 렌더링하는 경험 조각을 엽니다. [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html)
1. 루트 구성 **컨테이너** 조각에 대해 설명합니다. 이게 가장 외주예요 **컨테이너**.
   * 설정 **레이아웃** to **응답형 그리드**
1. 추가 **WKND Dark 로고** 을 맨 위에 있는 이미지로 **컨테이너**. 이전 단계에 설치된 패키지에 로고가 포함되어 있었습니다.
   * 레이아웃 수정 **WKND Dark 로고** 대상 **2개** 열 너비입니다. 핸들을 오른쪽에서 왼쪽으로 드래그합니다.
   * 로 로고 구성 **대체 텍스트** WKND 로고.
   * 로고를 다음으로 구성 **링크** to `/content/wknd/us/en` 홈 페이지.
1. 구성 **탐색** 페이지에 이미 있는 구성 요소입니다.
   * 설정 **루트 수준 제외** to **1**.
   * 설정 **탐색 구조 깊이** to **1**.
   * 레이아웃 수정 **탐색** 구성 요소 **8개** 열 너비입니다. 핸들을 오른쪽에서 왼쪽으로 드래그합니다.
1. 제거 **언어 탐색** 구성 요소.
1. 레이아웃 수정 **검색** 구성 요소 **2개** 열 너비입니다. 핸들을 오른쪽에서 왼쪽으로 드래그합니다. 이제 모든 구성 요소를 단일 행에 수평 정렬해야 합니다.

### 바닥글 경험 조각 업데이트

1. 바닥글을 렌더링하는 경험 조각을 엽니다. [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)
1. 루트 구성 **컨테이너** 조각에 대해 설명합니다. 이게 가장 외주예요 **컨테이너**.
   * 설정 **레이아웃** to **응답형 그리드**
1. 추가 **WKND 조명 로고** 을 맨 위에 있는 이미지로 **컨테이너**. 이전 단계에 설치된 패키지에 로고가 포함되어 있었습니다.
   * 레이아웃 수정 **WKND 조명 로고** 대상 **2개** 열 너비입니다. 핸들을 오른쪽에서 왼쪽으로 드래그합니다.
   * 로 로고 구성 **대체 텍스트** WKND 로고 표시등
   * 로고를 다음으로 구성 **링크** to `/content/wknd/us/en` 홈 페이지.
1. 추가 **탐색** 구성 요소를 추가했습니다. 구성 **탐색** 구성 요소:
   * 설정 **루트 수준 제외** to **1**.
   * 선택을 취소합니다 **모든 하위 페이지 수집**.
   * 설정 **탐색 구조 깊이** to **1**.
   * 레이아웃 수정 **탐색** 구성 요소 **8개** 열 너비입니다. 핸들을 오른쪽에서 왼쪽으로 드래그합니다.

## 문서 페이지 만들기

그런 다음 문서 페이지 템플릿을 사용하여 페이지를 만듭니다. 사이트의 컨텐츠를 작성하여 사이트 모집단 과 일치시킵니다. 아래 비디오에서 절차를 따르십시오.

>[!VIDEO](https://video.tv.adobe.com/v/330993?quality=12&learn=on)

위의 비디오에 대한 높은 수준의 단계:

1. 의 사이트 콘솔로 이동합니다. [http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine).
1. 아래에 페이지를 만듭니다 **WKND** > **미국** > **EN** > **잡지**.
   * 을(를) 선택합니다 **문서 페이지** 템플릿.
   * 아래 **속성** 설정 **제목** &quot;Ultimate Guide to LA Skatestparks&quot;
   * 설정 **이름** &quot;guide-la-skatestparks&quot;
1. 바꾸기 **작성자** 제목: &quot;By Stacey Roswells&quot;
1. 업데이트 **텍스트** 단락을 포함하여 문서에 채울 구성 요소입니다. 다음 텍스트 파일을 복사본으로 사용할 수 있습니다. [la-sketplace-copy.txt](assets/pages-templates/la-skateparks-copy.txt).
1. 다른 추가 **텍스트** 구성 요소.
   * 견적을 포함하도록 구성 요소를 업데이트합니다. &quot;LA보다 더 좋은 장소는 없다.&quot;
   * 전체 화면 모드에서 리치 텍스트 편집기를 편집하고 위의 따옴표를 수정하여 **견적 블록** 요소를 생성하지 않습니다.
1. 모집단 유사하게 문서의 본문을 계속 채우십시오.
1. 구성 **다운로드** 구성 요소를 사용하여 문서의 PDF 버전을 사용합니다.
   * 아래 **다운로드** > **속성**&#x200B;을 클릭할 경우 **DAM 자산에서 제목을 가져옵니다.**.
   * 설정 **설명** 변환: &quot;Get the Full Story&quot;.
   * 설정 **작업 텍스트** 변환: &quot;PDF 다운로드&quot;.
1. 구성 **목록** 구성 요소.
   * 아래 **목록 설정** > **목록 작성 방법**, 선택 **하위 페이지**.
   * 설정 **상위 페이지** to `/content/wknd/us/en/magazine`.
   * 아래에 있는 **항목 설정** check **링크 항목** 확인 **날짜 표시**.

## Inspect 노드 구조 {#node-structure}

이 시점에서 기사 페이지는 분명히 스타일이 지정되지 않았습니다. 그러나 기본적인 구조가 제자리에 있습니다. 그런 다음 문서 페이지의 노드 구조를 검사하여 템플릿, 페이지 및 구성 요소의 역할을 더 잘 이해할 수 있습니다.

로컬 AEM 인스턴스에서 CRXDE-Lite 도구를 사용하여 기본 노드 구조를 확인합니다.

1. 열기 [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) 트리 탐색 기능을 사용하여 `/content/wknd/us/en/magazine/guide-la-skateparks`.

1. 을(를) 클릭합니다. `jcr:content` 노드 아래의 노드 `la-skateparks` 페이지를 열고 속성을 확인합니다.

   ![JCR 컨텐츠 속성](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   다음 기간 동안 `cq:template`: `/conf/wknd/settings/wcm/templates/article-page`: 앞에서 만든 문서 페이지 템플릿.

   또한 값 `sling:resourceType`: `wknd/components/page`. AEM 프로젝트 원형에 의해 만들어진 페이지 구성 요소이며 템플릿을 기반으로 페이지 렌더링을 수행합니다.

1. 를 확장합니다. `jcr:content` 노드 아래 `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` 그리고 노드 계층을 봅니다.

   ![JCR 콘텐츠 LA 스케이트보드장](assets/pages-templates/page-jcr-structure.png)

   각 노드를 작성된 구성 요소에 느슨하게 매핑할 수 있어야 합니다. 접두사가 있는 노드를 검사하여 사용되는 다양한 레이아웃 컨테이너를 식별할 수 있는지 확인합니다. `container`.

1. 다음 위치 페이지 구성 요소를 검사합니다. `/apps/wknd/components/page`. CRXDE Lite에서 구성 요소 속성 보기:

   ![페이지 구성 요소 속성](assets/pages-templates/page-component-properties.png)

   HTL 스크립트는 두 개뿐입니다. `customfooterlibs.html` 및 `customheaderlibs.html` 페이지 구성 요소 아래에 표시됩니다. *그러면 이 구성 요소는 페이지를 어떻게 렌더링합니까?*

   다음 `sling:resourceSuperType` 속성이 `core/wcm/components/page/v2/page`. 이 속성을 사용하면 WKND의 페이지 구성 요소를 상속할 수 있습니다 **모두** 핵심 구성 요소 페이지 구성 요소의 기능입니다. 이것이 바로 [프록시 구성 요소 패턴](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern). 자세한 내용 [여기](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html).

1. Inspect은 WKND 구성 요소 내에서 `Breadcrumb` 구성 요소: `/apps/wknd/components/breadcrumb`. 동일한 `sling:resourceSuperType` 속성을 찾을 수 있지만 이번에는 `core/wcm/components/breadcrumb/v2/breadcrumb`. 프록시 구성 요소 패턴을 사용하여 코어 구성 요소를 포함하는 다른 예입니다. 실제로 WKND 코드 베이스의 모든 구성 요소는 AEM 코어 구성 요소의 프록시입니다(사용자 지정 데모 HelloWorld 구성 요소 제외). 핵심 구성 요소의 기능을 최대한 재사용하는 것이 좋습니다 *이전* 사용자 지정 코드를 작성하는 중입니다.

1. 다음으로 핵심 구성 요소 페이지 검사 위치 `/libs/core/wcm/components/page/v2/page` CRXDE Lite 사용:

   >[!NOTE]
   >
   > 에서 AEM 6.5/6.4의 핵심 구성 요소는 아래에 있습니다 `/apps/core/wcm/components`. AEM as a Cloud Service에서 핵심 구성 요소는 아래에 있습니다 `/libs` 및 은(는) 자동으로 업데이트됩니다.

   ![핵심 구성 요소 페이지](assets/pages-templates/core-page-component-properties.png)

   많은 스크립트 파일이 이 페이지 아래에 포함되어 있습니다. 핵심 구성 요소 페이지에는 다양한 기능이 포함되어 있습니다. 이 기능은 유지 보수 및 가독성을 향상시키기 위해 여러 스크립트로 분할됩니다. HTL 스크립트를 `page.html` 그리고 `data-sly-include`:

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

   HTL을 여러 스크립트로 분할하는 또 다른 이유는 프록시 구성 요소가 개별 스크립트를 재정의하여 사용자 지정 비즈니스 논리를 구현할 수 있도록 하기 위한 것입니다. HTL 스크립트 `customfooterlibs.html`, 및 `customheaderlibs.html`은 프로젝트를 구현하여 재정의할 명시적 목적으로 만들어집니다.

   편집 가능한 템플릿 요소가 렌더링을 구성하는 방법에 대해 자세히 알아볼 수 있습니다 [이 문서를 읽은 컨텐츠 페이지](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html).

1. 이동 경로에서 과 같은 다른 핵심 구성 요소 Inspect `/libs/core/wcm/components/breadcrumb/v2/breadcrumb`. 보기 `breadcrumb.html` 이동 경로 구성 요소에 대한 마크업이 최종적으로 생성되는 방법을 이해하는 스크립트입니다.

## 소스 컨트롤에 구성 저장 {#configuration-persistence}

특히 AEM 프로젝트 시작 시 템플릿 및 관련 컨텐츠 정책과 같은 구성을 소스 제어에 유지하는 것이 중요합니다. 이렇게 하면 모든 개발자가 동일한 컨텐츠 및 구성 세트에 대해 작업하고 있으므로 환경 간에 추가적인 일관성을 유지할 수 있습니다. 프로젝트가 일정 수준의 성숙기에 도달하면 템플릿 관리 방법을 특수 사용자 그룹으로 전환할 수 있습니다.


현재 템플릿은 다른 코드 조각처럼 처리되고 **문서 페이지 템플릿** 프로젝트의 일부로서.
지금까지 코드는 AEM 프로젝트에서 AEM의 로컬 인스턴스로 푸시됩니다. 다음 **문서 페이지 템플릿** 는 AEM의 로컬 인스턴스에 직접 만들어지므로 **가져오기** 템플릿을 AEM 프로젝트에 추가합니다. 다음 **ui.content** 이 특정 목적을 위해 모듈이 AEM 프로젝트에 포함되어 있습니다.

다음 몇 단계는 를 사용하여 VSCode IDE에서 수행됩니다 [VSCode AEM 동기화](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&amp;ssr=false#overview) 플러그인. 그러나 사용자가 구성한 모든 IDE를 사용할 수 있습니다 **가져오기** 또는 AEM의 로컬 인스턴스에서 컨텐츠를 가져옵니다.

1. 에서 VSCode가 `aem-guides-wknd` 프로젝트.

1. 를 확장합니다. **ui.content** 모듈(프로젝트 탐색기)을 클릭하여 제품에서 사용할 수 있습니다. 를 확장합니다. `src` 폴더 및 이동 `/conf/wknd/settings/wcm/templates`.

1. [!UICONTROL 마우스 오른쪽 단추 클릭] a `templates` 폴더를 선택하고 **AEM 서버에서 가져오기**:

   ![VSCode 가져오기 템플릿](assets/pages-templates/vscode-import-templates.png)

   다음 `article-page` 는 `page-content`, `xf-web-variation` 템플릿도 업데이트해야 합니다.

   ![업데이트된 템플릿](assets/pages-templates/updated-templates.png)

1. 단계를 반복하여 컨텐츠를 가져오지만 **정책** 폴더 위치 `/conf/wknd/settings/wcm/policies`.

   ![VSCode 가져오기 정책](assets/pages-templates/policies-article-page-template.png)

1. Inspect `filter.xml` 파일 위치 `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

   다음 `filter.xml` 파일은 패키지와 함께 설치된 노드의 경로를 식별해야 합니다. 다음 사항에 주의하십시오. `mode="merge"` 기존 콘텐츠를 수정하지 않음을 나타내는 각 필터에 새로운 콘텐츠만 추가됩니다. 컨텐츠 작성자는 이러한 경로를 업데이트할 수 있으므로 코드 배포가 수행하는 것이 중요합니다 **not** 콘텐츠를 덮어씁니다. 자세한 내용은 [FileVault 설명서](https://jackrabbit.apache.org/filevault/filter.html) 를 참조하십시오.

   비교 `ui.content/src/main/content/META-INF/vault/filter.xml` 및 `ui.apps/src/main/content/META-INF/vault/filter.xml` 각 모듈에서 관리하는 서로 다른 노드를 이해하기 위해

   >[!WARNING]
   >
   > WKND 참조 사이트에 대한 일관된 배포를 위해 프로젝트의 일부 분기가 설정되도록 합니다 `ui.content` 는 JCR의 모든 변경 사항을 덮어씁니다. 코드/스타일이 특정 정책에 대해 작성되므로 솔루션 분기 등의 디자인을 통해 이루어집니다.

## 축하합니다! {#congratulations}

축하합니다. Adobe Experience Manager Sites을 사용하여 템플릿과 페이지를 만들었습니다.

### 다음 단계 {#next-steps}

이 시점에서 기사 페이지는 분명히 스타일이 지정되지 않았습니다. 다음을 수행합니다 [클라이언트 측 라이브러리 및 프런트 엔드 워크플로우](client-side-libraries.md) 사이트에 전역 스타일을 적용하고 전용 프런트 엔드 빌드를 통합하는 CSS 및 JavaScript를 포함하는 우수 사례를 살펴보려면 자습서를 참조하십시오.

에서 완성된 코드 보기 [GitHub](https://github.com/adobe/aem-guides-wknd) 코드를 Git 분기에서 로컬로 검토하고 배포합니다 `tutorial/pages-templates-solution`.

1. 복제 [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 저장소.
1. 다음을 확인하십시오 `tutorial/pages-templates-solution` 분기
