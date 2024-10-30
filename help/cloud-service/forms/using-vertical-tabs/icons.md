---
title: AEM Formsas a Cloud Service 에서 세로 탭 사용
description: 세로 탭에 사용자 지정 아이콘 추가
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
thumbnail: 331891.jpg
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16418
source-git-commit: 1ed08d7784833b6c49139da525341af5ee587345
workflow-type: tm+mt
source-wordcount: '684'
ht-degree: 0%

---

# 사용자 정의 아이콘 추가

사용자 지정 아이콘을 탭에 추가하면 몇 가지 방법으로 사용자 경험과 시각적 호소력을 향상시킬 수 있습니다.

* 향상된 유용성: 아이콘은 각 탭의 목적을 빠르게 전달할 수 있어 사용자가 원하는 내용을 한눈에 쉽게 찾을 수 있습니다. 아이콘과 같은 시각적 큐를 사용하면 보다 직관적으로 탐색할 수 있습니다.

* 시각적 계층 구조 및 포커스: 아이콘은 탭 간의 보다 명확한 분리를 만들어 시각적 계층을 향상시킵니다. 이는 중요한 탭이 돋보이고 사용자의 주의를 더 효과적으로 유도하는 데 도움이 될 수 있습니다.
이 문서에 따라 아래와 같이 아이콘을 배치할 수 있습니다

![아이콘](assets/icons.png)

## 사전 요구 사항

이 문서를 팔로우하려면 Git, cloud manager를 사용하여 AEM 프로젝트 생성 및 배포, AEM cloud manager의 프론트엔드 파이프라인 설정 및 약간의 CSS에 익숙해야 합니다. 위에서 언급한 주제에 익숙하지 않은 경우 [테마를 사용하여 핵심 구성 요소 스타일링](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/using-themes-in-core-components#rename-env-file-theme-folder) 문서를 팔로우하십시오.

## 테마에 아이콘 추가

Visual Studio 코드 또는 선택한 다른 편집기에서 테마 프로젝트를 엽니다.
선택한 아이콘을 이미지 폴더에 추가합니다.
빨간색으로 표시된 아이콘은 새 아이콘입니다.
![새 아이콘](assets/newicons.png)

## 아이콘을 저장할 아이콘 맵 만들기

_variable.scss 파일에 대한 icon-map을 만듭니다. SCSS 맵 $icon-map은 키-값 쌍의 컬렉션입니다. 여기서 각 키는 아이콘 이름(예: 홈, 패밀리 등)을 나타내고 각 값은 해당 아이콘과 연결된 이미지 파일의 경로입니다.

![variable-scss](assets/variable.scss)

```css
$icon-map: (
    home: "./resources/images/home.png",
    family: "./resources/images/icons8-family-80.png",
    pdf: "./resources/images/pdf.png",
    income: "./resources/images/income.png",
    assets: "./resources/images/assets.png",
    cars: "./resources/images/cars.png"
);
```

## mixin 추가

_mixin.scss에 다음 코드 추가

```css
@mixin add-icon-to-vertical-tab($image-url) {
  display: inline-flex;
  align-self: center;
  &::before {
    content: "";
    display:inline-block;
    background: url($image-url) left center / cover no-repeat;
    margin-right: 8px; /* Space between icon and text */
    height:40px;
    width:40px;
    vertical-align:middle;
    
  }
  
}
```

추가 아이콘-세로 탭 mixin은 세로 탭의 텍스트 옆에 사용자 지정 아이콘을 추가하도록 설계되었습니다. 이 옵션을 사용하면 이미지를 탭에 아이콘으로 쉽게 포함하고, 텍스트 옆에 배치한 다음 스타일링하여 일관성과 정렬을 보장할 수 있습니다.

Mixin 분류
다음은 mixin의 각 부분이 수행하는 작업입니다.

매개 변수:

* $image-url: 탭 텍스트 옆에 표시할 아이콘 또는 이미지의 URL입니다. 이 매개 변수를 전달하면 필요에 따라 다른 탭에 다른 아이콘을 추가할 수 있으므로 mixin을 다양하게 사용할 수 있습니다.

* 스타일 적용됨:

   * 표시: inline-flex: 요소를 플렉스 컨테이너로 만들어 중첩된 콘텐츠(예: 아이콘 및 텍스트)를 가로로 정렬합니다.
   * align-self: center: 요소가 컨테이너 내에 수직으로 가운데에 배치됩니다.
   * 의사 요소(::before):
   * content: &quot;&quot;: 아이콘을 배경 이미지로 표시하는 데 사용되는 ::before 의사 요소를 초기화합니다.
   * display: inline-block: 의사 요소를 인라인 블록으로 설정하여 텍스트와 인라인으로 배치된 아이콘처럼 동작합니다.
   * background: url($image-url) left center / cover no-repeat;: $image-url을 통해 제공된 URL을 사용하여 배경 이미지를 추가합니다. 아이콘이 왼쪽에 정렬되고 세로로 가운데에 정렬됩니다.

## _verticaltab.scss 업데이트

이 문서에 탭 아이콘을 표시하는 새 css 클래스(cmp-verticaltab—marketing)를 만들었습니다. 이 새 클래스에서는 아이콘을 추가하여 탭 요소를 확장합니다. css 클래스의 전체 목록은 다음과 같습니다

```css
.cmp-verticaltabs--marketing
{
  .cmp-verticaltabs
    {
      &__tab 
        {
          cursor:pointer;
            @each $name, $url in $icon-map {
            &[data-icon-name="#{$name}"]
              {
                  @include add-icon-to-vertical-tab($url);
              }
            }
        }
    }
}
```

## Verticaltab 구성 요소 수정

```/apps/core/fd/components/form/verticaltabs/v1/verticaltabs/verticaltabs.html```에서 verticaltab.html 파일을 복사하여 프로젝트의 verticaltab 구성 요소 아래에 붙여넣습니다. 아래 이미지에 표시된 대로 li 역할 아래의 복사된 파일에 다음 줄 ```data-icon-name="${tab.name}"```을(를) 추가합니다.
![data-icon](assets/data-icons.png)
data-icon-name이라는 사용자 지정 데이터 속성을 탭 이름 값으로 설정하는 중입니다.탭 이름이 아이콘 맵의 이미지 이름과 일치하는 경우 해당 이미지는 탭과 연결됩니다.



## 코드 테스트

업데이트된 verticaltab 구성 요소를 클라우드 인스턴스에 배포합니다.
프론트엔드 파이프라인을 사용하여 업데이트된 테마를 배포합니다.
아래 그림과 같이 세로 탭 구성 요소에 대한 스타일 변형을 만듭니다
![style-variation](assets/verticaltab-style-variation.png)
css 클래스 _**cmp-verticaltab—marketing**_과(와) 연결된 Marketing이라는 스타일 변형을 만들었습니다.
세로 탭 구성 요소를 사용하여 적응형 양식을 만듭니다. 세로 탭 구성 요소를 마케팅 스타일 변형과 연결합니다.
수직 탭에 탭 두 개를 추가하고 홈, 패밀리와 같이 아이콘 맵에 정의된 이미지와 일치하도록 탭 이름을 지정합니다.
![home-icon](assets/tab-name.png)

양식을 미리 보면 탭과 관련된 적절한 아이콘이 표시됩니다
