---
title: 원격 SPA 동적 경로에 편집 가능한 구성 요소 추가
description: 원격 SPA에서 동적 경로에 편집 가능한 구성 요소를 추가하는 방법을 알아봅니다.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7636
thumbnail: kt-7636.jpeg
exl-id: 4accc1ca-6f4b-449e-bf2e-06f19d2fe17d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '916'
ht-degree: 1%

---

# 동적 경로 및 편집 가능한 구성 요소

이 장에서는 두 개의 동적 Adventure Detail 경로를 사용하여 편집 가능한 구성 요소를 지원합니다. __발리 서프 캠프__ 및 __포틀랜드의 베어바나__.

![동적 경로 및 편집 가능한 구성 요소](./assets/spa-dynamic-routes/intro.png)

Adventure Detail SPA 경로는 다음과 같이 정의됩니다. `/adventure:path` 여기서 `path` 은 WKND Adventure(컨텐츠 조각)에 대한 세부 사항을 표시하는 경로입니다.

## AEM 페이지에 SPA URL 매핑

이전 두 장에서는 SPA 홈 보기의 편집 가능한 구성 요소 컨텐츠를 AEM 의 해당 원격 SPA 루트 페이지에 매핑했습니다. `/content/wknd-app/us/en/`.

SPA 동적 경로의 편집 가능한 구성 요소에 대한 매핑을 정의하는 것은 비슷하지만 라우트의 인스턴스와 AEM 페이지 간에 1:1 매핑 구성표를 작성해야 합니다.

이 자습서에서는 경로의 마지막 세그먼트인 WKND Adventure 컨텐츠 조각의 이름을 가져와 아래의 간단한 경로에 매핑합니다 `/content/wknd-app/us/en/adventure`.

| 원격 SPA 경로 | AEM 페이지 경로 |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/en/home |
| /adventure:/content/dam/wknd/en/adventures/bali-surf-camp/__발리 서핑 캠프__ | /content/wknd-app/us/en/home/adventure/__발리 서핑 캠프__ |
| /adventure:/content/dam/wknd/en/adventures/beervanna-portland/__베어바나 포틀랜드__ | /content/wknd-app/us/en/home/adventure/__베어바나 인 포틀랜드__ |

따라서 이 매핑을 기반으로 에서는 두 개의 새 AEM 페이지를 만들어야 합니다.

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## 원격 SPA 매핑

Remote SPA에서 나가는 요청에 대한 매핑은 `setupProxy` 구성 완료 [SPA Bootstrap](./spa-bootstrap.md).

## SPA 편집기 매핑

AEM SPA Editor를 통해 SPA이 열릴 때 SPA 요청에 대한 매핑은에서 수행된 Sling 매핑 구성을 통해 구성됩니다. [AEM 구성](./aem-configure.md).

## AEM에서 컨텐츠 페이지 만들기

먼저, 중재자 만들기 `adventure` 페이지 세그먼트:

1. AEM 작성자에 로그인
1. 다음으로 이동 __사이트 > WKND 앱 > us > en > WKND 앱 홈 페이지__
   + 이 AEM 페이지는 SPA의 루트로 매핑되므로 여기에서 다른 SPA 경로에 대한 AEM 페이지 구조 작성을 시작합니다.
1. 탭 __만들기__ 을(를) 선택합니다. __페이지__
1. 을(를) 선택합니다 __원격 SPA 페이지__ 템플릿 및 탭 __다음__
1. 페이지 속성 채우기
   + __제목__: 모험
   + __이름__: `adventure`
      + 이 값은 AEM 페이지의 URL을 정의하므로 SPA의 경로 세그먼트와 일치해야 합니다.
1. __Done__&#x200B;을 누릅니다

그런 다음 편집 가능한 영역이 필요한 각 SPA URL에 해당하는 AEM 페이지를 만듭니다.

1. 새 페이지로 이동합니다 __모험__ 사이트 관리자의 페이지
1. 탭 __만들기__ 을(를) 선택합니다. __페이지__
1. 을(를) 선택합니다 __원격 SPA 페이지__ 템플릿 및 탭 __다음__
1. 페이지 속성 채우기
   + __제목__: 발리 서프 캠프
   + __이름__: `bali-surf-camp`
      + 이 값은 AEM 페이지의 URL을 정의하므로 SPA 경로의 마지막 세그먼트와 일치해야 합니다
1. __Done__&#x200B;을 누릅니다
1. 3-6단계를 반복하여 를 만듭니다 __포틀랜드의 베어바나__ 페이지, 다음 포함:
   + __제목__: 포틀랜드의 베어바나
   + __이름__: `beervana-in-portland`
      + 이 값은 AEM 페이지의 URL을 정의하므로 SPA 경로의 마지막 세그먼트와 일치해야 합니다

이 두 AEM 페이지에는 일치하는 SPA 경로에 대해 각각 작성된 컨텐츠가 있습니다. 다른 SPA 라우팅이 작성을 필요로 하는 경우 새 AEM 페이지는 원격 SPA 페이지의 루트 페이지(`/content/wknd-app/us/en/home`)을 클릭하여 제품에서 사용할 수 있습니다.

## WKND 앱 업데이트

그럼 `<AEMResponsiveGrid...>` 에서 만들어진 구성 요소 [마지막 장](./spa-container-component.md)에 대해 `AdventureDetail` SPA 구성 요소, 편집 가능한 컨테이너 만들기

### AEMRresponsiveGrid SPA 구성 요소 배치

배치 `<AEMResponsiveGrid...>` 에서 `AdventureDetail` 구성 요소는 해당 경로에 편집 가능한 컨테이너를 만듭니다. 방법은 여러 경로에서 `AdventureDetail` 렌더링할 구성 요소는 동적으로 조정해야 합니다  `<AEMResponsiveGrid...>'s pagePath` 속성을 사용합니다. 다음 `pagePath` 경로 인스턴스가 표시되는 탐색을 기반으로 해당 AEM 페이지를 가리키도록 파생되어야 합니다.

1. 열기 및 편집 `react-app/src/components/AdventureDetail.js`
1. 앞에 다음 줄을 추가합니다 `AdventureDetail(..)'s` second `return(..)` 컨텐츠 조각 경로에서 모험 이름을 파생시키는 문입니다.

   ```
   ...
   // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
   const adventureName = _path.split('/').pop();
   ...
   ```

1. 가져오기 `AEMResponsiveGrid` 구성 요소를 위에 배치하여 `<h2>Itinerary</h2>` 구성 요소.
1. 에서 다음 속성을 설정합니다. `<AEMResponsiveGrid...>` 구성 요소
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${adventureName}'`
   + `itemPath = 'root/responsivegrid'`

   이를 통해 `AEMResponsiveGrid` AEM 리소스에서 컨텐츠를 검색하는 구성 요소입니다.

   + `/content/wknd-app/us/en/home/adventure/${adventureName}/jcr:content/root/responsivegrid`


업데이트 `AdventureDetail.js` 다음 줄 사용:

```
...
import AEMResponsiveGrid from '../components/aem/AEMResponsiveGrid';
...

function AdventureDetail(props) {
    ...
    // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
    const adventureName = _path.split('/').pop();

    return(
        ...
        <AEMResponsiveGrid 
            pagePath={`/content/wknd-app/us/en/home/adventure/${adventureName}`}
            itemPath="root/responsivegrid"/>
            
        <h2>Itinerary</h2>
        ...
    )
}
```

다음 `AdventureDetail.js` 파일 형식은 다음과 같습니다.

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## AEM에서 컨테이너 작성

사용 `<AEMResponsiveGrid...>` 제자리에 `pagePath` 렌더링되는 모험에 따라 동적으로 설정하면 컨텐츠를 작성하려고 합니다.

1. AEM 작성자에 로그인
1. 다음으로 이동 __사이트 > WKND 앱 > us > en__
1. __편집__ a __WKND 앱 홈 페이지__ 페이지
   + 로 이동합니다 __발리 서프 캠프__ 편집할 SPA 경로 지정
1. 선택 __미리 보기__ 오른쪽 상단의 모드 선택기에서
1. 을(를) 탭합니다. __발리 서프 캠프__ 해당 경로로 이동할 SPA 카드
1. 선택 __편집__ 모드 선택기에서
1. 을(를) 찾습니다 __레이아웃 컨테이너__ 편집 가능한 영역 바로 위 __일정__
1. 를 엽니다. __페이지 편집기의 사이드 바__, 을(를) 선택하고 을(를) 선택합니다. __구성 요소 보기__
1. 활성화된 구성 요소 중 일부를 __레이아웃 컨테이너__
   + 이미지
   + 텍스트
   + 제목

   홍보 마케팅 자료를 만들 수 있습니다. 다음과 같은 모습일 수 있습니다.

   ![발리 모험 세부 사항 작성](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. __미리 보기__ AEM 페이지 편집기의 변경 사항
1. 로컬에서 실행되는 WKND 앱 새로 고침 [http://localhost:3000](http://localhost:3000)로 이동합니다. __발리 서프 캠프__ 작성된 변경 사항을 보려는 경로

   ![원격 SPA 발리](./assets/spa-dynamic-routes/remote-spa-final.png)

매핑된 AEM 페이지가 없는 모험 세부 정보 경로로 이동할 때 해당 경로 인스턴스에 작성 기능이 없습니다. 이러한 페이지에서 작성할 수 있도록 하려면 __모험__ 페이지!

## 축하합니다!

축하합니다! SPA에서 동적 경로에 작성 기능을 추가했습니다.

+ AEM React 편집 가능한 구성 요소의 ResponsiveGrid 구성 요소를 동적 경로에 추가했습니다
+ SPA(Bali Surf Camp and Beervana in Portland)에서 두 개의 특정 루트 작성을 지원하는 AEM 페이지가 작성되었습니다.
+ 동적 발리 서프 캠프 경로에서 컨텐츠를 작성합니다!

이제 AEM SPA Editor를 사용하여 특정 편집 가능한 영역을 Remote SPA에 추가하는 방법의 첫 번째 단계 탐색을 완료했습니다!
