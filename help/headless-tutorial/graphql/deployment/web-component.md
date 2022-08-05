---
title: AEM Headless Web Component 배포
description: 웹 구성 요소/순수 JS 기반 AEM Headless 배포에 대한 배포 고려 사항에 대해 알아봅니다.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10797
thumbnail: kt-10797.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 2%

---


# AEM Headless Web Component 배포

AEM Headless [웹 구성 요소](https://developer.mozilla.org/en-US/docs/Web/Web_Components)/JS 배포는 헤드리스 방식으로 AEM에서 컨텐츠를 사용하고 상호 작용하는 웹 브라우저에서 실행되는 순수 JavaScript 앱입니다. 웹 구성 요소/JS 배포는 다음과 다릅니다 [SPA 배포](./spa.md) 에는 강력한 SPA 프레임워크을 사용하지 않으며 모든 웹 사이트의 컨텍스트에 을 포함시켜 AEM의 컨텐츠를 전달합니다.


## 배포 구성

웹 구성 요소/JS 배포를 위해서는 다음 배포 구성을 적소에 배치해야 합니다.

| 웹 구성 요소/JS 앱은에 연결 | AEM Author | AEM 게시 | AEM 미리 보기 |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [디스패처 필터](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [CORS(원본 간 리소스 공유)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [AEM 호스트](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## 웹 구성 요소 예

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
                   <p class="is-size-6">순수 JavaScript로 작성된 예제 웹 구성 요소로서 AEM Headless GraphQL API의 컨텐츠를 사용합니다.</p>
                   <a href="../example-apps/web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">보기 예</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
