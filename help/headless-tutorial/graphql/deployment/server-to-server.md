---
title: AEM Headless 서버 간 배포
description: 서버 간 AEM Headless 배포를 위한 배포 고려 사항에 대해 알아봅니다.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10798
thumbnail: kt-10798.jpg
exl-id: d4ae08d9-dc43-4414-ab75-26853186a301
duration: 48
source-git-commit: b607ea10e0eed73b70751b1dd76266a4812d5280
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 1%

---

# AEM Headless 서버 간 배포

AEM Headless 서버 간 배포에는 AEM의 콘텐츠를 Headless 방식으로 사용하고 상호 작용하는 서버측 애플리케이션 또는 프로세스가 포함됩니다.

브라우저 컨텍스트에서 AEM Headless API에 대한 HTTP 연결이 시작되지 않으므로 서버 간 배포는 최소 구성이 필요합니다.

## 배포 구성

서버 간 앱 배포를 위해서는 다음 배포 구성이 적용되어야 합니다.

| 서버 간 앱을 서버에 → | AEM Author | AEM 게시 | AEM 미리 보기 |
|---------------------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher 필터](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| CORS(원본 간 리소스 공유) | ✘ | ✘ | ✘ |
| [AEM 호스트](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## 인증 요구 사항

[단일 페이지 앱](./spa.md), [모바일](./mobile.md) 또는 [웹 구성 요소](./web-component.md)와 같은 다른 앱 유형은 일반적으로 자격 증명 보안을 유지하기 어려우므로 권한 부여를 사용하기 때문에 AEM GraphQL API에 대한 승인된 요청은 일반적으로 서버 간 앱의 컨텍스트에서 발생합니다.

AEM as a Cloud Service에 대한 요청을 승인할 때 [서비스 자격 증명 기반 토큰 인증](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html)을 사용하십시오. AEM as a Cloud Service에 대한 요청 인증에 대한 자세한 내용은 [토큰 기반 인증 자습서](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)를 검토하세요. 자습서에서는 [AEM Assets HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets.html)를 사용하여 토큰 기반 인증을 살펴보지만 AEM Headless GraphQL API와 상호 작용하는 앱에도 동일한 개념과 접근 방식을 적용할 수 있습니다.

## 예제 서버 간 앱

Adobe은 Node.js로 코딩된 서버 간 앱의 예를 제공합니다.

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
                   <p class="is-size-6">AEM Headless GraphQL API의 콘텐츠를 사용하는 Node.js로 작성된 예제 서버 간 앱입니다.</p>
                   <a href="../example-apps/server-to-server-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">예제 보기</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
