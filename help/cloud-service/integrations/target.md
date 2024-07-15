---
title: AEM Headless 및 Target 통합
description: Experience Platform Web SDK를 사용하여 AEM Headless와 Adobe Target을 통합하여 Headless 경험을 개인화하는 방법에 대해 알아봅니다.
version: Cloud Service
feature: Content Fragments, Integrations
topic: Personalization, Headless
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-09T00:00:00Z
jira: KT-12433
thumbnail: KT-12433.jpeg
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM as a Cloud Service Headless" before-title="false"
exl-id: be886c64-9b8e-498d-983c-75f32c34be4b
duration: 1549
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1618'
ht-degree: 0%

---

# AEM Headless 및 Target 통합

AEM 콘텐츠 조각을 Adobe Target으로 내보내고 Adobe Experience Platform Web SDK의 alloy.js를 사용하여 Headless 경험을 개인화하는 데 사용하여 AEM Headless를 Adobe Target과 통합하는 방법을 알아봅니다. [React WKND 앱](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/how-to/example-apps/react-app.html)을 사용하여 콘텐츠 조각 오퍼를 사용하는 개인화된 Target 활동을 경험에 추가하여 WKND 모험을 홍보하는 방법을 탐색합니다.

>[!VIDEO](https://video.tv.adobe.com/v/3416585/?quality=12&learn=on)

이 자습서에서는 AEM 및 Adobe Target 설정과 관련된 단계를 다룹니다.

1. AEM 작성자의 [Adobe Target에 대한 Adobe IMS 구성 만들기](#adobe-ims-configuration)
2. AEM 작성자의 [Adobe Target Cloud Service 만들기](#adobe-target-cloud-service)
3. AEM 작성자의 [AEM Assets 폴더에 Adobe Target Cloud Service 적용](#configure-asset-folders)
4. Adobe Admin Console의 [Adobe Target Cloud Service 권한](#permission)
5. AEM 작성자에서 Target으로 [콘텐츠 조각 내보내기](#export-content-fragments)
6. Adobe Target에서 [콘텐츠 조각 오퍼를 사용하여 활동 만들기](#activity)
7. Experience Platform에서 [Experience Platform 데이터스트림 만들기](#datastream-id)
8. Adobe Web SDK를 사용하여 [React 기반 AEM Headless 앱에 개인화를 통합](#code)합니다.

## Adobe IMS 구성{#adobe-ims-configuration}

AEM과 Adobe Target 간의 인증을 용이하게 하는 Adobe IMS 구성입니다.

Adobe IMS 구성을 만드는 방법에 대한 단계별 지침은 [설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integration-adobe-target-ims.html)를 검토하십시오.

>[!VIDEO](https://video.tv.adobe.com/v/3416495/?quality=12&learn=on)

## Adobe Target Cloud Service{#adobe-target-cloud-service}

Adobe Target Cloud Service은 컨텐츠 조각을 Adobe Target으로 쉽게 내보낼 수 있도록 AEM에 만들어집니다.

Adobe Target Cloud Service을 만드는 방법에 대한 단계별 지침은 [설명서](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html)를 검토하십시오.

>[!VIDEO](https://video.tv.adobe.com/v/3416499/?quality=12&learn=on)


## 에셋 폴더 구성{#configure-asset-folders}

컨텍스트 인식 구성으로 구성된 Adobe Target Cloud Service을 Adobe Target으로 내보내려면 콘텐츠 조각이 포함된 AEM Assets 폴더 계층 구조에 적용해야 합니다.

+++확장 을 통한 단계별 지침

1. DAM 관리자로 __AEM 작성자 서비스__&#x200B;에 로그인합니다.
1. __Assets > 파일__(으)로 이동하여 `/conf`이(가) 적용된 자산 폴더를 찾습니다.
1. 자산 폴더를 선택하고 맨 위의 작업 표시줄에서 __속성__&#x200B;을 선택합니다.
1. __Cloud Service__ 탭 선택
1. 클라우드 구성이 Adobe Target Cloud Service 구성을 포함하는 컨텍스트 인식 구성(`/conf`)으로 설정되어 있는지 확인하십시오.
1. __Cloud Service 구성__ 드롭다운에서 __Adobe Target__&#x200B;을(를) 선택합니다.
1. 오른쪽 상단에서 __저장 및 닫기__ 선택

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416504/?quality=12&learn=on)

## AEM Target 통합 권한{#permission}

콘텐츠 조각을 Adobe Target으로 내보내려면 developer.adobe.com 프로젝트로 표시되는 Adobe Target 통합에 Adobe Admin Console의 __Editor__ 제품 역할이 부여되어야 합니다.

+++확장 을 통한 단계별 지침

1. Adobe Admin Console에서 Adobe Target 제품을 관리할 수 있는 사용자로 Experience Cloud에 로그인합니다
1. [Adobe Admin Console](https://adminconsole.adobe.com) 열기
1. __제품__&#x200B;을 선택한 다음 __Adobe Target__&#x200B;을 엽니다.
1. __제품 프로필__ 탭에서 __*DefaultWorkspace*__&#x200B;을(를) 선택합니다
1. __API 자격 증명__ 탭을 선택합니다.
1. 이 목록에서 developer.adobe.com 앱을 찾아 __제품 역할__&#x200B;을(를) __편집기__(으)로 설정합니다.

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416505/?quality=12&learn=on)

## Target으로 콘텐츠 조각 내보내기{#export-content-fragments}

[구성된 AEM Assets 폴더 계층 구조](#apply-adobe-target-cloud-service-to-aem-assets-folders)에 있는 콘텐츠 조각은 콘텐츠 조각 오퍼로 Adobe Target에 내보낼 수 있습니다. Target의 특별한 JSON 오퍼 양식인 이러한 콘텐츠 조각 오퍼는 Target 활동에서 사용하여 Headless 앱에서 개인화된 경험을 제공할 수 있습니다.

+++확장 을 통한 단계별 지침

1. __AEM 작성자__&#x200B;에 DAM 사용자로 로그인
1. __Assets > 파일__(으)로 이동한 다음 &quot;Adobe Target 활성화&quot; 폴더 아래에서 JSON으로 Target으로 내보낼 콘텐츠 조각을 찾습니다
1. Adobe Target으로 내보낼 콘텐츠 조각 선택
1. 상단 작업 표시줄에서 __Adobe Target 오퍼로 내보내기__&#x200B;를 선택합니다.
   + 이 작업은 콘텐츠 조각의 전체 하이드레이션된 JSON 표현을 Adobe Target에 &quot;콘텐츠 조각 오퍼&quot;로 내보냅니다.
   + 완전히 하이드레이션된 JSON 표현은 AEM에서 검토할 수 있습니다
      + 콘텐츠 조각 선택
      + 사이드 패널 펼치기
      + 왼쪽 패널에서 __미리 보기__ 아이콘 선택
      + Adobe Target으로 내보내는 JSON 표현은 기본 보기에 표시됩니다
1. Adobe Target용 편집기 역할의 사용자로 [Adobe Experience Cloud](https://experience.adobe.com)에 로그인합니다.
1. [Experience Cloud](https://experience.adobe.com)에서 오른쪽 상단의 제품 전환기에서 __Target__&#x200B;을 선택하여 Adobe Target을 엽니다.
1. 오른쪽 상단의 __Workspace 전환기__&#x200B;에서 기본 Workspace이 선택되어 있는지 확인하십시오.
1. 위쪽 탐색에서 __오퍼__ 탭을 선택합니다.
1. __유형__ 드롭다운을 선택하고 __콘텐츠 조각__&#x200B;을 선택합니다.
1. AEM에서 내보낸 콘텐츠 조각이 목록에 표시되는지 확인
   + 오퍼 위로 마우스를 가져간 후 __보기__ 단추를 선택하세요.
   + __오퍼 정보__&#x200B;를 검토하고 AEM Author 서비스에서 바로 콘텐츠 조각을 여는 __AEM 딥링크__&#x200B;를 확인하세요.

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416506/?quality=12&learn=on)

## 콘텐츠 조각 오퍼를 사용한 Target 활동{#activity}

Adobe Target에서는 컨텐츠 조각 오퍼 JSON을 컨텐츠로 사용하는 활동을 만들 수 있으므로, AEM에서 만들고 관리하는 컨텐츠를 사용하여 headless 앱에서 개인화된 경험을 할 수 있습니다.

이 예제에서는 간단한 A/B 활동을 사용하지만 모든 Target 활동을 사용할 수 있습니다.

+++확장 을 통한 단계별 지침

1. 위쪽 탐색에서 __활동__ 탭을 선택합니다.
1. __+ 활동 만들기__&#x200B;를 선택한 다음 만들 활동 유형을 선택하십시오.
   + 이 예제는 간단한 __A/B 테스트__&#x200B;를 만들지만 콘텐츠 조각 오퍼는 모든 활동 유형을 지원할 수 있습니다.
1. __활동 만들기__ 마법사에서
   + __웹__ 선택
   + __경험 작성기 선택__&#x200B;에서 __양식__&#x200B;을 선택합니다.
   + __Workspace 선택__&#x200B;에서 __기본 Workspace__&#x200B;을 선택합니다.
   + __속성 선택__&#x200B;에서 활동을 사용할 수 있는 속성을 선택하거나 __속성 제한 없음__&#x200B;을 선택하여 모든 속성에서 사용할 수 있도록 합니다.
   + __다음__&#x200B;을(를) 선택하여 활동 만들기
1. 왼쪽 상단에서 __이름 바꾸기__&#x200B;를 선택하여 활동 이름을 변경합니다
   + 활동에 의미 있는 이름을 지정하십시오.
1. 초기 경험에서 활동에 대한 __위치 1__&#x200B;을(를) 타깃팅으로 설정합니다.
   + 이 예제에서는 사용자 지정 위치 `wknd-adventure-promo`을(를) 타깃팅합니다
1. __콘텐츠__&#x200B;에서 기본 콘텐츠를 선택하고 __콘텐츠 조각 변경__&#x200B;을 선택합니다.
1. 이 경험에 제공할 내보낸 콘텐츠 조각을 선택하고 __완료__&#x200B;를 선택합니다.
1. 컨텐츠 텍스트 영역에서 컨텐츠 조각 오퍼 JSON을 검토하십시오. 이는 컨텐츠 조각의 미리보기 작업을 통해 AEM Author 서비스에서 사용할 수 있는 것과 동일한 JSON입니다.
1. 왼쪽 레일에서 경험을 추가하고 제공할 다른 콘텐츠 조각 오퍼를 선택합니다
1. __다음__&#x200B;을(를) 선택하고 활동에 필요한 타깃팅 규칙을 구성합니다.
   + 이 예에서는 A/B 테스트를 수동 50/50 분할로 둡니다.
1. __다음__&#x200B;을(를) 선택하고 활동 설정을 완료합니다.
1. __저장 및 닫기__&#x200B;를 선택하고 의미 있는 이름을 지정하십시오.
1. Adobe Target의 활동에서 오른쪽 상단의 비활성/활성화/보관 드롭다운에서 __활성화__&#x200B;를 선택합니다.

`wknd-adventure-promo` 위치를 타깃팅하는 Adobe Target 활동을 이제 AEM Headless 앱에 통합하여 노출할 수 있습니다.

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416507/?quality=12&learn=on)

## Experience Platform 데이터 스트림 ID{#datastream-id}

AEM Headless 앱이 [Adobe Experience Platform Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html)를 사용하여 Adobe Target과 상호 작용하려면 [Adobe 데이터스트림](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/initial-configuration/configure-datastream.html) ID가 필요합니다.

+++확장 을 통한 단계별 지침

1. [Adobe Experience Cloud](https://experience.adobe.com/)(으)로 이동
1. __Experience Platform__ 열기
1. __데이터 수집 > 데이터스트림__&#x200B;을 선택하고 __새 데이터스트림__&#x200B;을 선택합니다.
1. 새 데이터 스트림 마법사에서 다음을 입력합니다.
   + 이름: `AEM Target integration`
   + 설명: `Datastream used by the Adobe Web SDK to serve personalized Content Fragments Offers.`
   + 이벤트 스키마: `Leave blank`
1. __저장__ 선택
1. __서비스 추가__ 선택
1. __서비스__&#x200B;에서 __Adobe Target__ 선택
   + 활성화됨: __예__
   + 속성 토큰: __비워 둠__
   + 대상 환경 ID: __비워 둠__
      + Target 환경은 __관리 > 호스트__&#x200B;의 Adobe Target에서 설정할 수 있습니다.
   + 대상 타사 ID 네임스페이스: __비워 둠__
1. __저장__ 선택
1. 오른쪽에서 [Web SDK Adobe](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html) 구성 호출에 사용할 __데이터 스트림 ID__&#x200B;을(를) 복사합니다.

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416500/?quality=12&learn=on)

## AEM Headless 앱에 개인화 추가{#code}

이 튜토리얼에서는 [Adobe Experience Platform Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html)를 통해 Adobe Target에서 콘텐츠 조각 오퍼를 사용하여 간단한 React 앱을 개인화하는 방법을 살펴봅니다. 이 접근 방식을 사용하여 모든 JavaScript 기반 웹 경험을 개인화할 수 있습니다.

Android™ 및 iOS 모바일 경험은 [Adobe의 Mobile SDK](https://developer.adobe.com/client-sdks/documentation/)를 사용하여 유사한 패턴에 따라 개인화할 수 있습니다.

### 사전 요구 사항

+ Node.js 14
+ Git
+ AEM as a Cloud Author 및 Publish 서비스에 설치된 [WKND 공유 2.1.4+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest)

### 설정

1. [Github.com](https://github.com/adobe/aem-guides-wknd-graphql)에서 샘플 React 앱의 소스 코드를 다운로드합니다.

   ```shell
   $ mkdir -p ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 즐겨찾는 IDE의 `~/Code/aem-guides-wknd-graphql/personalization-tutorial`에서 코드 베이스를 엽니다.
1. 앱이 `~/Code/aem-guides-wknd-graphql/personalization-tutorial/src/.env.development`에 연결할 AEM 서비스의 호스트를 업데이트합니다.

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

1. [Adobe Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/installing-the-sdk.html#option-3%3A-using-the-npm-package)를 NPM 패키지로 설치합니다.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install @adobe/alloy
   ```

   코드에서 Web SDK를 사용하여 활동 위치별로 콘텐츠 조각 오퍼 JSON을 가져올 수 있습니다.

   웹 SDK를 구성할 때 두 개의 ID가 필요합니다.

   + [데이터 스트림 ID](#datastream-id)인 `edgeConfigId`
   + `orgId` __Experience Cloud > 프로필 > 계정 정보 > 현재 조직 ID에서 찾을 수 있는 AEM as a Cloud Service/Target Adobe 조직 ID__

   Web SDK를 호출할 때 Adobe Target 활동 위치(예: `wknd-adventure-promo`)를 `decisionScopes` 배열의 값으로 설정해야 합니다.

   ```javascript
   import { createInstance } from "@adobe/alloy";
   const alloy = createInstance({ name: "alloy" });
   ...
   alloy("config", { ... });
   alloy("sendEvent", { ... });
   ```

### 구현

1. React 구성 요소 `AdobeTargetActivity.js`을(를) 만들어 Adobe Target 활동을 표시합니다.

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

   AdobeTargetActivity React 구성 요소는 다음과 같이 을 사용하여 호출됩니다.

   ```jsx
   <AdobeTargetActivity activityLocation={"wknd-adventure-promo"} OfferComponent={AdventurePromo}/>
   ```

1. React 구성 요소 `AdventurePromo.js`을(를) 만들어 JSON Adobe Target에서 제공하는 모험을 렌더링합니다.

   이 React 구성 요소는 어드벤처 콘텐츠 조각을 나타내는 전체 하이드레이션된 JSON을 가져와서 프로모션 방식으로 표시합니다. Adobe Target 콘텐츠 조각 오퍼에서 서비스되는 JSON을 표시하는 React 구성 요소는 Adobe Target으로 내보내는 콘텐츠 조각을 기반으로 필요에 따라 다양하고 복잡할 수 있습니다.

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

1. 모험 목록 위에 React 앱의 `Home.js`에 AdobeTargetActivity 구성 요소를 추가합니다.

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

1. React 앱이 실행되고 있지 않으면 `npm run start`을(를) 사용하여 다시 시작하십시오.

   서로 다른 두 브라우저에서 React 앱을 열어 A/B 테스트가 각 브라우저에 서로 다른 경험을 제공할 수 있도록 합니다. 두 브라우저 모두 동일한 어드벤처 오퍼를 표시하는 경우 다른 경험이 표시될 때까지 브라우저 중 하나를 닫거나 다시 열어 보십시오.

   아래 이미지는 Adobe Target의 논리를 기반으로 `wknd-adventure-promo` 활동에 대해 표시되는 두 개의 다른 콘텐츠 조각 오퍼를 보여 줍니다.

   ![경험 오퍼](./assets/target/offers-in-app.png)

## 축하합니다!

콘텐츠 조각을 Adobe Target으로 내보내도록 AEM as a Cloud Service을 구성하고, Adobe Target 활동에서 콘텐츠 조각 오퍼를 사용하고, AEM Headless 앱의 활동을 표면화하여 경험을 개인화했습니다.
