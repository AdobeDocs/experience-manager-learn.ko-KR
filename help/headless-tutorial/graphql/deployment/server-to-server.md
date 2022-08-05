---
title: AEM Headless 서버 간 배포
description: 서버 간 AEM Headless 배포에 대한 배포 고려 사항에 대해 알아봅니다.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10798
thumbnail: kt-10798.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 1%

---


# AEM Headless 서버 간 배포

AEM 헤드리스 서버 간 배포에는 헤드리스 방식으로 AEM에서 컨텐츠를 사용하고 상호 작용하는 서버측 애플리케이션이나 프로세스가 포함되어 있습니다.

AEM Headless API에 대한 HTTP 연결이 브라우저 컨텍스트에서 시작되지 않으므로 서버 간 배포에서는 최소 구성이 필요합니다.

## 배포 구성

서버 간 앱 배포를 위해서는 다음 배포 구성을 적소에 배치해야 합니다.

| 서버 간 앱 연결 대상 | AEM Author | AEM 게시 | AEM 미리 보기 |
|---------------------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [디스패처 필터](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| CORS(원본 간 리소스 공유) | ✘ | ✘ | ✘ |
| [AEM 호스트](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## 인증 요구 사항

AEM GraphQL API에 대한 인증된 요청은 일반적으로 서버 간 앱 컨텍스트에서 발생하며, 이러한 요청은 다른 앱 유형(예: [단일 페이지 앱](./spa.md), [mobile](./mobile.md), 또는 [웹 구성 요소](./web-component.md)인 경우 일반적으로 자격 증명 의 보안을 유지하는 것이 어려우므로 권한 부여를 사용합니다.

AEM as a Cloud Service에 대한 요청을 인증하는 경우 [서비스 자격 증명 기반 토큰 인증](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html). AEM as a Cloud Service에 요청 인증에 대한 자세한 내용은 [토큰 기반 인증 자습서](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). 이 자습서에서는 다음을 사용하여 토큰 기반 인증을 탐색합니다 [AEM Assets HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets.html) 그러나 AEM Headless GraphQL API와 상호 작용하는 앱에 동일한 개념과 접근 방식이 적용됩니다.

## 서버 간 앱 예제

Adobe은 Node.js에서 코딩된 서버 간 앱의 예를 제공합니다.

<div class="columns is-multiline">
    <!-- Server-to-server app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Server-to-server app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/server-to-server-app.md" title="서버 간 앱" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/server-to-server-app/server-to-server-card.png" alt="서버 간 앱">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/server-to-server-app.md" title="서버 간 앱">서버 간 앱</a></p>
                   <p class="is-size-6">AEM Headless GraphQL API의 컨텐츠를 사용하는 Node.js에 작성된 서버 간 앱의 예입니다.</p>
                   <a href="../example-apps/server-to-server-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">보기 예</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>