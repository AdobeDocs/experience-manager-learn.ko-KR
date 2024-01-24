---
title: AEM as a Cloud Service 캐싱
description: AEM as a Cloud Service 캐싱에 대한 일반 개요.
version: Cloud Service
feature: Dispatcher, Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
exl-id: e76ed4c5-3220-4274-a315-a75e549f8b40
duration: 71
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 0%

---

# AEM as a Cloud Service 캐싱

AEM as a Cloud Service에서는 캐싱을 이해하는 것이 중요합니다. 캐싱에는 시스템 효율성을 높이고 로드 시간을 줄이기 위해 이전에 가져온 데이터를 저장하고 재사용하는 작업이 포함됩니다. 이 메커니즘은 콘텐츠 전달을 크게 가속화하고 웹 사이트 성능을 향상시키며 사용자 경험을 최적화합니다.

AEM as a Cloud Service에는 여러 캐싱 레이어가 있으며, 작성 및 게시 서비스 간에 다른 전략이 있습니다.

![AEM as a Cloud Service 캐싱 개요](./assets/overview/all.png){align="center"}

## AEM 캐싱

AEM as a Cloud Service에는 CDN, AEM Dispatcher 및 선택적으로 고객 관리 CDN을 포함하여 강력하고 구성 가능한 다중 계층 캐싱 전략이 있습니다. 레이어 간의 캐싱을 미세 조정하여 성능을 최적화할 수 있으므로 AEM이 최상의 경험만 제공하도록 할 수 있습니다. AEM에는 Author 및 Publish 서비스에 대한 서로 다른 캐싱 문제가 있습니다. 아래에서 각 서비스에 대한 캐싱 전략을 살펴보십시오.


<div class="columns is-multiline" style="margin-top: 2rem">
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Publish service caching">
    <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./publish.md" title="AEM Publish 서비스" tabindex="-1">
              <img class="is-bordered-r-small" src="./assets/overview/publish-card.png" alt="AEM Publish 서비스 캐싱">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p class="headline is-size-6 has-text-weight-bold"><a href="./publish.md" title="AEM Publish 서비스 캐싱">AEM Publish 서비스 캐싱</a></p>
            <p class="is-size-6">AEM Publish 서비스는 관리되는 CDN 및 AEM Dispatcher를 사용하여 최종 사용자 웹 경험을 최적화합니다.</p>
            <a href="./publish.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">학습</span>
            </a>
          </div>
        </div>
      </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Author service caching">
        <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
            <div class="card-image">
            <figure class="image is-16by9">
                <a href="./author.md" title="AEM 작성자 서비스 캐싱" tabindex="-1">
                <img class="is-bordered-r-small" src="./assets/overview/author-card.png" alt="AEM 작성자 서비스 캐싱">
                </a>
            </figure>
            </div>
            <div class="card-content is-padded-small">
            <div class="content">
                <p class="headline is-size-6 has-text-weight-bold"><a href="./author.md" title="AEM 작성자 서비스 캐싱">AEM 작성자 서비스 캐싱</a></p>
                <p class="is-size-6">AEM Author 서비스는 관리되는 CDN을 사용하여 최적화된 작성 경험을 제공합니다.</p>
                <a href="./author.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">학습</span>
                </a>
            </div>
            </div>
        </div>
    </div>
</div>
