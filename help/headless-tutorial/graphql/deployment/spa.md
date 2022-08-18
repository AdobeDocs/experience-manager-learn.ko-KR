---
title: AEM GraphQL용 SPA 배포
description: 단일 페이지 앱(SPA) AEM Headless 배포에 대한 배포 고려 사항에 대해 알아봅니다.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
source-git-commit: 34fbb22916cf8a8df0e3240835c71e0979fd11bd
workflow-type: tm+mt
source-wordcount: '612'
ht-degree: 1%

---


# AEM Headless SPA 배포


AEM 헤드리스 단일 페이지 앱(SPA) 배포에는 헤드리스 방식으로 AEM에서 컨텐츠를 소비하고 상호 작용하는 React 또는 Value와 같은 프레임워크를 사용하여 작성된 JavaScript 기반 애플리케이션이 포함됩니다.

헤드리스 방식으로 AEM을 상호 작용하는 SPA을 배포하려면 SPA을 호스팅하고 웹 브라우저를 통해 액세스할 수 있어야 합니다.

## SPA 호스팅

SPA은 기본 웹 리소스 모음으로 구성됩니다. **HTML, CSS 및 JavaScript**. 이러한 리소스는 _빌드_ 프로세스(예: `npm run build`) 및에 배포되어 최종 사용자가 사용할 수 있습니다.

여러 가지가 있습니다 **호스팅** 조직의 요구 사항에 따른 옵션:

1. **클라우드 공급자** 예 **Azure** 또는 **AWS**.

2. **온-프레미스** 기업의 호스팅 **데이터 센터**

3. **프런트엔드 호스팅 플랫폼** 예 **AWS 확대**, **Azure 앱 서비스**, **Netlify**, **헤로쿠**, **Vercel**&#x200B;등

## 배포 구성

AEM 헤드리스와 상호 작용하는 SPA을 호스팅할 때 주로 고려할 사항은 SPA이 AEM 도메인(또는 호스트)을 통해 액세스되거나 다른 도메인에서 액세스되는 경우입니다.  이유는 SPA이 웹 브라우저에서 실행되는 웹 응용 프로그램이므로 웹 브라우저 보안 정책을 따릅니다.

### 공유 도메인

SPA 및 AEM은 둘 다 동일한 도메인에서 최종 사용자가 액세스할 때 도메인을 공유합니다. 예:

+ AEM은 다음 링크를 통해 액세스할 수 있습니다. `https://wknd.site/`
+ SPA은 `https://wknd.site/spa`

동일한 도메인에서 AEM과 SPA이 모두 액세스되므로 웹 브라우저에서는 SPA에서 CORS가 필요 없이 AEM 헤드리스 종단점으로 XHR을 설정하고 AEM과 같은 HTTP 쿠키(예:)를 공유할 수 있도록 합니다 `login-token` 쿠키 참조).

공유 도메인에서 SPA 및 AEM 트래픽이 라우팅되는 방법은 사용자에 따라 다릅니다. 여러 원본 CDN과 역방향 프록시가 있는 HTTP 서버, AEM에서 직접 SPA을 호스팅하는 등.

다음은 AEM과 동일한 도메인에 호스팅될 때 SPA 프로덕션 배포에 필요한 배포 구성입니다.

| SPA에 연결 | AEM Author | AEM 게시 | AEM 미리 보기 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [디스패처 필터](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| CORS(원본 간 리소스 공유) | ✘ | ✘ | ✘ |
| AEM 호스트 | ✘ | ✘ | ✘ |

### 다른 도메인

SPA과 AEM은 다른 도메인의 최종 사용자가 액세스할 때 서로 다른 도메인이 있습니다. 예:

+ AEM은 다음 링크를 통해 액세스할 수 있습니다. `https://wknd.site/`
+ SPA은 `https://wknd-app.site/`

AEM과 SPA이 다른 도메인에서 액세스되므로 웹 브라우저는 다음과 같은 보안 정책을 적용합니다 [CORS(원본 간 리소스 공유)](./configurations/cors.md), 및 HTTP 쿠키 공유 방지(예: AEM `login-token` 쿠키 참조).

다음은 AEM이 아닌 다른 도메인에서 호스팅될 때 SPA 프로덕션 배포에 필요한 배포 구성입니다.

| SPA에 연결 | AEM 작성자 | AEM 게시 | AEM 미리 보기 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [디스패처 필터](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [CORS(원본 간 리소스 공유)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [AEM 호스트](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

#### 다른 도메인의 SPA 배포 예

이 예에서는 SPA이 Netlify 도메인(`https://main--sparkly-marzipan-b20bf8.netlify.app/`) 및 SPA은 AEM 게시 도메인( )에서 AEM GraphQL API를 사용합니다`https://publish-p65804-e666805.adobeaemcloud.com`). 아래 스크린샷에서는 CORS 요구 사항을 강조 표시합니다.

1. SPA은 Netlify 도메인에서 제공되지만 다른 도메인의 AEM GraphQL API에 XHR을 호출합니다. 이 사이트 간 요청에는 다음이 필요합니다 [CORS](./configurations/cors.md) AEM에서 설정하여 Netlify 도메인의 요청에서 해당 컨텐츠에 액세스할 수 있도록 합니다.

   ![SPA 및 AEM 호스트에서 제공된 SPA 요청 ](assets/spa/cors-requirement.png)

2. AEM GraphQL API에 대한 XHR 요청 검사, `Access-Control-Allow-Origin` 이 은(는) 웹 브라우저에 이 Netlify 도메인의 요청이 해당 컨텐츠에 액세스할 수 있도록 허용함을 나타냅니다.

   AEM이 [CORS](./configurations/cors.md) 가 없거나 Netlify 도메인을 포함하지 않은 경우 웹 브라우저에서 XHR 요청이 실패하고 CORS 오류가 보고됩니다.

   ![CORS 응답 헤더 AEM GraphQL API](assets/spa/cors-response-headers.png)

## 단일 페이지 앱 예

Adobe은 React로 코딩된 단일 페이지 앱의 예를 제공합니다.

<div class="columns is-multiline">
<!-- React app -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="React app" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="../example-apps/react-app.md" title="React 앱" tabindex="-1">
                   <img class="is-bordered-r-small" src="../example-apps/assets/react-app/react-app-card.png" alt="React 앱">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/react-app.md" title="React 앱">React 앱</a></p>
               <p class="is-size-6">AEM Headless GraphQL API의 컨텐츠를 사용하는 React로 작성된 단일 페이지 앱의 예입니다.</p>
               <a href="../example-apps/react-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">보기 예</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>
