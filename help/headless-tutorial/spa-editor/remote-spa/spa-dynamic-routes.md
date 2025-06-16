---
title: 원격 SPA의 동적 경로에 편집 가능한 구성 요소 추가
description: 원격 SPA에서 동적 경로에 편집 가능한 구성 요소를 추가하는 방법에 대해 알아봅니다.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7636
thumbnail: kt-7636.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: 4accc1ca-6f4b-449e-bf2e-06f19d2fe17d
duration: 202
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '903'
ht-degree: 0%

---

# 동적 경로 및 편집 가능한 구성 요소

{{spa-editor-deprecation}}

이 장에서는 편집할 수 있는 구성 요소를 지원하기 위해 두 개의 동적 모험 세부 경로(__Bali Surf Camp__ 및 __Beervana in Portland__)를 활성화합니다.

![동적 경로 및 편집 가능한 구성 요소](./assets/spa-dynamic-routes/intro.png)

Adventure Detail SPA 경로가 `/adventure/:slug`(으)로 정의되어 있습니다. 여기서 `slug`은(는) Adventure 콘텐츠 조각의 고유 식별자 속성입니다.

## AEM 페이지에 SPA URL 매핑

이전 두 장에서는 SPA의 홈 보기에서 편집 가능한 구성 요소 콘텐츠를 `/content/wknd-app/us/en/`의 AEM에 있는 해당 원격 SPA 루트 페이지에 매핑했습니다.

SPA의 동적 경로에 대해 편집 가능한 구성 요소에 대한 매핑을 정의하는 것은 비슷하지만 경로 인스턴스와 AEM 페이지 간에 1:1 매핑 구성표를 제시해야 합니다.

이 자습서에서는 경로의 마지막 세그먼트인 WKND Adventure 콘텐츠 조각의 이름을 가져와 `/content/wknd-app/us/en/adventure` 아래의 간단한 경로에 매핑합니다.

| 원격 SPA 경로 | AEM 페이지 경로 |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/en/home |
| /adventure/__bali-surf-camp__ | /content/wknd-app/us/en/home/adventure/__bali-surf-camp__ |
| /adventure/__베어바나-포틀랜드__ | /content/wknd-app/us/en/home/adventure/__beervana-in-portland__ |

따라서 이 매핑을 기반으로 다음 위치에 새 AEM 페이지를 두 개 만들어야 합니다.

* `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
* `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## 원격 SPA 매핑

원격 SPA에서 나가는 요청에 대한 매핑은 [SPA의 Bootstrap](./spa-bootstrap.md)에서 수행한 `setupProxy` 구성을 통해 구성됩니다.

## SPA 편집기 매핑

SPA가 AEM SPA 편집기를 통해 열릴 때 SPA 요청에 대한 매핑은 [AEM 구성](./aem-configure.md)에서 수행한 Sling 매핑 구성을 통해 구성됩니다.

## AEM에서 컨텐츠 페이지 만들기

먼저 중간 `adventure` 페이지 세그먼트를 만듭니다.

1. AEM 작성자에 로그인
1. __사이트 > WKND 앱 > us > en > WKND 앱 홈 페이지로 이동합니다__
   1. 이 AEM 페이지는 SPA의 루트로 매핑되므로 여기에서 다른 SPA 경로에 대한 AEM 페이지 구조를 구축합니다.
1. __만들기__&#x200B;를 탭하고 __페이지__&#x200B;를 선택하세요.
1. __원격 SPA 페이지__ 템플릿을 선택하고 __다음__&#x200B;을 누릅니다.
1. 페이지 속성 작성
   1. __제목__: 모험
   1. __이름__: `adventure`
      1. 이 값은 AEM 페이지의 URL을 정의하므로 SPA의 경로 세그먼트와 일치해야 합니다.
1. __완료__ 탭

그런 다음 편집 가능한 영역이 필요한 SPA의 각 URL에 해당하는 AEM 페이지를 만듭니다.

1. 사이트 관리자의 새 __모험__ 페이지로 이동
1. __만들기__&#x200B;를 탭하고 __페이지__&#x200B;를 선택하세요.
1. __원격 SPA 페이지__ 템플릿을 선택하고 __다음__&#x200B;을 누릅니다.
1. 페이지 속성 작성
   1. __제목__: 발리 서프 캠프
   1. __이름__: `bali-surf-camp`
      1. 이 값은 AEM 페이지의 URL을 정의하므로 SPA의 마지막 경로와 일치해야 합니다
1. __완료__ 탭
1. 3-6단계를 반복하여 __Beervana in Portland__ 페이지를 만듭니다.
   1. __제목__: Portland의 Beervana
   1. __이름__: `beervana-in-portland`
      1. 이 값은 AEM 페이지의 URL을 정의하므로 SPA의 마지막 경로와 일치해야 합니다

이 두 AEM 페이지에는 일치하는 SPA 경로에 대해 각각 작성된 콘텐츠가 들어 있습니다. 다른 SPA 경로를 작성해야 하는 경우 AEM에서 원격 SPA 페이지의 루트 페이지(`/content/wknd-app/us/en/home`) 아래에 있는 SPA의 URL에서 새 AEM 페이지를 만들어야 합니다.

## WKND 앱 업데이트

[마지막 챕터](./spa-container-component.md)에서 만든 `<ResponsiveGrid...>` 구성 요소를 `AdventureDetail` SPA 구성 요소에 배치하여 편집 가능한 컨테이너를 만들어 보겠습니다.

### ResponsiveGrid SPA 구성 요소 배치

`<ResponsiveGrid...>`을(를) `AdventureDetail` 구성 요소에 배치하면 해당 경로에서 편집 가능한 컨테이너가 만들어집니다. 여러 경로에서 `AdventureDetail` 구성 요소를 사용하여 렌더링하므로 `<ResponsiveGrid...>'s pagePath` 특성을 동적으로 조정해야 합니다. `pagePath`은(는) 경로의 인스턴스가 표시하는 모험을 기반으로 해당 AEM 페이지를 가리키도록 파생되어야 합니다.

1. `react-app-/src/components/AdventureDetail.js` 열기 및 편집
1. `ResponsiveGrid` 구성 요소를 가져와 `<h2>Itinerary</h2>` 구성 요소 위에 놓습니다.
1. `<ResponsiveGrid...>` 구성 요소에 대해 다음 특성을 설정합니다. `pagePath` 특성은 위에 정의된 매핑에 따라 모험 페이지에 매핑되는 현재 `slug`을(를) 추가합니다.
   1. `pagePath = '/content/wknd-app/us/en/home/adventure/${slug}'`
   1. `itemPath = 'root/responsivegrid'`

   이렇게 하면 `ResponsiveGrid` 구성 요소가 AEM 리소스에서 해당 콘텐츠를 검색하도록 지시합니다.

   1. `/content/wknd-app/us/en/home/adventure/${slug}/jcr:content/root/responsivegrid`

`AdventureDetail.js`을(를) 다음 줄로 업데이트하십시오.

```javascript
...
import { ResponsiveGrid } from '@adobe/aem-react-editable-components';
...

function AdventureDetailRender(props) {
    ...
    // Get the slug from the React route parameter, this will be used to specify the AEM Page to store/read editable content from
    const { slug } = useParams();

    return(
        ...
        // Pass the slug in
        function AdventureDetailRender({ title, primaryImage, activity, adventureType, tripLength, 
                groupSize, difficulty, price, description, itinerary, references, slug }) {
            ...
            return (
                ...
                <ResponsiveGrid 
                    pagePath={`/content/wknd-app/us/en/home/adventure/${slug}`}
                    itemPath="root/responsivegrid"/>
                    
                <h2>Itinerary</h2>
                ...
            )
        }
    )
}
```

`AdventureDetail.js` 파일은 다음과 같아야 합니다.

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## AEM에서 컨테이너 작성

`<ResponsiveGrid...>`이(가) 제자리에 있고 렌더링되는 모험에 따라 해당 `pagePath`이(가) 동적으로 설정된 상태에서 콘텐츠 작성을 시도합니다.

1. AEM 작성자에 로그인
1. __사이트 > WKND 앱 > us > en__(으)로 이동합니다.
1. __WKND 앱 홈 페이지__ 페이지를 __편집__
   1. SPA에서 __Bali Surf Camp__ 경로로 이동하여 편집합니다.
1. 오른쪽 상단의 모드 선택기에서 __미리 보기__&#x200B;를 선택합니다.
1. SPA에서 __Bali Surf Camp__ 카드를 탭하여 해당 경로로 이동합니다.
1. 모드 선택기에서 __편집__ 선택
1. __일정__ 바로 위에서 __레이아웃 컨테이너__ 편집 가능 영역을 찾습니다.
1. __페이지 편집기의 사이드바__&#x200B;를 열고 __구성 요소 보기__&#x200B;를 선택합니다.
1. 사용 가능한 구성 요소 중 일부를 __레이아웃 컨테이너__(으)로 끌어서 놓습니다.
   1. 이미지
   1. 텍스트
   1. 제목

   그리고 홍보 마케팅 자료를 만드세요. 다음과 같은 모습일 수 있습니다.

   ![발리 어드벤처 세부 정보 작성](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. AEM 페이지 편집기에서 변경 내용을 __미리 보기__
1. [http://localhost:3000](http://localhost:3000)에서 로컬로 실행되는 WKND 앱을 새로 고치고 __Bali Surf Camp__ 경로로 이동하여 작성된 변경 내용을 확인하십시오!

   ![원격 SPA 발리](./assets/spa-dynamic-routes/remote-spa-final.png)

매핑된 AEM 페이지가 없는 모험 세부 경로로 이동하면 해당 경로 인스턴스에 작성 기능이 없습니다. 이러한 페이지에서 작성을 활성화하려면 __모험__ 페이지에서 이름이 일치하는 AEM 페이지를 만들면 됩니다!

## 축하합니다!

축하합니다! SPA에서 동적 경로에 작성 기능을 추가했습니다.

* AEM React Editable Component의 ResponsiveGrid 구성 요소를 동적 경로에 추가했습니다
* SPA의 두 가지 특정 경로(포틀랜드의 발리 서프 캠프 및 비어바나)를 작성할 수 있도록 AEM 페이지를 만들었습니다.
* 역동적인 발리 서프 캠프 노선에 콘텐츠를 작성!

이제 AEM SPA 편집기를 사용하여 원격 SPA에 특정 편집 가능한 영역을 추가하는 방법의 첫 번째 단계를 살펴보았습니다!
