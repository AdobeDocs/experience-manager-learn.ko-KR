---
title: AEM GraphQL용 SPA 배포
description: SPA(단일 페이지 앱) AEM Headless 배포에 대한 배포 고려 사항에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
exl-id: 3fe175f7-6213-439a-a02c-af3f82b6e3b7
duration: 136
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '655'
ht-degree: 1%

---

# AEM 헤드리스 SPA 배포

AEM Headless SPA(단일 페이지 앱) 배포에는 React 또는 Vue와 같은 프레임워크를 사용하여 빌드된 JavaScript 기반 애플리케이션이 포함되며, Headless 방식으로 AEM의 콘텐츠를 소비하고 상호 작용합니다.

Headless 방식으로 AEM과 상호 작용하는 SPA를 배포하려면 SPA를 호스팅하고 웹 브라우저를 통해 액세스할 수 있도록 해야 합니다.

## SPA 호스팅

SPA는 기본 웹 리소스 **HTML, CSS 및 JavaScript**&#x200B;의 컬렉션으로 구성됩니다. 이러한 리소스는 _build_ 프로세스(예: `npm run build`) 중에 생성되며 최종 사용자가 사용할 수 있도록 호스트에 배포됩니다.

조직의 요구 사항에 따라 다양한 **호스팅** 옵션이 있습니다.

1. **Azure** 또는 **AWS**&#x200B;와 같은 **클라우드 공급자**.

2. 회사 **데이터 센터**&#x200B;에서 **온-프레미스** 호스팅

3. **AWS Amplify**, **Azure App Service**, **Netlify**, **Heroku**, **Vercel** 등과 같은 **프런트 엔드 호스팅 플랫폼**

## 배포 구성

AEM Headless와 상호 작용하는 SPA를 호스팅할 때 주로 고려해야 할 사항은 SPA가 AEM의 도메인(또는 호스트)을 통해 액세스되는지 또는 다른 도메인에서 액세스되는지 여부입니다.  그 이유는 SPA가 웹 브라우저에서 실행되는 웹 애플리케이션이기 때문에 웹 브라우저 보안 정책의 적용을 받기 때문입니다.

### 공유 도메인

SPA와 AEM은 동일한 도메인에서 최종 사용자가 둘 다 액세스할 때 도메인을 공유합니다. 예:

+ AEM은 `https://wknd.site/`을(를) 통해 액세스됩니다.
+ SPA는 `https://wknd.site/spa`을(를) 통해 액세스됩니다.

AEM과 SPA는 동일한 도메인에서 액세스되므로 웹 브라우저를 사용하면 SPA에서 CORS 없이도 AEM Headless 엔드포인트에 대한 XHR을 만들 수 있으며 HTTP 쿠키(예: AEM의 `login-token` 쿠키)를 공유할 수 있습니다.

SPA 및 AEM 트래픽이 공유 도메인에서 라우팅되는 방법은 여러 원본을 사용하는 CDN, 역방향 프록시가 있는 HTTP 서버, AEM에서 SPA를 직접 호스팅하는 방법 등에 따라 다릅니다.

다음은 AEM과 동일한 도메인에서 호스팅되는 경우 SPA 프로덕션 배포에 필요한 배포 구성입니다.

| SPA가 SPA에 → | AEM Author | AEM 게시 | AEM 미리보기 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher 필터](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| CORS(원본 간 리소스 공유) | ✘ | ✘ | ✘ |
| AEM 호스트 | ✘ | ✘ | ✘ |

### 다른 도메인

SPA와 AEM에는 다른 도메인의 최종 사용자가 액세스할 때 다른 도메인이 있습니다. 예:

+ AEM은 `https://wknd.site/`을(를) 통해 액세스됩니다.
+ SPA는 `https://wknd-app.site/`을(를) 통해 액세스됩니다.

AEM과 SPA는 서로 다른 도메인에서 액세스하므로 웹 브라우저는 [CORS(원본 간 리소스 공유)](./configurations/cors.md)와 같은 보안 정책을 시행하고 HTTP 쿠키(AEM의 `login-token` 쿠키 등) 공유를 차단합니다.

다음은 AEM이 아닌 다른 도메인에서 호스팅되는 경우 SPA 프로덕션 배포에 필요한 배포 구성입니다.

| SPA가 SPA에 → | AEM Author | AEM 게시 | AEM 미리보기 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher 필터](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [CORS(원본 간 리소스 공유)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [AEM 호스트](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

#### 다른 도메인에서의 SPA 배포 예

이 예에서 SPA는 Netlify 도메인(`https://main--sparkly-marzipan-b20bf8.netlify.app/`)에 배포되고 SPA는 AEM 게시 도메인(`https://publish-p65804-e666805.adobeaemcloud.com`)의 AEM GraphQL API를 사용합니다. 아래 스크린샷은 CORS 요구 사항을 강조합니다.

1. SPA는 Netlify 도메인에서 제공되지만 다른 도메인의 AEM GraphQL API에 대해 XHR을 호출합니다. 이 사이트 간 요청을 사용하려면 Netlify 도메인의 콘텐츠 액세스를 허용하도록 AEM에 [CORS](./configurations/cors.md)을(를) 설정해야 합니다.

   SPA 및 AEM 호스트 ![&#128279;](assets/spa/cors-requirement.png)에서 제공된 SPA 요청

2. AEM GraphQL API에 대한 XHR 요청을 검사하는 동안 `Access-Control-Allow-Origin`이(가) 있으며, 이는 AEM이 이 Netlify 도메인의 콘텐츠 액세스를 요청할 수 있음을 웹 브라우저에 나타냅니다.

   AEM [CORS](./configurations/cors.md)이(가) 없거나 Netlify 도메인을 포함하지 않는 경우 웹 브라우저에서 XHR 요청을 실패하고 CORS 오류를 보고합니다.

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
               <p class="is-size-6">AEM Headless GraphQL API의 콘텐츠를 사용하는 React로 작성된 단일 페이지 앱의 예입니다.</p>
               <a href="../example-apps/react-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">예제 보기</span>
               </a>
           </div>
       </div>
   </div>
</div>
<!-- Next.js app -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Next.js app" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="../example-apps/next-js.md" title="Next.js 앱" tabindex="-1">
                   <img class="is-bordered-r-small" src="../example-apps/assets/next-js/next-js-card.png" alt="Next.js 앱">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/next-js.md" title="Next.js 앱">Next.js 앱</a></p>
               <p class="is-size-6">AEM Headless GraphQL API의 콘텐츠를 사용하는 Next.js로 작성된 단일 페이지 앱의 예입니다.</p>
               <a href="../example-apps/next-js.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">예제 보기</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>
