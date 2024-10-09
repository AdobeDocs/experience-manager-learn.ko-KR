---
title: AEM Headless 웹 구성 요소 배포
description: 웹 구성 요소/순수 JS 기반 AEM Headless 배포에 대한 배포 고려 사항에 대해 알아봅니다.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10797
thumbnail: kt-10797.jpg
exl-id: 9d4aab4c-82af-4917-8c1b-3935f19691e6
duration: 31
source-git-commit: 089bcf71f03bdbb6d21337cc23452afb33ce8098
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 2%

---

# AEM Headless 웹 구성 요소 배포

AEM Headless [웹 구성 요소](https://developer.mozilla.org/en-US/docs/Web/Web_Components)/JS 배포는 웹 브라우저에서 실행되고 Headless 방식으로 AEM의 콘텐츠를 소비하고 상호 작용하는 순수 JavaScript 앱입니다. 웹 구성 요소/JS 배포는 강력한 SPA 프레임워크를 사용하지 않으며, AEM의 콘텐츠를 표시하기 위해 모든 웹 사이트의 컨텍스트에 포함되어야 한다는 점에서 [SPA 배포](./spa.md)와 다릅니다.


## 배포 구성

웹 구성 요소/JS 배포를 위해서는 다음 배포 구성이 적용되어야 합니다.

| Web Component/JS 앱이 AEM에 → | AEM Author | AEM 게시 | AEM 미리 보기 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher 필터](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [CORS(원본 간 리소스 공유)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [AEM 호스트](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## 예제 웹 구성 요소

Adobe은 웹 구성 요소의 예를 제공합니다.

<div class="columns is-multiline">
    <!-- Web Component -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web Component" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/web-component.md" title="웹 구성 요소" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/web-component/web-component-card.png" alt="웹 구성 요소">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/web-component.md" title="웹 구성 요소">웹 구성 요소</a></p>
                   <p class="is-size-6">AEM Headless JavaScript API의 콘텐츠를 사용하는 순수 GraphQL으로 작성된 예제 웹 구성 요소입니다.</p>
                   <a href="../example-apps/web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">예제 보기</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
