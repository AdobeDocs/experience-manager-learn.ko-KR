---
title: AEM Headless 배포
description: AEM Headless 앱에 대한 다양한 배포 고려 사항에 대해 알아봅니다.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10794
thumbnail: kt-10794.jpg
last-substantial-update: 2022-08-26T00:00:00Z
source-git-commit: 1ecd3c761ea7c79036b263ff8528a6cd01af0e76
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 0%

---


# AEM Headless 배포

AEM Headless 클라이언트 배포는 많은 양식을 사용합니다. AEM에서 호스팅하는 SPA, 외부 SPA, 웹 사이트, 모바일 앱 또는 심지어 서버 간 프로세스까지도 수행할 수 있습니다.

AEM Headless 배포는 클라이언트와 배포 방법에 따라 다른 고려 사항이 있습니다.

## AEM 서비스 아키텍처

배포 고려 사항을 살펴보기 전에 AEM 논리 아키텍처와 AEM as a Cloud Service 서비스 계층의 분리 및 역할을 이해하는 것이 중요합니다. AEM as a Cloud Service 은 두 개의 논리 서비스로 구성됩니다.

+ __AEM 작성자__ 는 팀이 컨텐츠 조각(및 기타 자산)을 만들고, 공동 작업하고, 게시하는 서비스입니다.
+ __AEM 게시__ 게시된 컨텐츠 조각(및 기타 자산)이 일반 소비를 위해 복제되는 서비스입니다.
+ __AEM 미리 보기__ 은 동작에서 AEM Publish를 모방하지만 미리 보기 또는 검토 목적으로 게시된 컨텐츠가 있는 서비스입니다. AEM 미리 보기는 컨텐츠를 일반적으로 전달하는 것이 아니라 내부 대상을 위한 것입니다. 원하는 워크플로우를 기반으로 AEM 미리 보기 사용은 선택 사항입니다.

![AEM 서비스 아키텍처](./assets/overview/aem-service-architecture.png)

일반적인 AEM as a Cloud Service 헤드리스 배포 아키텍처_

프로덕션 용량으로 작동하는 AEM Headless 클라이언트는 일반적으로 승인된 게시된 컨텐츠를 포함하는 AEM Publish와 상호 작용합니다. AEM 작성자와 상호 작용하는 클라이언트는 기본적으로 안전하므로 모든 요청에 대한 인증이 필요하며 진행 중인 작업 또는 승인되지 않은 컨텐츠를 포함할 수도 있으므로 특별한 주의를 기울여야 합니다.

## 헤드리스 클라이언트 배포

<div class="columns is-multiline">
    <!-- Single-page App (SPA) -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Single-page App (SPA)" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="./spa.md" title="단일 페이지 앱(SPA)" tabindex="-1">
                       <img class="is-bordered-r-small" src="./assets/spa/spa-card.png" alt="단일 페이지 앱(SPA)">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="./spa.md" title="단일 페이지 앱(SPA)">단일 페이지 앱(SPA)</a></p>
                   <p class="is-size-6">단일 페이지 앱(SPA)의 배포 고려 사항에 대해 알아봅니다.</p>
                   <a href="./spa.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">학습</span>
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
               <p class="is-size-6">웹 구성 요소 및 브라우저 기반 JavaScript 헤드리스 소비자에 대한 배포 고려 사항에 대해 알아봅니다.</p>
               <a href="./web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">학습</span>
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
               <p class="is-size-6">모바일 앱의 배포 고려 사항에 대해 알아봅니다.</p>
               <a href="./mobile.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">학습</span>
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
               <p class="is-size-6">서버 간 앱의 배포 고려 사항에 대해 알아봅니다</p>
               <a href="./server-to-server.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">학습</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>