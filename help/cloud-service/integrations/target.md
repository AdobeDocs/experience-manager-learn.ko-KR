---
title: AEM 헤드리스 및 Target 개인화
description: 이 자습서에서는 AEM 컨텐츠 조각을 Adobe Target으로 내보낸 다음 Adobe 웹 SDK를 사용하여 헤드리스 경험을 개인화하는 데 사용하는 방법을 설명합니다.
version: Cloud Service
feature: Content Fragments, Integrations
topic: Personalization, Headless
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-09T00:00:00Z
jira: KT-12433
thumbnail: KT-12433.jpeg
source-git-commit: b3cc9c4fbd36cdf5be46e4546a174fea0c8da05c
workflow-type: tm+mt
source-wordcount: '1703'
ht-degree: 2%

---

# 컨텐츠 조각을 사용하여 AEM Headless 경험 개인화

>[!IMPORTANT]
>
> Adobe Target으로 Adobe Experience Manager 컨텐츠 조각 내보내기는 AEM as a Cloud Service에서 사용할 수 있습니다 [사전 릴리스 채널](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/release-notes/prerelease.html?lang=ko#new-features).



이 자습서에서는 AEM 컨텐츠 조각을 Adobe Target으로 내보낸 다음 Adobe 웹 SDK를 사용하여 헤드리스 경험을 개인화하는 데 사용하는 방법을 설명합니다. 다음 [React WKND App](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/how-to/example-apps/react-app.html) 는 컨텐츠 조각 오퍼를 사용하여 개인화된 Target 활동을 경험에 추가하여 WKND 어드벤처를 홍보하는 데 사용됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/3416585/?quality=12&learn=on)

이 자습서에서는 AEM 및 Adobe Target 설정과 관련된 단계를 설명합니다.

1. [Adobe Target용 Adobe IMS 구성 만들기](#adobe-ims-configuration) AEM 작성자
1. [Adobe Target Cloud Service 만들기](#adobe-target-cloud-service) AEM 작성자
1. [AEM Assets 폴더에 Adobe Target Cloud Service 적용](#configure-asset-folders) AEM 작성자
1. [Adobe Target Cloud Service 권한](#permission) Adobe Admin Console
1. [컨텐츠 조각 내보내기](#export-content-fragments) AEM 작성자에서 Target으로
1. [컨텐츠 조각 오퍼를 사용하여 활동 생성](#activity) Adobe Target
1. [Experience Platform 데이터 스트림 만들기](#datastream-id) Experience Platform
1. [개인화를 React 기반 AEM Headless 앱에 통합](#code) Adobe 웹 SDK 사용.

## Adobe IMS 구성{#adobe-ims-configuration}

AEM과 Adobe Target 간의 인증을 쉽게 해주는 Adobe IMS 구성입니다.

검토 [설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integration-adobe-target-ims.html) adobe IMS 구성을 만드는 방법에 대한 단계별 지침입니다.

>[!VIDEO](https://video.tv.adobe.com/v/3416495/?quality=12&learn=on)

## Adobe Target Cloud Service{#adobe-target-cloud-service}

컨텐츠 조각을 Adobe Target으로 쉽게 내보낼 수 있도록 AEM에 Adobe Target Cloud Service이 만들어집니다.

검토 [설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html) Adobe Target Cloud Service을 만드는 방법에 대한 단계별 지침입니다.

>[!VIDEO](https://video.tv.adobe.com/v/3416499/?quality=12&learn=on)


## 자산 폴더 구성{#configure-asset-folders}

컨텍스트 인식 구성으로 구성된 Adobe Target Cloud Service은 Adobe Target으로 내보낼 컨텐츠 조각이 포함된 AEM Assets 폴더 계층 구조에 적용되어야 합니다.

+++확장 을 통해 단계별 지침 제공

1. 에 로그인합니다. __AEM 작성자 서비스__ DAM 관리자로
1. 다음으로 이동 __자산 > 파일__&#x200B;에서 를 찾아서 `/conf` 적용
1. 자산 폴더를 선택하고 을(를) 선택합니다 __속성__ 맨 위 작업 표시줄에서
1. __클라우드 서비스__ 탭을 선택합니다
1. 클라우드 구성이 컨텍스트 인식 구성(`/conf`). Adobe Target Cloud Services 구성을 포함합니다.
1. 선택 __Adobe Target__ 에서 __Cloud Service 구성__ 드롭다운.
1. 선택 __저장 및 닫기__ 오른쪽 상단에서

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416504/?quality=12&learn=on)

## AEM Target 통합 권한{#permission}

developer.adobe.com 프로젝트로 표시되는 Adobe Target 통합에는 __편집자__ 컨텐츠 조각을 Adobe Target으로 내보내기 위한 Adobe Admin Console의 제품 역할.

+++확장 을 통해 단계별 지침 제공

1. Adobe Admin Console에서 Adobe Target 제품을 관리할 수 있는 Experience Cloud으로 로그인합니다
1. 를 엽니다. [Adobe Admin Console](https://adminconsole.adobe.com)
1. 선택 __제품__ 그런 다음 엽니다. __Adobe Target__
1. 설정 __제품 프로필__ 탭, 선택 __*DefaultWorkspace*__
1. 을(를) 선택합니다 __API 자격 증명__ 탭
1. 이 목록에서 developer.adobe.com 앱을 찾아서 설정합니다 __제품 역할__ to __편집자__

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416505/?quality=12&learn=on)

## 컨텐츠 조각을 Target으로 내보내기{#export-content-fragments}

아래에 있는 컨텐츠 조각 [구성된 AEM Assets 폴더 계층 구조](#apply-adobe-target-cloud-service-to-aem-assets-folders) Adobe Target에 컨텐츠 조각 오퍼로 내보낼 수 있습니다. Target의 특수 JSON 오퍼 양식인 이러한 컨텐츠 조각 오퍼는 헤드리스 앱에서 Target 활동에 사용하여 개인화된 경험을 제공할 수 있습니다.

+++확장 을 통해 단계별 지침 제공

1. 에 로그인합니다. __AEM 작성자__ DAM 사용자로
1. 다음으로 이동 __자산 > 파일__&#x200B;를 JSON으로 내보내 &quot;Adobe Target 활성화&quot; 폴더 아래에 Target으로 내보낼 컨텐츠 조각을 찾습니다
1. Adobe Target으로 내보낼 컨텐츠 조각 을 선택합니다
1. 선택 __Adobe Target 오퍼으로 내보내기__ 맨 위 작업 표시줄에서
   + 이 작업은 컨텐츠 조각의 완전히 하이드레이션된 JSON 표현을 Adobe Target에 &quot;컨텐츠 조각 오퍼&quot;로 내보냅니다
   + 완전히 하이드레이션된 JSON 표현은 AEM에서 검토할 수 있습니다
      + 컨텐츠 조각을 선택합니다
      + 사이드 패널을 확장합니다.
      + 선택 __미리 보기__ 왼쪽 패널의 아이콘
      + Adobe Target으로 내보낸 JSON 표현은 기본 보기에 표시됩니다
1. 에 로그인합니다. [Adobe Experience Cloud](https://experience.adobe.com) Adobe Target용 편집기 역할의 사용자 사용
1. 에서 [Experience Cloud](https://experience.adobe.com), 선택 __Target__ 오른쪽 상단에 있는 제품 전환기에서 Adobe Target을 엽니다.
1. 에서 기본 작업 공간 이 선택되어 있는지 확인합니다. __작업 공간 전환기__ 오른쪽 위에 있습니다.
1. 을(를) 선택합니다 __오퍼__ 위쪽 탐색의 탭
1. 을(를) 선택합니다 __유형__ 드롭다운 및 선택 __컨텐츠 조각__
1. AEM에서 내보낸 컨텐츠 조각이 목록에 표시되는지 확인합니다.
   + 오퍼 위로 마우스를 가져간 다음 을(를) 선택합니다 __보기__ 버튼
   + 를 검토합니다. __오퍼 정보__ 그리고 __AEM 딥 링크__ 에서 직접 AEM 작성자 서비스에서 컨텐츠 조각을 엽니다

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416506/?quality=12&learn=on)

## 컨텐츠 조각 오퍼를 사용하여 활동 Target{#activity}

Adobe Target에서 컨텐츠 조각 오퍼 JSON을 컨텐츠로 사용하는 활동을 만들어 AEM에서 만들고 관리되는 컨텐츠가 있는 헤드리스 앱에서 개인화된 경험을 허용할 수 있습니다.

이 예제에서는 간단한 A/B 활동을 사용하지만 모든 Target 활동을 사용할 수 있습니다.

+++확장 을 통해 단계별 지침 제공

1. 을(를) 선택합니다 __활동__ 위쪽 탐색의 탭
1. 선택 __+ 활동 만들기__, 그런 다음 만들 활동 유형을 선택합니다.
   + 이 예는 간단한 __A/B 테스트__ 그러나 컨텐츠 조각 오퍼는 모든 활동 유형을 지원할 수 있습니다
1. 에서 __활동 만들기__ 마법사
   + 선택 __웹__
   + in __경험 작성기 선택__, 선택 __양식__
   + in __작업 공간 선택__, 선택 __기본 작업 공간__
   + in __속성 선택__&#x200B;에서 활동을 사용할 수 있는 속성 을 선택하거나 을(를) 선택합니다 __속성 제한 없음__ 를 사용
   + 선택 __다음__ 활동을 만들려면
1. 을(를) 선택하여 활동 이름을 변경합니다 __이름 바꾸기__ 왼쪽 상단에서
   + 활동에 의미 있는 이름을 지정합니다
1. 초기 경험에서 __위치 1__ 타겟팅할 활동
   + 이 예에서는 라는 사용자 지정 위치를 타깃팅합니다 `wknd-adventure-promo`
1. 아래 __컨텐츠__ 기본 콘텐츠를 선택하고 을(를) 선택합니다 __컨텐츠 조각 변경__
1. 이 경험에 대해 제공할 내보낸 컨텐츠 조각을 선택하고 을(를) 선택합니다 __완료__
1. 컨텐츠 텍스트 영역에서 컨텐츠 조각 오퍼 JSON을 검토하십시오. 이 JSON은 컨텐츠 조각의 미리 보기 작업을 통해 AEM 작성 서비스에서 사용할 수 있는 것과 동일합니다.
1. 왼쪽 레일에서 경험을 추가하고, 제공할 다른 컨텐츠 조각 오퍼를 선택합니다
1. 선택 __다음__, 활동에 대해 필요에 따라 타깃팅 규칙을 구성합니다.
   + 이 예에서 A/B 테스트를 수동 50/50 분할으로 둡니다.
1. 선택 __다음__, 및 활동 설정을 완료합니다
1. 선택 __저장 및 닫기__ 의미 있는 이름을 알려주세요
1. Adobe Target의 활동에서 __활성화__ 오른쪽 상단의 비활성/활성화/보관 드롭다운에서 을 선택합니다.

타겟팅하는 Adobe Target 활동 `wknd-adventure-promo` 이제 AEM Headless 앱에서 위치를 통합하여 노출할 수 있습니다.

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416507/?quality=12&learn=on)

## Experience Platform 데이터 스트림 ID{#datastream-id}

An [Adobe Experience Platform 데이터 스트림](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/initial-configuration/configure-datastream.html) AEM Headless 앱이 [웹 SDK Adobe](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html).

+++확장 을 통해 단계별 지침 제공

1. 다음으로 이동 [Adobe Experience Cloud](https://experience.adobe.com/)
1. 열기 __Experience Platform__
1. 선택 __데이터 수집 > 데이터 스트림__ 을(를) 선택합니다. __새 데이터 스트림__
1. 새 데이터 스트림 마법사에서 다음을 입력합니다.
   + 이름: `AEM Target integration`
   + 설명: `Datastream used by the Adobe Web SDK to serve personalized Content Fragments Offers.`
   + 이벤트 스키마: `Leave blank`
1. __저장__&#x200B;을 선택합니다
1. 선택 __서비스 추가__
1. in __서비스__ 선택 __Adobe Target__
   + 활성화됨: __예__
   + 속성 토큰: __비워 둡니다.__
   + Target 환경 ID: __비워 둡니다.__
      + Target 환경은 Adobe Target의 __관리 > 호스트__.
   + Target 타사 ID 네임스페이스: __비워 둡니다.__
1. __저장__&#x200B;을 선택합니다
1. 오른쪽에서 을 복사합니다. __데이터 스트림 ID__ 에서 사용 [웹 SDK Adobe](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html) 구성 호출.

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416500/?quality=12&learn=on)

## AEM Headless 앱에 개인화 추가{#code}

이 자습서에서는 를 통해 Adobe Target에서 컨텐츠 조각 오퍼를 사용하여 간단한 React 앱을 개인화하는 방법을 설명합니다 [Adobe Experience Platform Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). 이 접근 방식은 JavaScript 기반 웹 경험을 개인화하는 데 사용할 수 있습니다.

Android™ 및 iOS 모바일 경험은 [Adobe의 Mobile SDK](https://developer.adobe.com/client-sdks/documentation/).

### 사전 요구 사항

+ Node.js 14
+ Git
+ [WKND 공유 2.1.4+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) AEM as a Cloud Author 및 Publish 서비스에 설치됨

### 설정

1. 다음에서 샘플 React 앱의 소스 코드를 다운로드합니다. [Github.com](https://github.com/adobe/aem-guides-wknd-graphql)

   ```shell
   $ mkdir -p ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 에서 코드 베이스를 엽니다. `~/Code/aem-guides-wknd-graphql/personalization-tutorial` 좋아하는 IDE에서
1. 앱을 연결할 AEM 서비스의 호스트를 업데이트합니다 `~/Code/aem-guides-wknd-graphql/personalization-tutorial/src/.env.development`

   ```
   ...
   REACT_APP_HOST_URI=https://publish-p1234-e5678.adobeaemcloud.com/
   ...
   ```

1. 앱을 실행하고 구성된 AEM 서비스에 연결되는지 확인합니다. 명령줄에서 다음을 실행합니다.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install
   $ npm run start
   ```

1. 설치 [웹 SDK Adobe](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/installing-the-sdk.html#option-3%3A-using-the-npm-package) 를 NPM 패키지로 사용합니다.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install @adobe/alloy
   ```

   웹 SDK는 코드에서 활동 위치별로 컨텐츠 조각 오퍼 JSON을 가져오는 데 사용할 수 있습니다.

   웹 SDK를 구성할 때 두 가지 ID가 필요합니다.

   + `edgeConfigId` 다음 중 [데이터 스트림 ID](#datastream-id)
   + `orgId` 다음 위치에서 찾을 수 있는 AEM as a Cloud Service/Target Adobe 조직 ID __Experience Cloud > 프로필 > 계정 정보 > 현재 조직 ID__

   웹 SDK를 호출할 때 Adobe Target 활동 위치(이 예제에서는 `wknd-adventure-promo`)은 의 값으로 설정되어 있어야 합니다. `decisionScopes` 배열입니다.

   ```javascript
   import { createInstance } from "@adobe/alloy";
   const alloy = createInstance({ name: "alloy" });
   ...
   alloy("config", { ... });
   alloy("sendEvent", { ... });
   ```

### 구현

1. React 구성 요소 만들기 `AdobeTargetActivity.js` Adobe Target 활동을 표시할 수 있습니다.

   __src/components/AdobeTargetActivity.js__

   ```javascript
   import React, { useEffect } from 'react';
   import { createInstance } from '@adobe/alloy';
   
   const alloy = createInstance({ name: 'alloy' });
   
   alloy('configure', { 
     'edgeConfigId': 'e3db252d-44d0-4a0b-8901-aac22dbc88dc', // AEP Datastream ID
     'orgId':'7ABB3E6A5A7491460A495D61@AdobeOrg',
     'debugEnabled': true,
   });
   
   export default function AdobeTargetActivity({ activityLocation, OfferComponent }) { 
     const [offer, setOffer] = React.useState();
   
     useEffect(() => {
       async function sendAlloyEvent() {
         // Get the activity offer from Adobe Target
         const result = await alloy('sendEvent', {
           // decisionScopes is set to an array containing the Adobe Target activity location
           'decisionScopes': [activityLocation],
         });
   
         if (result.propositions?.length > 0) {
           // Find the first proposition for the active activity location
           var proposition = result.propositions?.filter((proposition) => { return proposition.scope === activityLocation; })[0];
   
           // Get the Content Fragment Offer JSON from the Adobe Target response
           const contentFragmentOffer = proposition?.items[0]?.data?.content || { status: 'error', message: 'Personalized content unavailable'};
   
           if (contentFragmentOffer?.data) {
             // Content Fragment Offers represent a single Content Fragment, hydrated by
             // the byPath GraphQL query, we must traverse the JSON object to retrieve the 
             // Content Fragment JSON representation
             const byPath = Object.keys(contentFragmentOffer.data)[0];
             const item = contentFragmentOffer.data[byPath]?.item;
   
             if (item) {
               // Set the offer to the React state so it can be rendered
               setOffer(item);
   
               // Record the Content Fragment Offer as displayed for Adobe Target Activity reporting
               // If this request is omitted, the Target Activity's Reports will be blank
               alloy("sendEvent", {
                   xdm: {
                       eventType: "decisioning.propositionDisplay",
                       _experience: {
                           decisioning: {
                               propositions: [proposition]
                           }
                       }
                   }
               });          
             }
           }
         }
       };
   
       sendAlloyEvent();
   
     }, [activityLocation, OfferComponent]);
   
     if (!offer) {
       // Adobe Target offer initializing; we render a blank component (which has a fixed height) to prevent a layout shift
       return (<OfferComponent></OfferComponent>);
     } else if (offer.status === 'error') {
       // If Personalized content could not be retrieved either show nothing, or optionally default content.
       console.error(offer.message);
       return (<></>);
     }
   
     console.log('Activity Location', activityLocation);
     console.log('Content Fragment Offer', offer);
   
     // Render the React component with the offer's JSON
     return (<OfferComponent content={offer} />);
   };
   ```

   Adobe TargetActivity React 구성 요소는 다음과 같이 를 사용하여 호출됩니다.

   ```jsx
   <AdobeTargetActivity activityLocation={"wknd-adventure-promo"} OfferComponent={AdventurePromo}/>
   ```

1. React 구성 요소 만들기 `AdventurePromo.js` dummytext에 대해 JSON Adobe Target 제공 서비스를 렌더링합니다.

   이 React 구성 요소는 모험 컨텐츠 조각을 나타내는 완전히 하이드레이션된 JSON을 가져와 홍보 방식으로 표시합니다. Adobe Target 컨텐츠 조각 오퍼에서 서비스되는 JSON을 표시하는 React 구성 요소는 Adobe Target으로 내보낸 컨텐츠 조각을 기반으로 필요에 따라 다양하고 복잡할 수 있습니다.

   __src/components/AdventurePromo.js__

   ```javascript
   import React from 'react';
   
   import './AdventurePromo.scss';
   
   /**
   * @param {*} content is the fully hydrated JSON data for a WKND Adventure Content Fragment
   * @returns the Adventure Promo component
   */
   export default function AdventurePromo({ content }) {
       if (!content) {
           // If content is still loading, then display an empty promote to prevent layout shift when Target loads the data
           return (<div className="adventure-promo"></div>)
       }
   
       const title = content.title;
       const description = content?.description?.plaintext;
       const image = content.primaryImage?._publishUrl;
   
       return (
           <div className="adventure-promo">
               <div className="adventure-promo-text-wrapper">
                   <h3 className="adventure-promo-eyebrow">Promoted adventure</h3>
                   <h2 className="adventure-promo-title">{title}</h2>
                   <p className="adventure-promo-description">{description}</p>
               </div>
               <div className="adventure-promo-image-wrapper">
                   <img className="adventure-promo-image" src={image} alt={title} />
               </div>
           </div>
       )
   }
   ```

   __src/components/AdventurePromo.scss__

   ```css
   .adventure-promo {
       display: flex;
       margin: 3rem 0;
       height: 400px;
   }
   
   .adventure-promo-text-wrapper {
       background-color: #ffea00;
       color: black;
       flex-grow: 1;
       padding: 3rem 2rem;
       width: 55%;
   }
   
   .adventure-promo-eyebrow {
       font-family: Source Sans Pro,Helvetica Neue,Helvetica,Arial,sans-serif;
       font-weight: 700;
       font-size: 1rem;
       margin: 0;
       text-transform: uppercase;
   }
   
   .adventure-promo-description {
       line-height: 1.75rem;
   }
   
   .adventure-promo-image-wrapper {
       height: 400px;
       width: 45%;
   }
   
   .adventure-promo-image {
       height: 100%;
       object-fit: cover;
       object-position: center center;
       width: 100%;
   }
   ```

   이 React 구성 요소는 다음과 같이 호출됩니다.

   ```jsx
   <AdventurePromo adventure={adventureJSON}/>
   ```

1. React 앱의 AdobeTargetActivity 구성 요소를 추가합니다 `Home.js` 모험목록 위에.

   __src/components/Home.js__

   ```javascript
   import AdventurePromo from './AdventurePromo';
   import AdobeTargetActivity from './AdobeTargetActivity';
   ... 
   export default function Home() {
       ...
       return(
           <div className="Home">
   
             <AdobeTargetActivity activityLocation={"wknd-adventure-promo"} OfferComponent={AdventurePromo}/>
   
             <h2>Current Adventures</h2>
             ...
       )
   }
   ```

1. React 앱이 실행되고 있지 않으면 을 다시 시작합니다 `npm run start`.

   A/B 테스트가 각 브라우저에 서로 다른 경험을 제공할 수 있도록 두 개의 다른 브라우저에서 React 앱을 엽니다. 두 브라우저 모두 동일한 모험 오퍼를 표시하는 경우, 다른 경험이 표시될 때까지 브라우저 중 하나를 닫기/다시 열어 보십시오.

   아래 이미지는 다음에 대해 표시되는 두 개의 다른 컨텐츠 조각 오퍼를 보여줍니다. `wknd-adventure-promo` Adobe Target의 논리를 기반으로 한 활동.

   ![경험 오퍼](./assets/target/offers-in-app.png)

## 축하합니다!

이제 컨텐츠 조각을 Adobe Target으로 내보내도록 AEM as a Cloud Service을 구성하고, Adobe Target 활동에서 컨텐츠 조각 오퍼를 사용하며, AEM Headless 앱에서 해당 활동을 표시하여 경험을 개인화했습니다.
