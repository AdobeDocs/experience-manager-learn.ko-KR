---
title: 원격 SPA 동적 경로에 편집 가능한 구성 요소 추가
description: 원격 SPA에서 동적 경로에 편집 가능한 구성 요소를 추가하는 방법을 알아봅니다.
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
duration: 260
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '903'
ht-degree: 0%

---

# 동적 경로 및 편집 가능한 구성 요소

이 장에서는 편집 가능한 구성 요소를 지원하기 위해 두 개의 동적 Adventure Detail 경로를 활성화합니다. __발리 서프 캠프__ 및 __비어바나 인 포틀랜드__.

![동적 경로 및 편집 가능한 구성 요소](./assets/spa-dynamic-routes/intro.png)

Adventure Detail SPA 경로는 `/adventure/:slug` 위치 `slug` 는 어드벤처 콘텐츠 조각의 고유 식별자 속성입니다.

## SPA URL을 AEM 페이지에 매핑

이전 두 장에서는 SPA 홈 보기의 편집 가능한 구성 요소 컨텐츠를 AEM 의 해당 원격 SPA 루트 페이지에 매핑했습니다. `/content/wknd-app/us/en/`.

SPA 동적 경로에 대해 편집 가능한 구성 요소에 대한 매핑을 정의하는 것은 유사하지만 경로 인스턴스와 AEM 페이지 인스턴스 간에 1:1 매핑 구성표를 제시해야 합니다.

이 자습서에서는 경로의 마지막 세그먼트인 WKND Adventure 콘텐츠 조각의 이름을 가져와 아래의 간단한 경로에 매핑합니다 `/content/wknd-app/us/en/adventure`.

| 원격 SPA 경로 | AEM 페이지 경로 |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/en/home |
| /adventure/__발리 서프 캠프__ | /content/wknd-app/us/en/home/adventure/__발리 서프 캠프__ |
| /adventure/__비어바나-포틀랜드__ | /content/wknd-app/us/en/home/adventure/__비어바나인포틀랜드__ |

따라서 이 매핑을 기반으로 다음 위치에 새 AEM 페이지를 두 개 만들어야 합니다.

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## 원격 SPA 매핑

원격 SPA을 떠나는 요청에 대한 매핑은 `setupProxy` 에서 구성 완료 [SPA Bootstrap](./spa-bootstrap.md).

## SPA 편집기 매핑

SPA 편집기를 통해 SPA을 열 때 AEM SPA 요청에 대한 매핑은에서 수행되는 Sling 매핑 구성을 통해 구성됩니다. [AEM 구성](./aem-configure.md).

## AEM에서 컨텐츠 페이지 만들기

먼저, 중개자를 만듭니다 `adventure` 페이지 세그먼트:

1. AEM 작성자에 로그인
1. 다음으로 이동 __사이트 > WKND 앱 > us > en > WKND 앱 홈 페이지__
   + 이 AEM 페이지는 SPA의 루트로 매핑되므로 다른 SPA 루트에 대한 AEM 페이지 구조를 빌드하기 시작합니다.
1. 누르기 __만들기__ 및 선택 __페이지__
1. 다음 항목 선택 __원격 SPA 페이지__ 템플릿 및 탭 __다음__
1. 페이지 속성 작성
   + __제목__: 어드벤처
   + __이름__: `adventure`
      + 이 값은 AEM 페이지의 URL을 정의하므로 SPA의 경로 세그먼트와 일치해야 합니다.
1. 누르기 __완료__

그런 다음 편집 가능한 영역이 필요한 각 AEM URL에 해당하는 SPA 페이지를 만듭니다.

1. 새 항목으로 이동 __모험__ 사이트 관리자의 페이지
1. 누르기 __만들기__ 및 선택 __페이지__
1. 다음 항목 선택 __원격 SPA 페이지__ 템플릿 및 탭 __다음__
1. 페이지 속성 작성
   + __제목__: 발리 서프 캠프
   + __이름__: `bali-surf-camp`
      + 이 값은 AEM 페이지의 URL을 정의하므로 SPA 경로의 마지막 세그먼트와 일치해야 합니다
1. 누르기 __완료__
1. 3-6단계를 반복하여 __비어바나 인 포틀랜드__ 페이지, 포함:
   + __제목__&#x200B;포틀랜드의 비어바나
   + __이름__: `beervana-in-portland`
      + 이 값은 AEM 페이지의 URL을 정의하므로 SPA 경로의 마지막 세그먼트와 일치해야 합니다

이 두 AEM 페이지에는 일치하는 SPA 경로에 대해 각자가 작성한 콘텐츠가 보관됩니다. 다른 SPA 경로를 작성해야 하는 경우 새 AEM SPA 페이지를 원격 SPA 페이지의 루트 페이지(`/content/wknd-app/us/en/home`AEM )을 참조하십시오.

## WKND 앱 업데이트

다음을 배치합니다. `<ResponsiveGrid...>` 에서 생성된 구성 요소 [마지막 챕터](./spa-container-component.md), 다음으로 이동 `AdventureDetail` SPA 구성 요소, 편집 가능한 컨테이너 만들기

### ResponsiveGrid SPA 구성 요소 배치

배치 `<ResponsiveGrid...>` 다음에서 `AdventureDetail` 구성 요소는 해당 경로에서 편집 가능한 컨테이너를 만듭니다. 비결은 여러 루트에서 `AdventureDetail` 렌더링할 구성 요소입니다. 다음을 동적으로 조정해야 합니다.  `<ResponsiveGrid...>'s pagePath` 특성. 다음 `pagePath` 경로의 인스턴스가 표시하는 모험을 기반으로 해당 AEM 페이지를 가리키도록 파생되어야 합니다.

1. 열기 및 편집 `react-app-/src/components/AdventureDetail.js`
1. 가져오기 `ResponsiveGrid` 구성 요소 및 위에 배치 `<h2>Itinerary</h2>` 구성 요소.
1. 에서 다음 속성을 설정합니다. `<ResponsiveGrid...>` 구성 요소. 다음을 참고하십시오. `pagePath` 속성이 현재 을 추가합니다. `slug` 위에서 정의한 매핑에 따라 어드벤처 페이지에 매핑됩니다.
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${slug}'`
   + `itemPath = 'root/responsivegrid'`

   이에 따라 `ResponsiveGrid` AEM 리소스에서 콘텐츠를 검색할 구성 요소:

   + `/content/wknd-app/us/en/home/adventure/${slug}/jcr:content/root/responsivegrid`

업데이트 `AdventureDetail.js` 다음 줄 포함:

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

다음 `AdventureDetail.js` 파일은 다음과 같아야 합니다.

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## AEM에서 컨테이너 작성

포함 `<ResponsiveGrid...>` 제 자리에, 그리고 `pagePath` 렌더링되는 모험에 따라 동적으로 설정되므로 콘텐츠 작성을 시도합니다.

1. AEM 작성자에 로그인
1. 다음으로 이동 __사이트 > WKND 앱 > us > en__
1. __편집__ 다음 __WKND 앱 홈 페이지__ 페이지
   + 다음 위치로 이동 __발리 서프 캠프__ 편집할 SPA에서 경로 지정
1. 선택 __미리 보기__ 오른쪽 상단의 모드 선택기에서
1. 을 누릅니다. __발리 서프 캠프__ 경로로 이동할 SPA의 카드
1. 선택 __편집__ 모드 선택기에서
1. 를 찾습니다. __레이아웃 컨테이너__ 편집 가능한 영역 바로 위 __여행 일정__
1. 를 엽니다. __페이지 편집기의 사이드바__&#x200B;을(를) 클릭하고 __구성 요소 보기__
1. 활성화된 구성 요소 중 일부를 __레이아웃 컨테이너__
   + 이미지
   + 텍스트
   + 제목

   그리고 홍보 마케팅 자료를 만드세요. 다음과 같은 모습일 수 있습니다.

   ![발리 어드벤처 세부 사항 작성](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. __미리 보기__ AEM 페이지 편집기의 변경 사항
1. 로컬에서 실행 중인 WKND 앱 새로 고침 [http://localhost:3000](http://localhost:3000)로 이동한 다음 __발리 서프 캠프__ 작성된 변경 내용을 보기 위한 경로!

   ![원격 SPA Bali](./assets/spa-dynamic-routes/remote-spa-final.png)

매핑된 AEM 페이지가 없는 모험 세부 경로로 이동하면 해당 경로 인스턴스에 작성 기능이 없습니다. 이러한 페이지에서 작성을 활성화하려면 아래에서 이름이 일치하는 AEM 페이지를 만들면 됩니다. __모험__ 페이지!

## 축하합니다!

축하합니다! SPA에서 동적 경로에 작성 기능을 추가했습니다.

+ AEM React Editable Component의 ResponsiveGrid 구성 요소를 동적 경로에 추가했습니다
+ SPA(포틀랜드의 발리 서프 캠프와 비어바나)에서 두 개의 특정 경로 작성을 지원하도록 AEM 페이지를 만들었습니다.
+ 역동적인 발리 서프 캠프 노선에 콘텐츠를 작성!

이제 AEM SPA Editor를 사용하여 원격 SPA에 특정 편집 가능한 영역을 추가하는 방법의 첫 번째 단계를 살펴보았습니다.
