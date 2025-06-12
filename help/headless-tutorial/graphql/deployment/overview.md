---
title: AEM Headless 배포
description: AEM Headless 앱의 다양한 배포 고려 사항에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10794
thumbnail: kt-10794.jpg
last-substantial-update: 2022-08-26T00:00:00Z
exl-id: 6de58ca0-9444-4272-9487-15a9e3c89231
duration: 59
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '315'
ht-degree: 100%

---

# AEM Headless 배포

AEM Headless 클라이언트 배포에는 AEM 호스팅 SPA, 외부 SPA, 웹 사이트, 모바일 앱, 서버 간 프로세스까지 다양한 형태가 있습니다.

클라이언트와 배포 방식에 따라 AEM Headless 배포에는 서로 다른 고려 사항이 있습니다.

## AEM 서비스 아키텍처

배포 고려 사항을 살펴보기 전에 AEM의 논리적 아키텍처와 AEM as a Cloud Service의 서비스 계층 구분 및 역할을 이해하는 것이 중요합니다. AEM as a Cloud Service는 두 개의 논리적 서비스로 구성됩니다.

+ __AEM 작성자 인스턴스__&#x200B;는 팀이 콘텐츠 조각 및 기타 자산을 만들고, 협업하고, 게시할 수 있는 서비스입니다.
+ __AEM 게시__&#x200B;는 게시된 콘텐츠 조각 및 기타 자산이 일반 사용자용으로 복제되어 제공되는 서비스입니다.
+ __AEM 미리보기__&#x200B;는 AEM 게시와 유사한 방식으로 작동하지만 콘텐츠가 미리보기 또는 검토 목적으로 게시되는 서비스입니다. AEM 미리보기는 내부 대상자를 대상으로 하며, 일반적인 콘텐츠 게재를 위한 것이 아닙니다. AEM 미리보기의 사용은 원하는 워크플로에 따라 선택 가능합니다.

![AEM 서비스 아키텍처](./assets/overview/aem-service-architecture.png)

전형적인 AEM as a Cloud Service Headless 배포 아키텍처_

프로덕션 환경에서 작동하는 AEM Headless 클라이언트는 일반적으로 승인되고 게시된 콘텐츠를 포함하는 AEM 게시와 상호 작용합니다. AEM 작성자 인스턴스와 상호 작용하는 클라이언트는 특별한 주의가 필요합니다. AEM 작성자 인스턴스는 기본적으로 보안이 적용되어 있어 모든 요청에 대해 인증이 필요하며, 진행 중이거나 승인되지 않은 콘텐츠를 포함할 수 있습니다.

## Headless 클라이언트 배포

<div class="columns is-multiline">
    <!-- Single-page App (SPA) -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Single-page App (SPA)" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="./spa.md" title="단일 페이지 앱 (SPA)" tabindex="-1">
                       <img class="is-bordered-r-small" src="./assets/spa/spa-card.png" alt="단일 페이지 앱 (SPA)">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="./spa.md" title="단일 페이지 앱 (SPA)">단일 페이지 앱 (SPA)</a></p>
                   <p class="is-size-6">단일 페이지 앱(SPA)의 배포 고려 사항에 대해 알아봅니다.</p>
                   <a href="./spa.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">알아보기</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
<!-- Web component/JS -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web component/JS" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./web-component.md" title="웹 구성 요소/JS" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/web-component/web-component-card.png" alt="웹 구성 요소/JS">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./web-component.md" title="웹 구성 요소/JS">웹 구성 요소/JS</a></p>
               <p class="is-size-6">웹 구성 요소와 브라우저 기반 JavaScript Headless 소비자에 대한 배포 고려 사항에 대해 알아봅니다.</p>
               <a href="./web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">알아보기</span>
               </a>
           </div>
       </div>
   </div>
</div>
<!-- Mobile apps -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Mobile apps" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./mobile.md" title="모바일 앱" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/mobile/mobile-card.png" alt="모바일 앱">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./mobile.md" title="모바일 앱">모바일 앱</a></p>
               <p class="is-size-6">모바일 앱 배포 시 고려사항에 대해 알아봅니다.</p>
               <a href="./mobile.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">알아보기</span>
               </a>
           </div>
       </div>
   </div>
</div>
<!-- Server-to-server apps -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Server-to-server apps" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./server-to-server.md" title="서버 간 앱" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/server-to-server/server-to-server-card.png" alt="서버 간 앱">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./server-to-server.md" title="서버 간 앱">서버 간 앱</a></p>
               <p class="is-size-6">서버 간 앱 배포 시 고려 사항에 대해 알아봅니다.</p>
               <a href="./server-to-server.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">알아보기</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>
